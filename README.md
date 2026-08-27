<!--
  Copyright 2026 Salesforce, Inc.

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
-->

# Agentforce Commerce Vibes

**AI-powered development assistant for Salesforce B2C Commerce developers**

Agentforce Commerce Vibes is an AI-powered tool available to B2C Commerce developers as an easy-to-install Visual Studio Code extension. Built on the Salesforce Coding Agent Platform and powered by Claude, it brings intelligent coding assistance directly into your development workflow—purpose-built for cartridge development, storefront customization, and B2C Commerce operations.

## What Can Agentforce Commerce Vibes Do?

Use Agentforce Commerce Vibes to enhance developer productivity across all aspects of B2C Commerce development.

### 💬 Chat
Use agentic chat to get development help in natural language. Describe what you want to build, debug, or understand—the agent determines the best approach and works directly in your workspace. Chat supports streaming responses, multi-tab conversations, inline diff review for file changes, and automatic context management for long sessions.

### 📋 Plan Mode
For complex tasks, use Plan Mode to review and approve a structured plan before anything executes. Describe your feature, iterate on the plan in conversation, and click **Approve Plan** to start execution. No files change until you approve.

### 🔍 Diff Review
Every file the agent edits is surfaced for your review with line-by-line diffs. Accept or reject changes file by file, or bulk accept all. Select any file to open a native VS Code diff editor showing exactly what changed.

### 🧰 Skills, MCP Servers, and Rules
Manage everything that shapes how the agent works from a single Toolkit panel.

- **Skills**: Pre-built B2C Commerce skills from the B2C Developer Toolkit, auto-updated on every activation. Toggle skills on or off or create your own. See [Agent Skills & Plugins](https://salesforcecommercecloud.github.io/b2c-developer-tooling/guide/agent-skills.html) in the B2C Developer Toolkit.
- **Model Context Protocol (MCP) Servers**: The B2C Commerce DX MCP server ships out of the box, giving the agent access to tools for cartridge management, deployment, and Commerce API operations. Add custom MCP servers via `.mcp.json`. See [MCP Server](https://salesforcecommercecloud.github.io/b2c-developer-tooling/mcp/) in the B2C Developer Toolkit.
- **Rules**: Persistent developer instructions the agent respects across every session. Store them in `.afv/rules/` and commit to version control so your whole team benefits.

### 🔐 Sign In
Sign in once with your Salesforce credentials. The extension handles authentication and keeps you connected across sessions.

### 🔎 Conversation Search and Export
Search across all your local chat history by keyword. Export any conversation to a Markdown file with timestamps and speaker labels.

## Requirements

- VS Code `^1.101.0`
- Salesforce B2C Commerce account
- `dw.json` in your workspace root
- Node.js `>=22.15.1`

## Get Started

### 1. Install
Install **Agentforce Commerce Vibes** from the VS Code Marketplace and open the Agentforce Commerce Vibes panel in the Activity Bar.

### 2. Complete Setup
The setup screen validates prerequisites before the chat unlocks.

| Step | What it checks |
|------|----------------|
| Connected Commerce Org | Salesforce default org is authorized |
| B2C Commerce CLI | CLI is installed and accessible |
| B2C Commerce Extension | Required VS Code extension is present |
| B2C Commerce Project | Workspace contains a recognized B2C project (SFRA cartridges, PWA Kit v3, or Storefront Next) |

### 3. Authorize Your Salesforce Org
Click the **Connected Commerce Org** step to open the sign-in overlay. Enter your Salesforce org URL—the URL you use to log into Salesforce. The extension opens a terminal to run the Salesforce CLI login command. After authenticating in your browser, click **Re-check** to validate the connection.

**Examples of org URLs:**
- Production: `login.salesforce.com` or `mycompany.my.salesforce.com`
- Sandbox: `mycompany--sandbox1.sandbox.my.salesforce.com`

Alternatively, run the Salesforce CLI command directly in your terminal:

**For production orgs (using default login URL):**
```bash
sf org login web --set-default
```

**For any other org (sandbox, My Domain, or a custom domain):**
```bash
sf org login web --instance-url https://<your-org-url> --set-default
```
Replace `<your-org-url>` with the full URL you use to log into Salesforce (for example, `mycompany--dev.sandbox.my.salesforce.com`).

After all setup steps show as ready, the chat panel unlocks.

### 4. Start Building
The chat panel unlocks. Try:

```
"Create a Storefront Next project with default settings"
"Update the storefront theme colors to match a new brand palette"
"Show me how to call the Product Search API"
"Deploy my SFRA cartridges to sandbox"
```

## Configuration

Manage extension settings from the Agentforce Commerce Vibes Settings panel inside VS Code. In the Agentforce Commerce Vibes activity bar, click the gear icon to access model selection, debug logging, permissions, and other options.

## Troubleshooting

- **Skills not loading** — Run **Agentforce Commerce Vibes: Refresh Commerce Skills** from the Command Palette to re-download the latest skill set.

- **MCP server not connecting** — Open the Toolkit panel, check the MCP tab for the server status, and click **Reconnect**.

- **Capture diagnostics** — Run **Agentforce Commerce Vibes: Copy Diagnostics** from the Command Palette to copy system info and recent events to your clipboard for support.

- **Enable debug logs** — Open the Agentforce Commerce Vibes Settings panel, enable debug logging, then run **Agentforce Commerce Vibes: Open Debug Logs**.

## Privacy and Telemetry

Agentforce Commerce Vibes collects anonymized usage telemetry (extension lifecycle events, feature usage, error rates) to improve the product. No source code, file contents, or personally identifiable information is included. Telemetry follows the [Salesforce Privacy Policy](https://www.salesforce.com/company/privacy/).

To opt out, set `telemetry.telemetryLevel` to `"off"` in your VS Code settings.

## License

This extension is licensed under the [Apache 2.0 License](LICENSE.txt).
