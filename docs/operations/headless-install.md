# Headless install: Docker, CI, postinstall

As of v0.37, `gbrain init --pglite` in a non-TTY context (Docker `RUN`, CI step, postinstall hook) exits 1 when no embedding-provider API key is present in the environment. This is a deliberate fail-loud — the alternative was the v0.36 silent-broken-state class where init succeeded with a default that didn't match any real key.

Two patterns work for headless installs. Pick whichever fits your image lifecycle.

## Pattern 1: Provider key available at image build time

If your CI / Docker pipeline can inject the API key as a build-time env var, set it before `gbrain init`:

```dockerfile
# Multi-stage Dockerfile sketch
FROM oven/bun:1 AS builder

# Inject key at build via --build-arg or `--env` from CI.
ARG OPENAI_API_KEY
ENV OPENAI_API_KEY=$OPENAI_API_KEY

RUN bun install -g github:garrytan/gbrain
RUN gbrain init --pglite  # auto-picks OpenAI, persists config
```

```yaml
# GitHub Actions equivalent
- name: Initialize gbrain
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
  run: |
    bun install -g github:garrytan/gbrain
    gbrain init --pglite
```

Init writes `~/.gbrain/config.json` with the resolved `embedding_model` + `embedding_dimensions`. Subsequent runs (in the same image / runner) read from that config and don't re-resolve.

## Pattern 2: Provider key only at runtime (deferred-setup)

If the API key is a runtime secret, use `--no-embedding` at build time and
recreate the empty PGLite database with the real model and dimensions when the
container starts:

```dockerfile
FROM oven/bun:1
RUN bun install -g github:garrytan/gbrain

# Build the brain shape without a provider. The schema lands at the default
# width, but no embed callsite will actually run until runtime config.
RUN gbrain init --pglite --no-embedding

# At container start, provide the real provider and rebuild the empty schema:
ENTRYPOINT ["/bin/sh", "-c", "\
  gbrain reinit-pglite \
    --embedding-model openai:text-embedding-3-large \
    --embedding-dimensions 1536 \
    --yes --no-sync \
  && exec gbrain serve"]
```

The `gbrain init --no-embedding` opt-in writes `embedding_disabled: true` to
config. Embed callsites refuse to run instead of choosing a silent default.

The runtime `gbrain reinit-pglite` command:

- Preserves the previous empty database as `<path>.bak`.
- Rebuilds the schema at the requested vector size.
- Persists the explicit embedding provider and dimensions.

Do not use this deferred pattern after importing content. Choose the provider
before import, or follow the normal `reinit-pglite` backup and resync flow.

## What will not work

```dockerfile
# Don't do this — silent default leaves you with vector(1280) ZE column
# and 1536d OpenAI provider at runtime, mismatched.
RUN gbrain init --pglite
```

If you upgrade an older image that used this pattern, run `gbrain doctor` first.
Repair PGLite with `gbrain reinit-pglite`; use
[`../embedding-migrations.md`](../embedding-migrations.md) for Postgres.

## Verifying a headless install

After init, run `gbrain doctor --json` to verify state:

```bash
gbrain doctor --json | jq '.checks[] | select(.name=="embedding_provider")'
```

The `embedding_provider` check returns `status: 'ok'` when:

- Config has a persisted `embedding_model`.
- Config has a persisted `embedding_dimensions`.
- Live provider probe returns the configured dim.
- DB column width matches.

If you used Pattern 2's deferred-setup path, the check shows `Skipped (no provider credentials)` until the runtime config is populated. That's expected.
