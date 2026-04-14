---
type: procedure
created: 2026-03-20
status: active
slug: ccp-031-example-plugin
tags: [procedure, nexus]
---

# CCP-031 — Example Plugin (Reference Implementation for Plugin Structure)

## Problema
When developing new Claude Code plugins, developers lack a canonical reference showing the correct directory layout, manifest schema, skill format, command format, and how these components differ. Without a working reference, plugin structure errors are only caught at runtime.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install example-plugin@claude-plugins-official` (or study the source directly)
- Source: `plugins/example-plugin/` in the claude-plugins-official repository

### Steps

1. **Full plugin directory structure** (standard layout):
   ```
   plugin-name/
   ├── .claude-plugin/
   │   └── plugin.json          # Plugin metadata (REQUIRED)
   ├── .mcp.json                # MCP server configuration (optional)
   ├── skills/
   │   ├── skill-name/
   │   │   └── SKILL.md         # Model-invoked skill (preferred format)
   │   └── command-name/
   │       └── SKILL.md         # User-invoked skill / slash command
   └── commands/
       └── command-name.md      # Legacy slash command format (still valid)
   ```

2. **`plugin.json` manifest schema** (minimum required fields):
   ```json
   {
     "name": "plugin-name",
     "description": "What this plugin does",
     "author": {
       "name": "Author Name",
       "email": "author@example.com"
     }
   }
   ```

3. **Skills directory — preferred format for all components**: Create `skills/<name>/SKILL.md`:

   **Model-invoked skill** (Claude activates based on task context):
   ```yaml
   ---
   name: skill-name
   description: This skill should be used when the user asks to "specific phrase",
     mentions "keyword", or discusses topic-area.
   version: 1.0.0
   ---
   ```
   Body: instructions Claude follows when the skill is active.

   **User-invoked skill** (slash command — `/skill-name`):
   ```yaml
   ---
   name: skill-name
   description: Short description shown in /help
   argument-hint: <required-arg> [optional-arg]
   allowed-tools: [Read, Glob, Grep]
   ---
   ```

4. **Commands directory — legacy format** (still works, loaded identically to skills):
   ```markdown
   ---
   description: Short description shown in /help
   argument-hint: <arg1> [optional-arg]
   allowed-tools: [Read, Glob, Grep, Bash]
   ---

   # Command Title
   Instructions for Claude when this command runs.
   The user invoked this with: $ARGUMENTS
   ```
   The ONLY difference between `commands/*.md` and `skills/<name>/SKILL.md` is file layout. For new plugins, prefer the `skills/` directory format.

5. **MCP server configuration in `.mcp.json`** (three main types):
   ```json
   // HTTP (hosted REST API)
   { "server-name": { "type": "http", "url": "https://mcp.example.com/api" } }

   // SSE (hosted, real-time)
   { "server-name": { "type": "sse", "url": "https://mcp.example.com/sse" } }

   // stdio (local process)
   { "server-name": { "command": "node", "args": ["${CLAUDE_PLUGIN_ROOT}/server.js"] } }
   ```

6. **`${CLAUDE_PLUGIN_ROOT}` variable**: Always use this for paths in hook commands and MCP args. It resolves to the plugin's installation directory at runtime, making plugins portable across machines.

7. **YAML frontmatter fields for skills/commands**:
   - `name` (required): Identifier for the skill
   - `description` (required): Trigger conditions (model-invoked) or help text (user-invoked)
   - `argument-hint`: Shown to user when invoking as slash command
   - `allowed-tools`: Pre-approved tools (reduces permission prompts)
   - `model`: Override model ("haiku", "sonnet", "opus")
   - `version`: Semantic version

8. **Hook events available**: PreToolUse, PostToolUse, Stop, SubagentStop, SessionStart, SessionEnd, UserPromptSubmit, PreCompact, Notification

9. **Plugin installation**:
   - From marketplace: `/plugin install plugin-name@claude-plugins-official`
   - Local development: `claude --plugin-dir /path/to/plugin`
   - Browse: `/plugin > Discover`

10. **Auto-discovery**: Claude Code auto-discovers all components in the standard directories. No registration required — placing a `SKILL.md` in `skills/<name>/` is sufficient.

### Verification
- [ ] `plugin.json` exists in `.claude-plugin/` directory with `name` and `description`
- [ ] Skills in `skills/<name>/SKILL.md` are discoverable (test with a trigger phrase)
- [ ] Slash command works via `/plugin-name:command-name` or `/command-name`
- [ ] `${CLAUDE_PLUGIN_ROOT}` resolves correctly in hook commands
- [ ] Plugin loads without error via `claude --plugin-dir /path/to/plugin`

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, plugin-structure, reference, manifest, skills, commands, v2

## Enforcement Loop
- WHERE: All new plugin development
- WHEN: Starting a new plugin, checking component format, troubleshooting plugin loading
- HOW: Compare new plugin structure against this reference; verify manifest schema; use `skills/` format for new components
- CONNECT: CCP-004 (plugin-dev for full guided development), CCP-005 (skill-creator for skill writing)
