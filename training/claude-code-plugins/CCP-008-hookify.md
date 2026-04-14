---
type: procedure
created: 2026-03-20
status: active
slug: ccp-008-hookify
tags: [procedure, nexus]
---

# CCP-008 — Hookify (Rule Engine for Blocking Dangerous Patterns)

## Problema
Creating Claude Code hooks normally requires editing complex `hooks.json` files with precise JSON schemas. This is error-prone and inaccessible to developers who just want to prevent a specific behavior — like blocking `rm -rf` commands or warning about hardcoded secrets — without learning the full hooks API.

## Procedura

### Prerequisites
- Claude Code installed
- Python 3.7+ (no external dependencies — uses stdlib only)
- Install: `/plugin install hookify@claude-plugins-official`
- Rules are stored as `.local.md` files in `.claude/` — no restart needed when adding rules

### Steps

1. **Create a rule from a natural language description**:
   ```
   /hookify Warn me when I use rm -rf commands
   /hookify Don't use console.log in TypeScript files
   ```
   Claude analyzes the request and creates `.claude/hookify.rule-name.local.md` with the correct YAML frontmatter and message body.

2. **Analyze conversation to find patterns** (no-argument mode):
   ```
   /hookify
   ```
   Claude scans the recent conversation for behaviors you've corrected or expressed frustration about, then proposes appropriate rules.

3. **Rule file format** — YAML frontmatter + message body:
   ```markdown
   ---
   name: block-dangerous-rm
   enabled: true
   event: bash
   pattern: rm\s+-rf
   action: block
   ---

   Warning message shown to Claude when rule triggers.
   ```

4. **Action types**:
   - `warn` — shows warning but allows the operation to proceed (default)
   - `block` — prevents the operation from executing (PreToolUse) or stops the session (Stop events)

5. **Event types**:
   - `bash` — triggers on Bash tool commands; field: `command`
   - `file` — triggers on Edit, Write, MultiEdit; fields: `file_path`, `new_text`, `old_text`, `content`
   - `stop` — triggers when Claude attempts to stop
   - `prompt` — triggers on user prompt submission; field: `user_prompt`
   - `all` — triggers on all events

6. **Pattern syntax** uses Python regex:
   - `rm\s+-rf` — matches `rm -rf`
   - `console\.log\(` — matches `console.log(`
   - `\.env$` — files ending in `.env`
   - `(API_KEY|SECRET|TOKEN)\s*=\s*["']` — hardcoded credentials
   - `(eval\|exec)\(` — eval or exec calls

7. **Multiple conditions** with AND logic (all must match):
   ```yaml
   ---
   name: api-key-in-typescript
   enabled: true
   event: file
   conditions:
     - field: file_path
       operator: regex_match
       pattern: \.tsx?$
     - field: new_text
       operator: regex_match
       pattern: (API_KEY|SECRET|TOKEN)\s*=\s*["']
   ---
   ```

8. **Available operators**: `regex_match`, `contains`, `equals`, `not_contains`, `starts_with`, `ends_with`

9. **Managing rules**:
   - List all rules: `/hookify:list`
   - Interactive configure (enable/disable): `/hookify:configure`
   - Disable a rule: edit `.local.md`, set `enabled: false`
   - Delete a rule: `rm .claude/hookify.rule-name.local.md`

10. **Rules take effect immediately** — no Claude Code restart required. The `.claude/` directory (in project root, not plugin directory) is read on every tool use.

11. **Troubleshooting**: If a rule doesn't trigger, verify the file is in `.claude/` (project root), check `enabled: true`, test the regex with `python3 -c "import re; print(re.search(r'pattern', 'text'))"`, and use specific event types (not `all`) for better performance.

12. **Useful pre-built rule patterns**:
   - Block destructive ops: `rm\s+-rf|dd\s+if=|mkfs|format` on bash event
   - Warn debug code: `console\.log\(|debugger;|print\(` on file event
   - Require tests before stop: check transcript for `npm test|pytest|cargo test`
   - Block sensitive file edits: `\.env$|credentials|secrets` on file_path

### Verification
- [ ] `/hookify [description]` creates a `.local.md` file in `.claude/`
- [ ] Rule triggers immediately on next tool use (no restart needed)
- [ ] `action: block` prevents the operation from executing
- [ ] `action: warn` shows the message but allows proceeding
- [ ] `/hookify:list` shows all active rules

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, hookify, hooks, safety, rule-engine, v2

## Enforcement Loop
- WHERE: Any Claude Code project where unsafe patterns should be prevented
- WHEN: Setting up project safety rules, after noticing repeated unwanted behaviors
- HOW: Use `/hookify [description]` for explicit rules; run `/hookify` to auto-detect from conversation patterns
- CONNECT: CCP-004 (plugin-dev hook-development skill), CCP-013 (security-guidance)
