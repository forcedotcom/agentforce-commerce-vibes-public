# Change Log

## 1.0.3


### Fixed
- Marketplace Q&A on the Agentforce Commerce Vibes listing now opens the troubleshooting guide.

## 1.0.2


### Fixed
- Installing a dependency from the setup checklist no longer drops you out of the checklist and into chat mid-install — the checklist stays open through the recheck that runs after the click.

### Enhancements
- Clicking the **B2C Commerce CLI** row automatically refreshes the checklist once `npm install -g @salesforce/b2c-cli` finishes, so the row flips to Ready without a manual Recheck.
- The post-auth checklist footer now shows **Start Chat** in place of Dismiss, so returning to chat reads as a positive next step rather than a dismissal.

## 1.0.1


### Enhancements

**Setup Checklist — Dependency Installation**
- **B2C Commerce CLI**: clicking the row now runs `npm install -g @salesforce/b2c-cli` in a reusable VS Code terminal instead of opening the docs page, so the CLI installs (and upgrades in place) with visible progress.
- **CLI update detection**: the row now emits `needs-update` when the installed CLI is older than the npm registry's `latest`; the same click upgrades to the latest version.
- **B2C Commerce Extension**: clicking the row now invokes VS Code's native marketplace install flow via `workbench.extensions.installExtension` instead of opening the GitHub releases page.

### Changed
- Marketplace Q & A now opens the Agentforce Commerce Vibes troubleshooting guide

### Fixed
- Scaffolding a new Storefront Next project now asks only for the folder name in chat, then lets the CLI prompt for vertical and Commerce Cloud settings in the terminal
- Previewing a Storefront Next project recovers from common first-install pnpm errors (stale lockfile verification and blocked dependency build scripts)

## 1.0.0


**General Availability Release** 🎉

Agentforce Commerce Vibes is now generally available! This AI-powered development assistant brings intelligent coding support directly into your B2C Commerce workflow, purpose-built for cartridge development, storefront customization, and Commerce operations.

### Features

**💬 Agentic Chat**
- Natural language development assistance with streaming responses
- Multi-tab conversation management with automatic context handling
- Direct workspace integration for building, debugging, and understanding code

**📋 Plan Mode**
- Review and approve structured execution plans before any files change
- Iterate on plans through conversation before committing to implementation
- Clear visibility into what the agent will do before it acts

**🔍 Diff Review**
- Line-by-line file change review with native VS Code diff editor
- Accept or reject changes individually or in bulk
- Full transparency on every file modification

**🧰 Unified Toolkit Panel**
- Pre-built B2C Commerce skills from the B2C Developer Toolkit, auto-updated on activation
- B2C Commerce DX MCP server included out of the box for cartridge management and Commerce APIs
- Persistent developer rules stored in `.afv/rules/` and committed to version control

**🔐 Commerce Cloud Authentication**
- One-time sign-in with Salesforce credentials
- Persistent session across VS Code restarts
- Support for production orgs, sandboxes, and custom domains

**🔎 Conversation Management**
- Search across all local chat history by keyword
- Export conversations to Markdown with timestamps and speaker labels
- Recent chats quick-access for fast context switching

## 0.3.7


### Bug Fixes
- Fixed SHA-256 verification to properly handle all platform VSIXs (darwin-arm64, darwin-x64, linux-arm64, linux-x64, win32-x64)
- Store and verify platform-specific hashes instead of single hash for all platforms

## 0.3.3


### Bug Fixes
- Fixed CHANGELOG fetch from private repo in marketplace publishing workflow
- Updated repository_dispatch payload to include changelog section

