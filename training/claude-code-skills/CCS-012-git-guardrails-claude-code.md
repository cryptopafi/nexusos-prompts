---
type: procedure
created: 2026-03-20
status: active
slug: ccs-012-git-guardrails-claude-code
tags: [procedure, nexus]
---

# CCS-012 — Git Guardrails for Claude Code (Block Dangerous Git Commands)

## Problema
Claude Code agents running autonomously can accidentally execute destructive git operations — force pushes, hard resets, branch deletions, or `git clean` — that permanently destroy work. Without a hook intercepting these commands before execution, a single mistaken or hallucinated command can cause unrecoverable data loss. The Claude Code `PreToolUse` hook system allows interception of Bash commands before they execute, providing a safety layer that blocks specific git patterns.

## Procedura

### Prerequisites
- Claude Code installed and operational
- Target repository or global `~/.claude/` directory accessible
- `block-dangerous-git.sh` script available (from `git-guardrails-claude-code/scripts/`)

### Steps

1. **Ask scope before anything else.** Ask the user: install for this project only (`.claude/settings.json`) or for all projects globally (`~/.claude/settings.json`)? The answer determines where both the hook script and the settings entry go.

2. **Understand the blocked commands list.** The default block list covers the most destructive git operations: `git push` (all variants including `--force`); `git reset --hard`; `git clean -f` and `git clean -fd`; `git branch -D`; `git checkout .`; `git restore .`. When blocked, Claude sees a message stating it does not have authority to execute that command.

3. **Copy the hook script to the target location.** For project scope: copy to `.claude/hooks/block-dangerous-git.sh`. For global scope: copy to `~/.claude/hooks/block-dangerous-git.sh`. Create the `hooks/` directory if it does not exist. The script reads the tool input JSON from stdin and checks the `tool_input.command` field against the blocked patterns.

4. **Make the script executable.** Run `chmod +x <path-to-script>`. A non-executable hook script silently fails to block anything — this step is mandatory.

5. **Add the hook to the settings file.** For project scope, edit or create `.claude/settings.json`. For global scope, edit or create `~/.claude/settings.json`. The hook entry goes under `hooks.PreToolUse` as a `"matcher": "Bash"` entry with `"type": "command"` pointing to the script.

6. **Merge, never overwrite.** If the settings file already exists with other hooks or settings, merge the new `PreToolUse` entry into the existing `hooks.PreToolUse` array. Never overwrite an existing settings file. If `hooks.PreToolUse` doesn't exist yet, create it as an array.

7. **Use the correct path token.** For project-scope hooks, use `"$CLAUDE_PROJECT_DIR"/.claude/hooks/block-dangerous-git.sh` (with the environment variable) so the hook works regardless of where the project is checked out. For global hooks, use the literal expanded path `~/.claude/hooks/block-dangerous-git.sh`.

8. **Ask about customization.** Ask the user if they want to add or remove any patterns from the blocked list. Common additions: `git rebase` (in interactive mode), `git stash drop`, `git tag -d`. Common removals: `git push` if the user intentionally pushes from Claude Code. Edit the copied script to match the desired block list.

9. **Verify the hook works.** Run a quick test by piping a blocked command through the script: `echo '{"tool_input":{"command":"git push origin main"}}' | <path-to-script>`. The script should exit with code 2 and print a BLOCKED message to stderr. An exit code 2 is what Claude Code interprets as "block this command." Verify at least two patterns work (e.g., `git push` and `git reset --hard`).

10. **Verify a safe command is NOT blocked.** Test that `git status`, `git diff`, `git log`, and `git add` still pass through: `echo '{"tool_input":{"command":"git status"}}' | <path-to-script>`. Should exit with code 0 (allow). Blocking safe commands would cripple Claude Code's git awareness.

11. **Document the guardrails.** Tell the user which commands are now blocked and the message Claude will see when blocked. Confirm the scope (project vs global) and where the settings file was modified.

### Verification
- [ ] Hook script copied and made executable (`chmod +x`)
- [ ] Settings file updated (merged, not overwritten)
- [ ] `git push origin main` test exits with code 2 (BLOCKED)
- [ ] `git status` test exits with code 0 (allowed)
- [ ] Customization preferences applied to the block list

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, git-safety, claude-code-hooks, pre-tool-use, guardrails, v2

## Enforcement Loop
- WHERE: Any Claude Code project where autonomous agent operation is enabled
- WHEN: User says "add git guardrails", "prevent destructive git", "block git push in Claude Code"
- HOW: Always ask scope first; always verify with test commands; always merge settings
- CONNECT: CCS-011 (pre-commit hooks for human-triggered commits), CCS-013 (write-a-skill — used to build this guardrail skill)
