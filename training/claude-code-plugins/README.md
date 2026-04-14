---
type: procedure
created: 2026-03-20
status: active
slug: readme
tags: [procedure, nexus]
---

# Claude Code Plugins — Training Domain

**Domeniu**: `claude-code-plugins`
**Creat**: 2026-03-20
**Scope**: Official Claude Code plugins from anthropics/claude-plugins-official — development tools, code review, messaging bridges, integrations, LSPs
**Sursa**: github.com/anthropics/claude-plugins-official (MIT License, Anthropic 2026)
**Proceduri**: 31 (CCP-001 → CCP-031)

## Sub-domenii

### Core Development (CCP-001 → CCP-014)
Feature development, code review, plugin creation, skill building, hooks, automation

### Output Styles (CCP-015 → CCP-016)
Educational and interactive learning modes via SessionStart hooks

### Channel/Messaging (CCP-017 → CCP-019)
Discord, Telegram, and localhost chat bridges for Claude Code

### External Integrations (CCP-020 → CCP-029)
GitHub, Slack, Stripe, Linear, Asana, Supabase, Firebase, Context7, Playwright, Serena

### Language Servers (CCP-030)
Consolidated LSP setup for 12 programming languages

### Meta/Reference (CCP-031)
Example plugin structure and patterns

## Procedure Index

| ID | Plugin | Description |
|----|--------|-------------|
| CCP-001 | feature-dev | 7-phase feature development with parallel code-explorer, code-architect, code-reviewer agents |
| CCP-002 | code-review | 4-agent PR review with confidence scoring (0-100), filters issues below 80 |
| CCP-003 | pr-review-toolkit | 6 specialized agents: comment-analyzer, pr-test-analyzer, silent-failure-hunter, type-design-analyzer, code-reviewer, code-simplifier |
| CCP-004 | plugin-dev | 7 skills + 3 agents + /plugin-dev:create-plugin 8-phase workflow |
| CCP-005 | skill-creator | Draft → eval → iterate loop with benchmarking and description optimization |
| CCP-006 | claude-code-setup | Read-only codebase analysis → tailored MCP/skills/hooks/subagents recommendations |
| CCP-007 | claude-md-management | claude-md-improver skill (audit) + /revise-claude-md command (session capture) |
| CCP-008 | hookify | Markdown rule files → hooks without JSON; /hookify command; regex patterns; warn/block actions |
| CCP-009 | ralph-loop | Stop hook intercepts exit → re-injects prompt → iterates until completion-promise |
| CCP-010 | commit-commands | /commit (auto-message), /commit-push-pr (full workflow), /clean_gone (branch cleanup) |
| CCP-011 | playground | Self-contained HTML playgrounds; 4 templates: design, data-explorer, concept-map, document-critique |
| CCP-012 | frontend-design | Anti-AI-slop skill; bold aesthetic choices; auto-activates for frontend work |
| CCP-013 | security-guidance | PreToolUse hook; 9 security patterns; session-scoped dedup; ENABLE_SECURITY_REMINDER env var |
| CCP-014 | code-simplifier | Opus agent; preserves functionality; reduces nesting/redundancy; applies CLAUDE.md conventions |
| CCP-015 | explanatory-output-style | SessionStart hook; ★ Insight blocks; codebase-specific educational notes |
| CCP-016 | learning-output-style | SessionStart hook; interactive contributions at decision points + explanatory insights |
| CCP-017 | discord | Bun MCP server; pairing flow; allowlist policy; reply/react/edit_message/fetch_messages/download_attachment |
| CCP-018 | telegram | Bun MCP server; pairing flow; no history/search; eager photo download |
| CCP-019 | fakechat | Localhost HTTP chat UI; dev tool only; no access control; `FAKECHAT_PORT` env var |
| CCP-020 | github | HTTP MCP; Bearer token auth; full GitHub API access |
| CCP-021 | slack | HTTP MCP; OAuth flow; clientId + callbackPort 3118 |
| CCP-022 | stripe | HTTP MCP + best-practices skill + /explain-error + /test-cards commands |
| CCP-023 | linear | HTTP MCP; issues/projects/cycles management |
| CCP-024 | asana | SSE MCP; persistent connection; task and project management |
| CCP-025 | supabase | HTTP MCP; database/auth/storage/edge functions |
| CCP-026 | firebase | stdio via npx firebase-tools; Firestore/Auth/Functions/Hosting |
| CCP-027 | context7 | stdio via npx @upstash/context7-mcp; version-specific library docs |
| CCP-028 | playwright | stdio via npx @playwright/mcp; browser automation and E2E testing |
| CCP-029 | serena | stdio via uvx from GitHub; LSP-based semantic analysis and refactoring |
| CCP-030 | LSP setup | 12 languages: TypeScript, Go, Rust, Python, Java, Kotlin, C/C++, C#, Ruby, Swift, Lua, PHP |
| CCP-031 | example-plugin | Reference implementation: plugin.json schema, skills/ vs commands/ formats, ${CLAUDE_PLUGIN_ROOT} |

## Key Patterns

- **plugin.json manifest schema**: `name`, `description`, `author` in `.claude-plugin/plugin.json`
- **3 MCP types**: stdio (local process), HTTP (REST), SSE (Server-Sent Events)
- **Hook events**: PreToolUse, PostToolUse, Stop, SubagentStop, SessionStart, SessionEnd, UserPromptSubmit, PreCompact, Notification
- **Channel protocol**: `notifications/claude/channel` (Discord, Telegram, fakechat share same protocol)
- **`${CLAUDE_PLUGIN_ROOT}`**: Always use for portable paths in hook commands and MCP args
- **Parallel agent architectures**: feature-dev (3+3+3 agents), code-review (4 agents), pr-review-toolkit (6 agents)
- **Progressive disclosure**: metadata (always) → SKILL.md (on trigger) → bundled resources (as needed)
- **Skills vs Commands**: `skills/<name>/SKILL.md` is preferred; `commands/*.md` is legacy but still valid; loaded identically
- **Confidence scoring**: 0-100 scale in code-review agents; filter at 80 threshold to reduce false positives
- **Pairing flow**: Discord and Telegram both use 6-char pairing code → `/discord:access pair <code>` or `/telegram:access pair <code>`
- **Session-scoped state**: security-guidance hook deduplicates warnings per session using `~/.claude/security_warnings_state_{session_id}.json`

## Installation Pattern

All official plugins install via:
```
/plugin install {plugin-name}@claude-plugins-official
```

Browse available plugins:
```
/plugin > Discover
```

Test locally during development:
```bash
claude --plugin-dir /path/to/plugin
```
