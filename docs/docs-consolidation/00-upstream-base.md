# GBrain upstream base

Status: upstream documentation consolidation baseline

## Git

- Current branch: codex/docs-consolidate-operational-v2
- Pinned upstream commit: 1a449bf5015e8ff33af966d9f108a0b0e81a6da9
- Current upstream version: 0.42.64.0

## Current release baseline

The consolidation baseline was refreshed after upstream advanced past the first
inventory pass. The changelog is now the first authority for documentation
drift checks: docs should be compared against the current release series before
rewrites are proposed.

Latest release entries reviewed:

- `0.42.64.0` - confidential OAuth clients can revoke their own tokens while
  invalid or mixed authentication fails closed.
- `0.42.63.0` - schema commands respect a configured custom PGLite database
  path.
- `0.42.62.0` - multi-source identity, reconnect, nested source scanning,
  source-scoped writes, LiteLLM routing, agent-bound auth clients, and
  cross-platform installation were hardened. Existing multi-source brains
  require a one-time `gbrain extract all`.
- `0.42.61.0` - autopilot lock recovery, deterministic atom slugs, takes
  bootstrap progress, schema-pack atom scope, and citation timeline extraction.
- `0.42.60.0` - Windows sync safety, non-Anthropic job resume, source isolation,
  cache policy, and safer non-interactive admin output.
- `0.42.59.0` - migration recovery, multi-source engine migration, pipe-safe
  facts, ambiguous-name quarantine, and source-scoped `think`.
- `0.42.58.0` - local provider, LiteLLM, llama-server, Ollama, custom base URL,
  and embedding-dimension reliability.
- `0.42.57.0` - PGLite lock corruption prevention and the
  `gbrain reinit-pglite` recovery path.
- `0.42.56.0` - Life Chronicle and migrations `v121` and `v122`.
- `0.42.55.0` - security hardening, migration `v120`, and consent-bearing DCR
  as the default.

## Remotes

```
origin	https://github.com/TheAngryPit/gbrain.git (fetch)
origin	https://github.com/TheAngryPit/gbrain.git (push)
upstream	https://github.com/garrytan/gbrain.git (fetch)
upstream	https://github.com/garrytan/gbrain.git (push)
```

## Safety statement

No local Nexus runtime commands were run.

This fork/project is for upstream GBrain documentation analysis only.

Forbidden in this project:
- no local GBRAIN_HOME inspection
- no local GBRAIN_CORPUS_PATH inspection
- no Hermes/OpenClaw/Hindsight runtime inspection
- no gbrain init
- no gbrain sync
- no gbrain serve
- no gbrain auth
- no migrations
- no service starts/stops/restarts
