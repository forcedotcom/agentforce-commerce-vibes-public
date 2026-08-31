# Change Log

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

