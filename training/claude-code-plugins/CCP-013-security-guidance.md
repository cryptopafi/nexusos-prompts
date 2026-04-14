---
type: procedure
created: 2026-03-20
status: active
slug: ccp-013-security-guidance
tags: [procedure, nexus]
---

# CCP-013 — Security Pattern Warnings via Hooks

## Problema
Developers inadvertently introduce security vulnerabilities — command injection via `exec()`, XSS via `innerHTML`, code injection via `eval()`, insecure GitHub Actions workflows — because they don't recognize the dangerous pattern in the moment of writing code. A reminder at write time is far more effective than a post-hoc audit.

## Procedura

### Prerequisites
- Claude Code installed
- Python 3 available (`python3 --version`)
- Install: `/plugin install security-guidance@claude-plugins-official`
- No configuration required — works automatically via PreToolUse hook

### Steps

1. **The plugin is fully automatic**: A PreToolUse hook (`security_reminder_hook.py`) intercepts every Edit, Write, and MultiEdit tool call. No commands or skill triggers needed.

2. **Hook trigger logic**: On each file write, the hook checks:
   - The file path (for path-based patterns like GitHub Actions workflows)
   - The new content being written (for substring-based patterns)

   If a match is found AND it's the first time this specific rule has triggered for this file in the current session, the hook outputs the warning and **blocks the tool execution** (exit code 2).

3. **Session-scoped deduplication**: Each warning fires only once per file+rule combination per session. State is stored in `~/.claude/security_warnings_state_{session_id}.json`. Files older than 30 days are automatically cleaned up.

4. **Security patterns detected** (9 built-in rules):

   - **GitHub Actions workflows** (`.github/workflows/*.yml`): Warns about command injection via untrusted event inputs (issue titles, PR descriptions, commit messages). Shows safe `env:` pattern vs unsafe direct interpolation.

   - **`child_process.exec()`**: Warns about shell injection. Suggests `execFileNoThrow` alternative that uses `execFile` instead of `exec` (prevents shell injection, handles Windows compatibility).

   - **`new Function()`**: Dynamic code evaluation risk.

   - **`eval()`**: Arbitrary code execution risk. Suggests `JSON.parse()` for data parsing.

   - **`dangerouslySetInnerHTML`**: XSS risk in React. Recommends DOMPurify or safe alternatives.

   - **`document.write()`**: XSS risk and performance issues. Recommends DOM manipulation methods.

   - **`.innerHTML =`**: XSS risk with untrusted content. Recommends `textContent` or DOMPurify.

   - **`pickle`** (Python): Unsafe deserialization. Recommends JSON.

   - **`os.system`** (Python): Command injection risk. Only safe with static, non-user-controlled arguments.

5. **Disable the plugin** for a session: Set environment variable `ENABLE_SECURITY_REMINDER=0` before starting Claude Code. Re-enable by unsetting or setting to `1`.

6. **The warning format**: The hook writes the reminder to stderr and exits with code 2. Claude sees the warning as a system message and must consider it before proceeding with the file write. This is an informational block — Claude can still write the code after acknowledging the risk.

7. **Adding custom patterns**: The `SECURITY_PATTERNS` list in `security_reminder_hook.py` can be extended with additional rules following the same structure: `ruleName`, `path_check` (lambda) or `substrings` (list), and `reminder` (string).

8. **Debug log**: Security warning events are logged to `/tmp/security-warnings-log.txt` with timestamps — useful for auditing what triggered and when.

### Verification
- [ ] Writing a file containing `eval(` shows the eval security warning
- [ ] Warning only appears once per file+rule per session (no repeat spam)
- [ ] Writing a `.github/workflows/` YAML triggers the Actions workflow warning
- [ ] Setting `ENABLE_SECURITY_REMINDER=0` disables all warnings for the session
- [ ] Plugin operates without any additional configuration after install

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, security, hooks, xss, injection, github-actions, v2

## Enforcement Loop
- WHERE: All Claude Code projects where code is being written
- WHEN: Automatically active on every Edit/Write/MultiEdit tool use
- HOW: No manual action needed; when warning fires, acknowledge it and verify the code is safe before proceeding
- CONNECT: CCP-008 (hookify for custom security rules), CCP-004 (plugin-dev hook-development)
