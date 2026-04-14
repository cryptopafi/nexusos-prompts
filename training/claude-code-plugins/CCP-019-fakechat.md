---
type: procedure
created: 2026-03-20
status: active
slug: ccp-019-fakechat
tags: [procedure, nexus]
---

# CCP-019 — Fakechat (Localhost Test Chat for Channel Development)

## Problema
Building and testing the Discord or Telegram channel integration requires a real bot account and internet connectivity. This creates friction during development — changes require redeployment, errors are harder to debug, and testing requires real credentials. A localhost alternative allows testing the channel protocol without any external service.

## Procedura

### Prerequisites
- Bun installed: `curl -fsSL https://bun.sh/install | bash`
- Install: `/plugin install fakechat@claude-plugins-official`
- No external accounts or credentials required

### Steps

1. **Install and relaunch with channel flag**:
   ```
   /plugin install fakechat@claude-plugins-official
   claude --channels plugin:fakechat@claude-plugins-official
   ```
   The channel flag is required — the server won't start without it.

2. **Find the URL**: On startup, the server prints the URL to stderr:
   ```
   fakechat: http://localhost:8787
   ```

3. **Open the URL in a browser**: A simple chat UI loads. Type a message → message goes to your Claude Code session → Claude replies in-thread.

4. **Change the default port** if 8787 is in use:
   ```bash
   FAKECHAT_PORT=8080 claude --channels plugin:fakechat@claude-plugins-official
   ```

5. **Tools exposed to Claude** (minimal set matching the channel protocol):

   | Tool | Purpose |
   |------|---------|
   | `reply` | Send text to the UI. Optional: `reply_to` (message ID), `files` (absolute path, 50MB). Attachments appear as `[filename]` under the text. |
   | `edit_message` | Edit a previously-sent message in place. |

6. **File handling**: Inbound images/files save to `~/.claude/channels/fakechat/inbox/` and the path is included in the notification. Outbound files are copied to `outbox/` and served over HTTP.

7. **Limitations — this is a dev tool, not a messaging bridge**:
   - No history (fresh on every browser reload)
   - No search
   - No `access.json` or access control
   - No skill commands
   - Single browser tab only
   - Not suitable for production use

8. **Typical development workflow**:
   - Develop and test channel behavior with fakechat (no credentials needed)
   - Verify the `reply`, `edit_message`, and file handling work as expected
   - When channel logic is confirmed, switch to real Discord or Telegram plugin
   - Use fakechat again when debugging issues in the channel implementation

9. **Protocol reference**: Fakechat implements the `notifications/claude/channel` protocol — the same protocol used by Discord and Telegram channel plugins. It's the minimal reference implementation of how channel notifications reach Claude Code.

### Verification
- [ ] Server starts and prints URL to stderr on launch
- [ ] Typing in browser and pressing send delivers message to Claude session
- [ ] Claude's `reply` tool sends text back visible in the browser chat
- [ ] File sent in browser is saved to `~/.claude/channels/fakechat/inbox/`
- [ ] `FAKECHAT_PORT=8080` changes the port correctly

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, fakechat, channel-dev, localhost, testing, v2

## Enforcement Loop
- WHERE: Plugin development sessions building channel integrations
- WHEN: Testing channel protocol behavior before deploying real Discord/Telegram bots
- HOW: Install fakechat → relaunch with channel flag → test in browser → iterate without credentials
- CONNECT: CCP-017 (discord, production channel), CCP-018 (telegram, production channel)
