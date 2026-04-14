---
type: procedure
created: 2026-03-20
status: active
slug: ccp-004-plugin-dev
tags: [procedure, nexus]
---

# CCP-004 — Plugin Development Toolkit (7 Skills, 3 Agents)

## Problema
Building Claude Code plugins from scratch requires knowledge of multiple systems — hooks, MCP servers, plugin manifests, slash commands, agents, and skills. Without structured guidance, developers produce plugins with poor hook implementations, brittle MCP configs, and unclear triggering — resulting in plugins that fail silently or don't activate when needed.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install plugin-dev@claude-plugins-official`
- 7 skills auto-load when relevant questions are asked (progressive disclosure)

### Steps

1. **Use the guided workflow command for new plugins**:
   ```
   /plugin-dev:create-plugin [optional description]
   ```
   This runs an 8-phase process: Discovery → Component Planning → Detailed Design → Structure Creation → Component Implementation → Validation → Testing → Documentation.

2. **7 available skills — each auto-triggers on relevant questions**:

   - **hook-development**: Covers all hook events (PreToolUse, PostToolUse, Stop, SubagentStop, SessionStart, SessionEnd, UserPromptSubmit, PreCompact, Notification), prompt-based vs command hooks, hook output formats, `${CLAUDE_PLUGIN_ROOT}` for portable paths, security best practices. Triggers: "create a hook", "add a PreToolUse hook", "block dangerous commands"

   - **mcp-integration**: MCP server configuration in `.mcp.json` vs `plugin.json`, all server types (stdio/SSE/HTTP/WebSocket), environment variable expansion, authentication patterns (OAuth/tokens/env vars), tool naming and usage. Triggers: "add MCP server", "configure .mcp.json", "Model Context Protocol"

   - **plugin-structure**: Standard plugin directory structure, `plugin.json` manifest format and all fields, auto-discovery rules, `${CLAUDE_PLUGIN_ROOT}` usage. Triggers: "plugin structure", "plugin.json manifest", "component organization"

   - **plugin-settings**: `.claude/plugin-name.local.md` pattern for per-project config, YAML frontmatter + markdown body structure, bash parsing techniques, flag files for temporarily active hooks. Triggers: "plugin settings", "store plugin configuration", ".local.md files"

   - **command-development**: Slash command structure, YAML frontmatter fields (description, argument-hint, allowed-tools), dynamic arguments, bash execution for context. Triggers: "create a slash command", "command frontmatter"

   - **agent-development**: Agent file structure (YAML frontmatter + system prompt), all frontmatter fields (name/description/model/color/tools), description format with `<example>` blocks for reliable triggering, system prompt design patterns. Triggers: "create an agent", "write a subagent"

   - **skill-development**: Skill structure with SKILL.md, progressive disclosure principle, strong trigger descriptions, bundled resources organization. Triggers: "create a skill", "add a skill to plugin"

3. **Progressive disclosure in all skills**: 3 levels — (1) metadata always loaded (~100 words), (2) SKILL.md body when triggered (<500 lines), (3) bundled resources as needed. Context stays focused.

4. **Utility scripts in hook-development skill**:
   ```bash
   ./validate-hook-schema.sh hooks/hooks.json    # Validate JSON structure
   ./test-hook.sh my-hook.sh test-input.json      # Test before deployment
   ./hook-linter.sh my-hook.sh                    # Lint for best practices
   ```

5. **Plugin manifest structure** (`plugin.json` in `.claude-plugin/` directory):
   ```json
   {
     "name": "my-plugin",
     "description": "What it does",
     "author": { "name": "Name", "email": "email" }
   }
   ```

6. **MCP config in `.mcp.json`** — three main types:
   - stdio: `{"command": "node", "args": ["${CLAUDE_PLUGIN_ROOT}/server.js"]}`
   - HTTP: `{"type": "http", "url": "https://mcp.service.com/api"}`
   - SSE: `{"type": "sse", "url": "https://mcp.service.com/sse"}`

7. **Always use `${CLAUDE_PLUGIN_ROOT}`** for paths in hook commands and MCP args — this makes plugins portable across machines without path hardcoding.

8. **Security-first approach** emphasized across all skills: input validation in hooks, HTTPS for all MCP servers, environment variables for credentials, principle of least privilege in tool permissions.

9. **Testing a plugin locally**:
   ```bash
   cc --plugin-dir /path/to/my-plugin
   ```
   Use `claude --debug` to see skill and hook activation in detail.

10. **After building**: Use `plugin-validator` validation agent, then test in real Claude Code session, then document in README.

### Verification
- [ ] `hook-development` skill triggers when asking about hooks
- [ ] `validate-hook-schema.sh` runs cleanly on `hooks/hooks.json`
- [ ] `${CLAUDE_PLUGIN_ROOT}` is used in all hook command paths
- [ ] All 7 skills are accessible after plugin install
- [ ] `/plugin-dev:create-plugin` runs the full 8-phase workflow

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, plugin-dev, hooks, mcp, agents, skills, v2

## Enforcement Loop
- WHERE: When developing new Claude Code plugins
- WHEN: Structuring plugin files, implementing hooks, configuring MCP, writing agents or skills
- HOW: Install plugin-dev toolkit; ask natural language questions to trigger the appropriate skill
- CONNECT: CCP-005 (skill-creator), CCP-008 (hookify), CCP-031 (example-plugin)
