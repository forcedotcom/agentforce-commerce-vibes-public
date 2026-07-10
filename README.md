<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Commerce Vibes v2](#commerce-vibes-v2)
  - [New to v2?](#new-to-v2)
  - [Quick Links](#quick-links)
  - [Prerequisites](#prerequisites)
  - [First-run commands](#first-run-commands)
  - [Packages](#packages)
  - [AI Skills](#ai-skills)
  - [AI Rules](#ai-rules)
  - [AI Agents](#ai-agents)
  - [AI Commands](#ai-commands)
  - [Development](#development)
  - [Testing](#testing)
  - [Contributing](#contributing)
  - [Branch Model](#branch-model)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Commerce Vibes v2

Commerce Cloud development assistant built on Agentforce Vibes v4 (AFV) architecture.
Replaces Commerce Vibes v1.0 (Cline-based) with the ADX agent harness from
`@salesforce/sfdx-agent-sdk` — using `MastraHarnessFactory` for LLM Gateway
integration — in a Turborepo monorepo. See
[`docs/decisions/agentic-dx-adoption.md`](docs/decisions/agentic-dx-adoption.md)
for the harness-adoption ADR.

## New to v2?

Commerce Vibes v2 is built on the AFV v4 architecture, customized for
**Salesforce B2C Commerce Cloud only** (no Salesforce Platform support).
It uses Commerce Cloud authentication (AM OAuth + ECOM token exchange) and
B2C Commerce-specific tools, skills, and MCP servers.

**Clone and build:**

Clone into a separate directory so v2's `node_modules` and build artifacts
don't conflict when you switch back to work on `master` (v1.0):

```bash
# Clone v2.0 branch (AFV v4-based) into separate directory
git clone -b feature/afv-v4.0-clean git@github.com:SalesforceCommerceCloud/commerce-vibes-builder.git commerce-vibes-v2
cd commerce-vibes-v2
corepack enable && pnpm install && pnpm build
```

**Branch structure**:
- `master` — Commerce Vibes v1.0 (Cline/3.x-based, npm, different architecture)
- `feature/afv-v4.0-clean` — Commerce Vibes v2.0 (AFV v4-based, pnpm + Turborepo monorepo)

Keep them in separate directories to avoid `node_modules` and build artifact conflicts.

**Read in this order:**

1. [docs/overview.md](docs/overview.md) -- architecture, roadmap, scope decisions
2. [docs/developing.md](docs/developing.md) -- commands, conventions, dependency management, logging policy, spec/story-driven workflows
3. The `spec.md` in whichever package you'll work in first (e.g., [packages/core/spec.md](packages/core/spec.md))

**Commerce Vibes v2 Migration:** Key changes from v1.0:
- Replace Salesforce authentication with Commerce Cloud (AM OAuth + ECOM token exchange)
- Replace Salesforce DX MCP with B2C Commerce DX MCP
- Replace Salesforce skills with Commerce skills (via B2C CLI)
- Remove all Salesforce Platform dependencies (SObject context, SFDX project detection)
- Update system prompts for Commerce domain (cartridges, ISML, OCAPI, Business Manager)

**Open a PR:** all PRs target `feature/afv-v4.0-clean`. Use the `afv-pr` skill to
get the base branch and conventions right.

## Quick Links

- **Overview**: [docs/overview.md](docs/overview.md) -- AFV v4 architecture (base for Commerce Vibes v2)
- **Developing**: [docs/developing.md](docs/developing.md) -- commands, conventions, workflows, principles
- **ADRs**: [docs/decisions/](docs/decisions/) -- architecture decision records
- **v3 → v4 subsystem changes**: [docs/v3-to-v4-subsystem-changes.md](docs/v3-to-v4-subsystem-changes.md) -- per-subsystem migration reference
- **Testing**: [docs/testing/](docs/testing/) -- parity matrix, telemetry parity, cross-boundary error scenarios
- **Feature specs**: each package and subsystem has a co-located `spec.md` (e.g., [packages/core/spec.md](packages/core/spec.md), [packages/core/src/prompts/spec.md](packages/core/src/prompts/spec.md))

## Prerequisites

- **Node.js** >= 24
- **pnpm** >= 10 — enabled via corepack (ships with Node): `corepack enable`

`nvm` is not required — `pnpm` auto-manages Node version via `.npmrc`. The `.nvmrc` is provided for convenience and CI.

## First-run commands

```bash
pnpm install      # Install all dependencies
pnpm run build    # Build all packages
pnpm run all      # Full validation: install → build → lint → test
```

Full command reference — including `dev`, `test`, `lint:*`, per-package turbo
filters, and extension bundling — lives in
[docs/developing.md > Common Commands](docs/developing.md#common-commands).

## Packages

| Package | Description |
|---------|-------------|
| `@afv/core` | Agent types, SDK adapters, tools, hooks, services, observability (unchanged from AFV v4) |
| `@afv/features` | Feature logic: MCP (B2C Commerce DX), skills (Commerce), abilities (sub-path exports) |
| `@afv/webview` | Chat UI assembly, shared components, config UIs (Commerce-branded) |
| `@afv/extension` | VS Code shell: activation, **Commerce Cloud auth**, project detection. **Published name**: `commerce-vibes` |
| `@afv/e2e` | Trace-based end-to-end tests |
| `@afv/azure-insights` | KQL queries, Bicep templates, codegen |

## AI Skills

Skills are interactive workflows in `.claude/skills/` that work in both
Cursor and Claude Code. Invoke by name or description.

| Skill | When to use |
|-------|-------------|
| `afv-feature-implement` | Starting a new feature, porting a 3.x subsystem, or doing foundational `@afv/core`/extension/shared-infra work — walks through plan, scaffold, implement, and verify (features are the primary track; foundation deltas are called out in the skill) |
| `afv-refine-wi` | Fleshing out a skeletal work item from `docs/v4-implementation-plan.md` into an implementation-ready spec (Why block, Description, Acceptance Criteria, Assumptions) |
| `afv-pr` | Opening or updating a PR — targets `afv-v4.0` with correct base branch and conventions; auto-detects create vs update for the current branch |
| `afv-merge` | Pulling `afv-v4.0` into a long-running feature branch — proactive structural rebasing, merge execution, post-merge audit, forward-fix, and adaptation-gap sweep. Pairs with `afv-pr` (the merge does not ship — `afv-pr` does) and `afv-spec-sync` (spec drift surfaced during alignment). |
| `afv-storybook-prototype` | UX work — creating a skeleton-first Storybook component with mock data variants |
| `afv-storybook-implement` | Eng work — wiring a UX prototype into a production component with real data |
| `afv-debug` | Verifying a code change in the running extension — tails `afv.log` and `trace.md` around a manual probe and reports a PASS/FAIL verdict against a checklist derived from the change. Pairs with `afv-feature-implement`'s implement → bundle → reload → tail-verify cycle. |
| `afv-spec-sync` | After implementation changes — checks spec.md files for drift against source code |
| `afv-release` | Release-process questions — versioning, alpha/beta/GA promotion, marketplace publishing, cutover, rollback, nightly pipeline. Also flags documentation gaps when found. |
| `afv-changelog` | Drafting v4 pre-release notes — generates a curated `CHANGELOG.md` block from commits since the last pre-release tag for the release engineer to review before a cut. Feeds the notes the pipeline ships on each GitHub Release. |
| `afv-declare-feature-dependency` | Declaring that one `@afv/features` feature may import from another — edits the importer's Biome GritQL deny-list plugin under `scripts/biome-plugins/`. Name-only invocation (model-triggered description matching is disabled). |

## AI Rules

Rules are auto-applied guidance in `.claude/rules/` that trigger when
matching files are edited. They enforce conventions without requiring
explicit invocation.

| Rule | Applies when editing |
|------|---------------------|
| `afv-spec-lint` | `docs/spec-template.md`, `docs/spec-frontmatter.schema.json`, `**/spec.md` — required headings, audience-appropriate summary, and forbidden sections. Propagates template/schema changes across all specs. (Frontmatter shape is enforced by the schema via `pnpm lint:md:integrity`.) |
| `afv-spec-hygiene` | `**/spec.md` — keeps specs focused on design intent, not volatile implementation details |
| `afv-arch-docs` | Package / dependency graph / CoreConfig / biome.jsonc / convention-doc files — enforces matching updates to `docs/overview.md`, `docs/developing.md`, and per-package `biome.jsonc` |

## AI Agents

Specialized subagents in `.claude/agents/` that the model can dispatch
for focused, deep-context work. Each has its own system prompt, model,
and a description that controls when the parent agent should delegate.

| Agent | When the parent should dispatch |
|-------|--------------------------------|
| `afv-abilities-feature-expert` | Working with, modifying, or debugging the Abilities feature across `@afv/features` and `@afv/extension` |
| `afv-telemetry-expert` | Telemetry work — writing KQL, modifying Observer sinks, working with `@afv/azure-insights`, analyzing event flows |

## AI Commands

Commands in `.claude/commands/` are explicit multi-step workflows invoked
by name (unlike skills, they don't trigger on natural-language phrasing).

| Command | What it does |
|---------|-------------|
| `afv-update-dev-deps` | Updates dev dependencies in small batches from the `pnpm-workspace.yaml` catalog, running install/lint/build/test after each batch so regressions surface incrementally |

## Development

**F5 to debug:** press F5 to launch an Extension Development Host with the
esbuild watcher running. The bundler rebuilds `dist/extension.js` on every
save. Press **Cmd+R** (Ctrl+R) in the dev host window to reload with your
changes. Breakpoints in `packages/extension/src/` work through sourcemaps.

**`pnpm dev` for type-checking:** runs a single root `tsc -b --watch` with
correct cross-package rebuild ordering. Run it in a terminal alongside F5
to get real-time type errors while developing. esbuild skips type errors
for fast bundling, so a type error won't block the F5 debug cycle — but
`pnpm dev` will catch it.

## Testing

| Script | What it does |
|---|---|
| `pnpm test` | Full unit + e2e replay-mode sweep. The CI lane. |
| `pnpm test:e2e` | E2E specs only, replay mode (uses `*.fixture.json` siblings, no network, no auth). |
| `pnpm test:e2e:live` | Re-runs e2e specs against real LLMG. Requires `SFDX_AUTH_URL` (CI) or `SF_TARGET_ORG` (dev laptop, after `sf org login`). |

See [docs/developing.md > Testing](docs/developing.md#testing) for the
full table including `test:watch`, `test:unit`, `test:e2e:record` and `AFV_E2E_MODE` dispatch.

## Contributing

All PRs target `feature/afv-v4.0-clean` (not `main`). Use the `afv-pr`
skill to open or update PRs with the correct base branch.

## Branch Model

This is the `feature/afv-v4.0-clean` branch -- built on AFV v4 architecture,
customized for Commerce Cloud only.

**Key differences from Agentforce Vibes v4**:
- **Commerce Cloud authentication** (AM OAuth + ECOM token exchange) instead of Salesforce org auth
- **B2C Commerce DX MCP server** instead of Salesforce DX
- **Commerce skills** instead of Salesforce skills
- **No Salesforce Platform support** (SObject context, SFDX project, Extension Pack removed)
- **Commerce-specific system prompts** (cartridges, ISML, OCAPI, Business Manager)

See [docs/overview.md](docs/overview.md) for the AFV v4 architecture that Commerce Vibes v2 is built on.
