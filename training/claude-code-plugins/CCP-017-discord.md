---
type: procedure
created: 2026-03-20
status: active
slug: ccp-017-discord
tags: [procedure, nexus]
---

# CCP-017 — Discord Bot MCP Bridge with Access Control

## Problema
Claude Code sessions are locked to the terminal. There's no way to interact with a running Claude Code session from a mobile device or shared platform. Building a custom Discord bot integration requires significant boilerplate — webhook handling, message routing, authentication, attachment management — that distracts from the actual Claude Code workflow.

## Procedura

### Prerequisites
- Bun installed: `curl -fsSL https://bun.sh/install | bash`
- Discord Developer Portal account
- Install: `/plugin install discord@claude-plugins-official`

### Steps

1. **Create a Discord application and bot**:
   - Go to Discord Developer Portal → New Application
   - Navigate to Bot → enable "Message Content Intent" (required — without it, bot receives empty content)
   - Copy the bot token (shown once)

2. **Invite the bot to a server** (required even for DM-only use):
   - OAuth2 → URL Generator → select `bot` scope
   - Enable permissions: View Channels, Send Messages, Send Messages in Threads, Read Message History, Attach Files, Add Reactions
   - Open the generated URL and add bot to a server you're in

3. **Install the plugin and configure the token**:
   ```
   /plugin install discord@claude-plugins-official
   /discord:configure MTIz...
   ```
   Writes `DISCORD_BOT_TOKEN=...` to `.claude/channels/discord/.env`. Shell environment variable takes precedence over the file.

4. **Relaunch with channel flag** (required — server won't connect without it):
   ```
   claude --channels plugin:discord@claude-plugins-official
   ```

5. **Pair your Discord account**:
   - DM your bot on Discord — it replies with a pairing code
   - In your Claude Code session: `/discord:access pair <code>`
   - Your next DM reaches the Claude session

6. **Lock down access after pairing**:
   ```
   /discord:access policy allowlist
   ```
   Prevents strangers from getting pairing-code replies. This is the `allowlist` policy — only paired users can interact.

7. **Tools exposed to Claude** via the MCP bridge:

   | Tool | Purpose |
   |------|---------|
   | `reply` | Send to a channel. Takes `chat_id` + `text`. Optional: `reply_to` (message ID for threading), `files` (absolute paths, max 10 files, 25MB each). Auto-chunks long messages. |
   | `react` | Add emoji reaction to any message by ID. Unicode emoji work directly; custom emoji need `<:name:id>` form. |
   | `edit_message` | Edit a bot-sent message. Used for "working…" → result progress updates. |
   | `fetch_messages` | Pull recent channel history (oldest-first, max 100). Messages with attachments marked `+Natt`. |
   | `download_attachment` | Download attachments from a specific message to `~/.claude/channels/discord/inbox/`. |

8. **Attachment handling**: Attachments are NOT auto-downloaded. The `<channel>` notification lists each attachment's name, type, and size. Claude calls `download_attachment(chat_id, message_id)` explicitly when needed.

9. **Inbound typing indicator**: Automatically shown while Claude processes — Discord displays "botname is typing…".

10. **Access control details**: See `ACCESS.md` in the plugin for DM policies, guild channel setup, mention detection, delivery config, skill commands, and the full `access.json` schema. IDs are Discord snowflakes (numeric) — enable Developer Mode, right-click → Copy ID.

### Verification
- [ ] Bot token is saved to `.claude/channels/discord/.env`
- [ ] Session launched with `--channels plugin:discord@claude-plugins-official`
- [ ] DM to bot triggers pairing code response
- [ ] After `/discord:access pair <code>`, next DM reaches Claude session
- [ ] `reply` tool sends message back to the Discord DM

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, discord, mcp, messaging, channel-protocol, v2

## Enforcement Loop
- WHERE: Projects where Claude Code should be accessible via Discord
- WHEN: Setting up remote access to Claude Code; team or multi-device access
- HOW: Follow 5-step setup (create bot → configure token → relaunch → pair → lock down)
- CONNECT: CCP-018 (telegram for alternative messaging), CCP-019 (fakechat for local testing)
