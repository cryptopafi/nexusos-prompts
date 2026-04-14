---
type: procedure
created: 2026-03-20
status: active
slug: ccp-018-telegram
tags: [procedure, nexus]
---

# CCP-018 — Telegram Bot MCP Bridge with Access Control

## Problema
Claude Code is terminal-bound. A Telegram bot integration enables interaction with running Claude Code sessions from any device via Telegram, without building custom server infrastructure. Unlike Discord, Telegram bots can receive DMs without a shared server invite, making setup simpler for single-user cases.

## Procedura

### Prerequisites
- Bun installed: `curl -fsSL https://bun.sh/install | bash`
- Telegram account
- Install: `/plugin install telegram@claude-plugins-official`

### Steps

1. **Create a bot with BotFather**:
   - Open chat with @BotFather on Telegram → send `/newbot`
   - Provide: display name (anything, spaces allowed), username (must end in `bot`)
   - BotFather returns a token like `123456789:AAHfiqksKZ8...` — copy the entire string including the leading number and colon

2. **Install the plugin and configure the token**:
   ```
   /plugin install telegram@claude-plugins-official
   /telegram:configure 123456789:AAHfiqksKZ8...
   ```
   Writes `TELEGRAM_BOT_TOKEN=...` to `.claude/channels/telegram/.env`. Shell environment variable takes precedence.

3. **Relaunch with channel flag** (required):
   ```
   claude --channels plugin:telegram@claude-plugins-official
   ```

4. **Pair your Telegram account**:
   - DM your bot on Telegram — it replies with a 6-character pairing code
   - In Claude Code session: `/telegram:access pair <code>`
   - Your next DM reaches the Claude session

5. **Lock down access**:
   ```
   /telegram:access policy allowlist
   ```
   Prevents strangers from getting pairing-code replies.

6. **Tools exposed to Claude** via the MCP bridge:

   | Tool | Purpose |
   |------|---------|
   | `reply` | Send to a chat. Takes `chat_id` + `text`. Optional: `reply_to` (message ID), `files` (absolute paths, max 50MB each). Images (`.jpg`/`.png`/`.gif`/`.webp`) send as photos with preview; other types send as documents. |
   | `react` | Add emoji reaction using Telegram's fixed whitelist only (👍 👎 ❤ 🔥 👀 etc). |
   | `edit_message` | Edit a bot-sent message in place. |

7. **Key difference from Discord**: Telegram has NO `fetch_messages` tool and NO `download_attachment` for historical messages. Bot API exposes no history or search — the bot only sees messages as they arrive. If context from earlier messages is needed, ask the user to paste or summarize.

8. **Inbound photo handling**: Photos are downloaded eagerly to `~/.claude/channels/telegram/inbox/` when they arrive (since there's no way to fetch them later). The local path is included in the `<channel>` notification. Telegram compresses photos — send as document (long-press → Send as File) to preserve original quality.

9. **Typing indicator**: Automatically shown while Claude processes — Telegram displays "botname is typing…".

10. **Access control details**: See `ACCESS.md` in the plugin for group support, mention detection, delivery config, skill commands, and the full `access.json` schema. User IDs are numeric — get yours from @userinfobot.

### Verification
- [ ] Bot token saved to `.claude/channels/telegram/.env`
- [ ] Session launched with `--channels plugin:telegram@claude-plugins-official`
- [ ] DM to bot triggers 6-character pairing code response
- [ ] After `/telegram:access pair <code>`, next DM reaches Claude session
- [ ] Photos sent to bot are saved to `~/.claude/channels/telegram/inbox/`

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, telegram, mcp, messaging, channel-protocol, v2

## Enforcement Loop
- WHERE: Projects where Claude Code should be accessible via Telegram
- WHEN: Single-user remote access; mobile interaction with running Claude Code sessions
- HOW: Create BotFather bot → configure token → relaunch → pair → allowlist
- CONNECT: CCP-017 (discord for alternative with history support), CCP-019 (fakechat for local testing)
