# Final docs-only consistency report

Status: issue #14 final pass, content complete; publication checks in progress

Proof level: `code_proven` with static/source inspection. Fresh CodeGraph and
`understand-anything` proof are complete. No GBrain
runtime command, local brain home, local corpus path, service, migration,
import, sync, or Docker lifecycle command was run.

## Inputs checked

- GitHub PRD issue: <https://github.com/TheAngryPit/gbrain/issues/1>
- GitHub final-pass issue: <https://github.com/TheAngryPit/gbrain/issues/14>
- Operator feedback comment by `Ben-Home` on issue #1.
- `docs/docs-consolidation/05-current-capabilities-ledger.md`
- `docs/docs-consolidation/06-documentation-status-taxonomy.md`
- `docs/docs-consolidation/03-issue-breakdown.md`
- Current branch diff against `upstream/master`.
- Fresh `git fetch upstream --prune` on 2026-07-23, confirming
  `upstream/master` at `a356f64e4f36c6f3dd9251c7127e557fc161c7cd`
  and version `0.42.64.0`.
- Source review of all six post-release reverts on current `master`.
- Landed docs and generated maps listed below.
- CodeGraph source exploration and a read-only source-contract review.
- `unslop`, `deslop`, `teach`, and the current Vercel writing guidelines as
  editorial and teaching checks.

## Post-review corrections

The first final report found the broad consolidation shape, but follow-up review
found concrete install and auth issues that needed patching before PR handoff:

| Finding | Resolution | Proof |
| --- | --- | --- |
| Six source reverts landed on `master` after the latest `0.42.64.0` changelog entry. | Rebased onto `a356f64e`, reviewed every revert, and treated current source as authority where it differs from the release ledger. | `git log` and source diff for `a356f64e`, `a1dadebd`, `c43ed81c`, `1d0df706`, `8078c46a`, and `e20a6a53` |
| The autopilot install path assumed the generated wrapper made Bun available to scheduled processes. | Added the non-interactive PATH prerequisite to the human path, agent protocol, and architecture index. | Source read of `writeWrapperScript()` in `src/commands/autopilot.ts` |
| Lens-pack docs promised that one dream schedule automatically carried newly extracted atoms into concept synthesis. | Documented that current extraction does not assign `concepts` labels and that zero-yield pages can remain in the atom backlog. | Source reads of `src/core/cycle/extract-atoms.ts`, `synthesize-concepts.ts`, and `extract-atoms-drain.ts` |
| Human remediation-plan output can contradict its own unreachable-target result. | Directed humans and agents to the stable JSON fields and blocked prerequisites. | Source read of `runRemediationPlan()` in `src/commands/doctor.ts` |
| `docs/INSTALL.md` was a reference page more than a route-by-route install journey. | Added Routes A-E at the top of `docs/INSTALL.md`. Route A now takes a human from prerequisites through a sample note, import, search, expected results, and the optional semantic-retrieval step. | docs inspection plus pedagogical review |
| The first local route ran plain `gbrain init --pglite` before a provider choice. That command can fail in non-interactive use with no provider. | Split the human journey into a provider-backed path and a deterministic `--no-embedding` keyword-only path. Added the supported `reinit-pglite` transition for adding embeddings later. | CodeGraph trace to `src/commands/init.ts`, `src/commands/init-provider-picker.ts`, and the deferred-setup E2E tests |
| The docs called `balanced` the fresh-install default. | Documented the installer recommendation separately from the internal unset-config fallback. Fresh init normally recommends `tokenmax`, or `conservative` for Haiku-class/no-OpenAI setups; an unset config falls back to `balanced`. | CodeGraph trace to `src/commands/init-mode-picker.ts` and `src/core/search/mode.ts` |
| The agent flow offered `--no-embedding` but always ran `gbrain embed --stale` afterward. | Split Step 4 into keyword-only and provider-backed outcomes, each with its own observable verification. | source and docs inspection |
| README still carried a competing install quickstart. | Reduced README install content to a router that points to the canonical install routes and agent protocol. | docs inspection |
| The machine-readable map did not list the canonical human install guide directly. | Added `docs/INSTALL.md` to `scripts/llms-config.ts` and regenerated both LLM maps. | generator inspection and focused test |
| Several OAuth examples used comma-separated scopes. | Replaced public docs examples with OAuth space-separated scope strings, for example `--scopes "read write"`. | CodeGraph trace to `src/commands/auth.ts` and `src/core/scope.ts` |
| Mounted-brain example omitted the explicit engine flag. | Updated the topology example to include `--engine postgres` with `--db-url`; follow-up fidelity review confirmed current `mounts` management includes `add`, `list`, `remove`, `enable`, `disable`, `trust-frontmatter`, and `untrust-frontmatter` despite an older file header still mentioning `enable/disable` as future work. | CodeGraph trace to `src/commands/mounts.ts` dispatcher and help text |
| Protected onboard scope was documented as grantable through OAuth, but current allowlist does not expose it. | Updated `INSTALL_FOR_AGENTS.md` to say protected onboard handlers stay skipped over OAuth today and require trusted local CLI operation until the registration path exposes the grant. | CodeGraph trace to `src/core/operations.ts`, `src/core/scope.ts`, and `src/core/oauth-provider.ts` |
| Remote OAuth examples used public client URLs without setting the server issuer to that URL. | Added `--public-url` to thin-client host, MCP deploy, tunnel alternatives, ChatGPT, agent, and company-brain examples, plus notes that the value must match the URL used for OAuth discovery. | CodeGraph trace to `src/commands/serve-http.ts` |
| Agent guide mixed HTTP MCP setup with worker-side shell-job opt-in. | Removed `GBRAIN_ALLOW_SHELL_JOBS=1` from the HTTP server example and documented that protected shell jobs require trusted host-side CLI submission, not remote MCP. | CodeGraph trace to `src/core/operations.ts` |
| Thin-client route still pointed at the local stdio MCP section. | Updated Route C and the agent connection section to say thin-client homes must not run local `gbrain serve`; agents either call the thin-client CLI or connect directly to the host `/mcp` endpoint. | docs inspection plus thin-client dispatch contract |
| Mounted-brain docs advertised generic `gbrain query --brain` before CLI dispatch support is documented. | Replaced the query example with supported `gbrain mounts` management commands, updated the brain/source mental model and agent routing convention, qualified schema-pack examples that implied transparent mounted-brain querying, and marked query-time cross-brain dispatch as an integration/agent responsibility until documented by the command layer. | CodeGraph trace to `src/cli.ts`, `src/core/operations.ts`, `src/commands/mounts.ts`, and brain resolver source |
| The advanced install route linked to an unstable mounted-brains heading anchor. | Added a stable `topology-mounted-cross-team-brains` anchor before the mounted/cross-team topology heading. | docs inspection |
| The inventory manifest looked live after later branch edits. | Labeled it as a frozen baseline snapshot before consolidation edits. | docs inspection |
| The multi-source guide still spoke from the v0.18 planning moment and taught `--source a,b` as if a comma-separated source list were supported. | Reframed sources as current infrastructure, updated the command list to the current dispatcher, replaced the source-list example with one-source-per-invocation guidance, and moved absent `sources prune` / `sources import-from-github` commands into an explicit current-boundaries note. | CodeGraph trace to `src/core/source-resolver.ts`, `src/commands/sources.ts`, and `src/cli.ts` search dispatch |
| Several live docs still used old release-roadmap wording as current guidance. | Removed stale version promises from plugin-author docs, brain/source architecture, schema-pack boundaries, PDF skill prerequisites, sync troubleshooting, and takes-quality eval guidance. | CodeGraph trace to `src/core/minions/plugin-loader.ts`, `src/core/schema-pack/per-source.ts`, `src/commands/eval-takes-quality.ts`, `src/core/takes-quality-eval/replay.ts`, plus current `CHANGELOG.md` sync entries |
| `SECURITY.md` still described open DCR as granting `client_credentials` by default. | Updated the warning to the current consent-bearing `authorization_code` default and the explicit `--enable-dcr-insecure` opt-in. | CodeGraph trace to `src/core/oauth-provider.ts` |
| The ChatGPT guide said exactly four operations were `localOnly`. | Replaced the brittle count with examples and named `src/core/operations.ts` as authority. | CodeGraph trace to the current operation registry |

The current `understand-anything` pass scanned and structurally extracted all
2,674 included files, regenerated the import and call graph, rebuilt the layers
and tour, and produced a 2,674-file fingerprint baseline. Validation found zero
duplicate IDs, missing file paths, dangling edges, or broken layer/tour
references. The stale semantic graph from an older commit was not retained.
This is structural static analysis, not a fresh file-by-file LLM semantic
review. All local Understand and CodeGraph state will be removed from the
publishable diff.

## PRD acceptance map

| PRD acceptance bar | Landed evidence | Status |
| --- | --- | --- |
| New user can start from README and find one correct human install/operating path. | `README.md` routes installation to `docs/INSTALL.md`; Route A lists prerequisites, offers provider-backed or keyword-only setup, creates a sample note, verifies exact search, and explains the semantic upgrade. | pass |
| Coding agent can start from README or `AGENTS.md` and find one correct agent operating path. | `README.md` routes coding agents to `INSTALL_FOR_AGENTS.md`, `AGENTS.md`, and `CLAUDE.md`; `INSTALL_FOR_AGENTS.md` contains the agent operating map and safety gates. | pass |
| Production operators have a central path covering topology, safety boundaries, brain repo layout, mode selection, credentials, backups, and failure modes. | Route D in `docs/INSTALL.md` points production/shared readers to the production branch, which covers shape, modes, MCP exposure, scopes, providers/secrets, backups/restore, health, and failure-mode checklist. | pass |
| Operating Model and Deployment Topology are distinct and branchable. | `docs/architecture/topologies.md` separates operating model from deployment topology and links from `README.md`, `docs/INSTALL.md`, and `INSTALL_FOR_AGENTS.md`. | pass |
| Shared Group Brain includes family/team/company use cases without inventing separate topology families unnecessarily. | `docs/architecture/topologies.md`, `docs/INSTALL.md`, and `INSTALL_FOR_AGENTS.md` treat family, household, team, and company as shared group brain operating models. | pass |
| `search`, `think`, and `dream`/autopilot are explained through a mode-selection guide. | `docs/guides/mode-selection.md` covers retrieval, synthesis, maintenance, push context, and search-mode bundles; README and install docs route to it. | pass |
| Historical/design/superseded docs are visibly labeled. | `docs/docs-consolidation/07-status-label-application.md` tracks labels applied to selected historical/design/proposal/incident/plan docs and `TODOS.md`. | pass |
| `CHANGELOG.md` is treated as the Release Evolution Ledger. | `docs/docs-consolidation/05-current-capabilities-ledger.md` derives the baseline from `CHANGELOG.md`; `docs/adr/0001-centralize-operational-documentation.md` preserves the authority decision. | pass |
| Generated `llms.txt` and `llms-full.txt` are promoted as machine-readable entrypoints. | `README.md`, `AGENTS.md`, `INSTALL_FOR_AGENTS.md`, `scripts/llms-config.ts`, `scripts/build-llms.ts`, `llms.txt`, and `llms-full.txt`. | pass |
| PR remains docs-only and does not mutate runtime behavior. | Branch diff is docs, repo-local docs support, generated LLM maps, and documentation generator source. No `src/`, runtime migrations, package manifests, or tests were modified. | pass after local analysis state is removed before publication |

## Changelog-current capability map

| Capability group from ledger | Docs now explaining or routing to it |
| --- | --- |
| Current version and release evolution | `README.md`, `VERSION`, `CHANGELOG.md`, `docs/docs-consolidation/05-current-capabilities-ledger.md` |
| Push-based context: retrieval reflex, `volunteer_context`, `gbrain volunteer-context`, `gbrain watch` | `docs/guides/mode-selection.md`, `docs/guides/push-context.md`, `README.md`, `docs/INSTALL.md`, `INSTALL_FOR_AGENTS.md` |
| Search, synthesis, dream/autopilot, and search-mode cost/quality choices | `docs/guides/mode-selection.md`, `docs/INSTALL.md`, `INSTALL_FOR_AGENTS.md` |
| Brain/source routing, source-local write-through, repo layout, managed/generated surfaces, backup implications | `docs/architecture/brain-repo-layout.md`, `docs/architecture/brains-and-sources.md`, `docs/INSTALL.md`, `INSTALL_FOR_AGENTS.md` |
| Operating models: personal, multi-source, shared group, single-agent shared, auth-scoped shared | `docs/architecture/topologies.md`, `docs/INSTALL.md`, `INSTALL_FOR_AGENTS.md` |
| Deployment topologies: local/default, remote/thin client, split-engine/worktree, mounted/cross-team, security-driven isolation | `docs/architecture/topologies.md`, `docs/architecture/thin-client.md`, `docs/INSTALL.md`, `docs/mcp/DEPLOY.md` |
| PGLite locking, single-writer caution, resumable sync, failure/retry posture | `docs/INSTALL.md`, `docs/architecture/topologies.md`, `docs/TESTING.md`, `docs/GBRAIN_VERIFY.md` |
| Provider/base URL gotchas, key management, model/search cost guardrails | `docs/INSTALL.md`, `INSTALL_FOR_AGENTS.md`, `docs/guides/mode-selection.md`, provider references linked from install docs |
| OAuth 2.1, scopes, legacy bearer compatibility, remote MCP exposure, localOnly boundaries | `docs/mcp/DEPLOY.md`, `SECURITY.md`, `docs/mcp/ALTERNATIVES.md`, `docs/architecture/thin-client.md`, `INSTALL_FOR_AGENTS.md`, client-specific MCP docs |
| Relational retrieval and current retrieval architecture | `README.md`, `docs/architecture/RETRIEVAL.md`, `docs/guides/mode-selection.md` |
| Production health, doctor/remediation, backups, restore proof, queue/minion failure signals | `docs/INSTALL.md`, `docs/GBRAIN_VERIFY.md`, `docs/guides/minions-deployment.md`, references routed from install docs |
| Brittle skill/test/generated-map count drift | `docs/docs-consolidation/08-brittle-counts-and-generated-maps.md`, `README.md`, `CLAUDE.md`, `docs/TESTING.md`, generated map source |

## Operator feedback map

| Feedback from issue #1 | Landed resolution |
| --- | --- |
| Single "getting started in production" path. | Implemented as the production/shared-brain branch inside `docs/INSTALL.md`, not a separate overlapping `OPERATING.md`. |
| Search vs chat vs dream scattered across files. | Implemented as `docs/guides/mode-selection.md`, using current GBrain surfaces: `search`, `think`, `dream`/autopilot, retrieval reflex, volunteer context, and watch. |
| Vault directory structure undocumented. | Implemented as `docs/architecture/brain-repo-layout.md`; uses "Brain Repo Layout" as the generic term and reserves "vault" for compatibility/import contexts. |
| Status badges on historical docs. | Implemented through selected banners tracked in `docs/docs-consolidation/07-status-label-application.md`. |
| Production checklist should cover base URL gotcha, keys, backup strategy. | Implemented in `docs/INSTALL.md` production branch and failure-mode checklist. |
| Local glossary and ADR from grill session should be included. | `CONTEXT.md` and `docs/adr/0001-centralize-operational-documentation.md` are present in the branch. |

## Issue ledger

| Issue | Slice | Evidence | Status |
| --- | --- | --- | --- |
| #2 | Changelog-to-current-capabilities baseline | `docs/docs-consolidation/05-current-capabilities-ledger.md` | pass |
| #3 | Inventory and status taxonomy | `docs/docs-consolidation/06-documentation-status-taxonomy.md`, manifest files | pass |
| #4 | README router, current version, LLM entrypoints | `README.md`, regenerated LLM maps | pass |
| #5 | Human Operational Center | `docs/INSTALL.md` route map plus detailed install/reference sections | pass |
| #6 | Agent Operational Center | `INSTALL_FOR_AGENTS.md`, `AGENTS.md`, regenerated LLM maps | pass |
| #7 | Operating model and topology trees | `docs/architecture/topologies.md` plus README/install/agent routes | pass |
| #8 | Brain Repo Layout | `docs/architecture/brain-repo-layout.md` plus routes | pass |
| #9 | Mode Selection Guide | `docs/guides/mode-selection.md` plus routes | pass |
| #10 | Production operational path and checklist | `docs/INSTALL.md` production/shared branch | pass |
| #11 | MCP/auth/remote/thin-client alignment | `docs/mcp/DEPLOY.md`, `SECURITY.md`, `docs/mcp/ALTERNATIVES.md`, `docs/architecture/thin-client.md`, MCP client docs | pass |
| #12 | Status labels | `docs/docs-consolidation/07-status-label-application.md` and selected banners | pass |
| #13 | Brittle counts and generated maps | `docs/docs-consolidation/08-brittle-counts-and-generated-maps.md`, generator updates, regenerated LLM maps | pass |
| #14 | Final consistency pass | This report | pass; publication checks remain |

## Branch scope check

`git diff --name-only upstream/master...HEAD` shows documentation, generated LLM
maps, docs-consolidation artifacts, repo-local documentation support, and docs
generator source. The final pass found no changes under:

- `src/`
- `test/` or `tests/`
- `migrations/`
- `package.json`
- `bun.lock`
- Docker compose files
- runtime config templates

No runtime behavior change was introduced by this branch.

## Static validation

| Check | Result |
| --- | --- |
| CodeGraph-first exploration before raw search on final pass | pass |
| GitHub issue #1 and #14 bodies read | pass |
| GitHub issue #1 comments read, including operator feedback | pass |
| `bun run build:llms` after generator/content changes | pass: `llms.txt` 8,589 bytes; `llms-full.txt` 300,948 bytes |
| `git diff --check` / staged whitespace checks on implementation slices | pass |
| Added local Markdown links and anchors | pass: 175 checked, 0 broken |
| Targeted stale-count search for #13 phrases | pass |
| Secret-pattern scan over staged #13 diff | pass |
| `bun test test/build-llms.test.ts` | pass with a temporary Bun config that disables unrelated repository-wide test preloads; no dependency install was performed |
| Fresh CodeGraph index after the final source review | pass: index up to date at 2,136 files, 23,539 nodes, and 120,474 edges |
| Fresh repository-wide `understand-anything` pass | pass: 2,674 files, 8,644 nodes, 17,288 edges, 9 layers, 6 tour steps, 0 validation issues, and a 2,674-file fingerprint baseline |
| Final `openclaw-autoreview` pass | pending final content freeze |

## Proof limits

- This is static documentation/source inspection only.
- No GBrain runtime command was run.
- No local GBrain brain home, corpus path, runtime config, Hermes/OpenClaw, or
  Nexus runtime was inspected.
- No service was started.
- No dependency install was performed in this refresh.
- The first generated-doc test attempt was blocked by the repository-wide test
  preload importing an unavailable `ai` package in the clean worktree. The
  focused test passed with those unrelated preloads disabled.

## Strongest safe truth

The docs-only consolidation branch now centralizes human operation in
`docs/INSTALL.md`, agent operation in `INSTALL_FOR_AGENTS.md`, branchable topology in
`docs/architecture/topologies.md`, brain layout in
`docs/architecture/brain-repo-layout.md`, mode selection in
`docs/guides/mode-selection.md`, MCP/auth truth in `docs/mcp/DEPLOY.md` plus
linked security/client docs, and LLM ingestion in generated maps.

The proof is not runtime proof. Final generated-map regeneration,
local-analysis cleanup, and `openclaw-autoreview` remain before publication.
