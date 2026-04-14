---
type: procedure
created: 2026-03-20
status: active
slug: ccp-021-slack
tags: [procedure, nexus]
---

# CCP-021 — Slack Workspace MCP with OAuth

## Problema
Developers constantly context-switch to Slack to look up prior discussions, find relevant threads, and check team decisions — losing focus during development. The Slack MCP server exposes workspace messages and channels directly in Claude Code via OAuth, enabling conversational search without leaving the terminal.

## Procedura

### Prerequisites
- Slack workspace access
- Install: `/plugin install slack@claude-plugins-official`
- MCP type: HTTP with built-in OAuth flow (clientId: `1601185624273.8899143856786`, callbackPort: 3118)

### Steps

1. **Install the plugin**:
   ```
   /plugin install slack@claude-plugins-official
   ```

2. **OAuth authentication**: The plugin uses Slack's OAuth flow — no manual token generation needed. On first use, Claude Code opens the OAuth authorization URL. The `.mcp.json` configuration:
   ```json
   {
     "slack": {
       "type": "http",
       "url": "https://mcp.slack.com/mcp",
       "oauth": {
         "clientId": "1601185624273.8899143856786",
         "callbackPort": 3118
       }
     }
   }
   ```

3. **Ensure port 3118 is available** for the OAuth callback before authenticating.

4. **Capabilities**: Search messages across the workspace, read channel contents, access threads, navigate workspace communications while coding.

5. **Usage patterns**:
   ```
   "Find the Slack discussion about the auth refactor"
   "What did the team decide about the database migration?"
   "Search for messages mentioning the payment webhook"
   ```

6. **Re-authentication**: If the OAuth token expires, Claude Code will prompt for re-authentication. The OAuth flow repeats — authorize in browser, callback to port 3118.

7. **Workspace scope**: Access is limited to channels and DMs the authenticated user has access to — standard Slack permission model applies.

### Verification
- [ ] Plugin installs without error
- [ ] OAuth flow completes successfully (browser redirect + callback)
- [ ] Claude can search messages in workspace channels
- [ ] Port 3118 is open for OAuth callback during authentication

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, slack, mcp, oauth, team-communication, v2

## Enforcement Loop
- WHERE: Projects with active Slack team communication
- WHEN: Searching for prior decisions, finding relevant discussions, checking team context
- HOW: Install plugin → complete OAuth → ask Claude to search Slack messages
- CONNECT: CCP-020 (github for repository context alongside Slack discussions)
