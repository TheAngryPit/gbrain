# Docs consolidation PR proposal

Status: fresh PR handoff in progress. The previous PR was closed because local
analysis caches made its diff unreviewable. This proposal excludes those files.

## Proposed title

v0.42.64.0 docs: consolidate install and operating paths (#2211)

## Proposed body

### Summary

This is a docs-only consolidation pass for GBrain operational documentation. It
does not change runtime behavior.

This is a clean replacement for #2212. It excludes `.codegraph/`,
`.understand-anything/`, and other local analysis state.

The branch:

- adds a changelog-derived current-capabilities ledger;
- refreshes the documentation inventory and status taxonomy;
- makes README a router with current version, human install, agent install,
  production, architecture, and LLM entrypoints;
- rebuilds `docs/INSTALL.md` as the canonical human install and operating
  guide;
- adds route-based install journeys for local personal, personal multi-source,
  thin-client, shared/production, and advanced topology setups;
- gives the local human route a complete first result: install, initialize,
  create a sample note, import it, search it, and verify the output;
- separates provider-backed initialization from the supported keyword-only
  `--no-embedding` path and its later `reinit-pglite` transition;
- updates `INSTALL_FOR_AGENTS.md` as the canonical agent protocol;
- separates operating models from deployment topologies;
- adds a central Brain Repo Layout guide;
- adds a Mode Selection Guide for `search`, `think`, `dream`/autopilot, and
  push context;
- adds a production/shared-brain path and checklist;
- aligns MCP/auth/remote/thin-client docs with current OAuth/scopes/localOnly
  behavior;
- corrects DCR documentation to the consent-bearing default and removes a
  brittle count of `localOnly` operations;
- corrects OAuth scope examples to the current space-separated CLI contract and
  documents the protected-onboard OAuth limitation without promising an
  ungrantable scope;
- adds `--public-url` guidance where remote OAuth clients use a public issuer
  URL;
- clarifies that protected shell jobs are trusted host-side CLI submissions,
  not remote MCP operations;
- keeps thin-client agent guidance off local stdio `gbrain serve`;
- limits mounted-brain examples to currently supported `gbrain mounts`
  management commands;
- labels selected historical/design/superseded docs;
- removes or qualifies brittle skill/test/generated-map count claims;
- adds the human install guide directly to the machine-readable documentation
  index and regenerates `llms.txt` and `llms-full.txt`;
- adds a final consistency report mapping PRD acceptance, changelog-current
  capabilities, operator feedback, and issues #2 through #14.

### Key artifacts

- `docs/docs-consolidation/05-current-capabilities-ledger.md`
- `docs/docs-consolidation/06-documentation-status-taxonomy.md`
- `docs/docs-consolidation/09-final-consistency-report.md`
- `README.md`
- `docs/INSTALL.md`
- `INSTALL_FOR_AGENTS.md`
- `docs/architecture/topologies.md`
- `docs/architecture/brain-repo-layout.md`
- `docs/guides/mode-selection.md`
- `docs/mcp/DEPLOY.md`
- `SECURITY.md`
- `llms.txt`
- `llms-full.txt`

### Validation

- `bun run build:llms`
- focused `bun test test/build-llms.test.ts` with unrelated suite preloads
  disabled
- `git diff --check`
- current source-contract review through CodeGraph
- fresh upstream check confirming version `0.42.64.0`
- fresh repository-wide `understand-anything` structural scan: 2,678 files,
  8,664 nodes, 15,683 edges, 9 layers, 6 tour steps, and 0 validation issues
- final `openclaw-autoreview` result will be copied from
  `docs/docs-consolidation/09-final-consistency-report.md` after it runs

### Proof limits

- Static documentation/source inspection only.
- No GBrain runtime command was run.
- No local GBrain brain home, corpus path, runtime config, Hermes/OpenClaw,
  Hindsight, or Nexus runtime was inspected.
- No service, Docker lifecycle, migration, import, sync, or dependency install
  was run.

## Review checklist

- [ ] README routes readers without duplicating operational detail.
- [ ] `docs/INSTALL.md` is acceptable as the Human Operational Center.
- [ ] `INSTALL_FOR_AGENTS.md` preserves the search-mode user gate and remote
      safety boundaries.
- [ ] Operating model and deployment topology are distinct enough for personal,
      family/team/company, thin-client, split-engine, and security-driven
      setups.
- [ ] Production checklist covers provider/base URL gotchas, keys, backups,
      OAuth/scopes, MCP exposure, health, and failure verification.
- [ ] Historical/design/superseded labels are useful without over-labeling.
- [ ] `llms.txt` and `llms-full.txt` generated-map guidance is acceptable for
      upstream and fork users.
- [ ] The final diff contains no `.codegraph/`, `.understand-anything/`, or
      local-analysis ignore entries.
