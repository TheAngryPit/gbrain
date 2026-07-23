# GBrain upstream base

Status: upstream documentation consolidation baseline

## Git

- Current branch: codex/docs-consolidate-operational-v2
- Pinned upstream commit: 0bd752b3f72eae72728f9091e12b3b7bfa4f5cbd
- Current upstream version: 0.42.64.0

## Current release baseline

The consolidation baseline was refreshed after upstream advanced past the first
inventory pass. The changelog is the first release-evolution ledger for
documentation drift checks. Current source is authoritative when `master`
changes after the latest release entry.

The pinned commit contains 13 source commits after the `0.42.64.0` changelog
entry. Six are reverts: the autopilot Bun PATH fix, `reference/` wikilink
extraction, automatic concept labels from `extract_atoms`, zero-yield atom
tombstones, the doctor human-output fix for unreachable targets, and the
OpenRouter reranker recipe touchpoint. Seven later commits add or correct:

- metered and configurable `extract_atoms` model spend;
- source-scoped orphan candidates during normal cycles, with an explicit
  global maintenance override;
- single-file frontmatter slug validation relative to the brain repo;
- Matryoshka dimension lookup for provider-prefixed OpenAI-compatible models;
- a chat-only `claude-cli` OAuth recipe with subagent support;
- idempotent resubmission after `dead` or `cancelled` jobs;
- rejection of unknown `gbrain init` flags before migration work begins.

This docs pass reviewed all 13 commits against current operational claims.

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
