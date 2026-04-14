---
type: procedure
created: 2026-03-20
status: active
slug: ccp-009-ralph-loop
tags: [procedure, nexus]
---

# CCP-009 — Ralph Loop (Continuous Self-Referential AI Loops)

## Problema
Complex development tasks — getting tests to pass, iterative refinement, greenfield builds — require many rounds of work, verification, and adjustment. Running Claude Code manually for each iteration is slow and requires constant human supervision. Tasks with automatic verification criteria should be able to run autonomously until complete.

## Procedura

### Prerequisites
- Claude Code installed
- Install: `/plugin install ralph-loop@claude-plugins-official`
- Python 3 for hook scripts
- On Windows: Git for Windows required for bash hook execution

### Steps

1. **Understand the core mechanic**: Ralph Loop uses a Stop hook (`hooks/stop-hook.sh`) that intercepts Claude's exit attempts, feeds the same prompt file back to Claude, and allows it to continue iterating. Previous file modifications persist between iterations — Claude sees its own past work and improves on it.

2. **Start a loop with one command**:
   ```
   /ralph-loop "Your task description" --completion-promise "DONE" --max-iterations 50
   ```
   After this, Claude works autonomously until it outputs the completion promise or hits the iteration limit.

3. **Command options**:
   - `--max-iterations <n>` — stop after N iterations (default: unlimited). **Always set this** as a safety net.
   - `--completion-promise <text>` — exact string that signals completion (uses literal string matching)

4. **Cancel an active loop**:
   ```
   /cancel-ralph
   ```

5. **Write effective prompts — 4 principles**:

   **Clear completion criteria** (bad: "build and make it good" | good: explicit list of requirements + `Output: <promise>COMPLETE</promise>` when all met)

   **Incremental goals**: Break complex work into phases with explicit phase labels. Claude completes Phase 1, verifies it, then moves to Phase 2.

   **Self-correction loop**: Build the TDD cycle into the prompt:
   ```
   1. Write failing tests
   2. Implement feature
   3. Run tests
   4. If any fail, debug and fix
   5. Repeat until all green
   6. Output: <promise>COMPLETE</promise>
   ```

   **Escape hatches**: Include fallback instructions for when Claude gets stuck: "After 15 iterations, if not complete — document what's blocking, list what was attempted, suggest alternative approaches"

6. **How the Stop hook creates the loop**:
   - Claude finishes a work iteration and tries to exit
   - `stop-hook.sh` intercepts the exit
   - Hook checks if `--completion-promise` text appears in transcript
   - If not found: blocks exit, re-injects the original prompt
   - Claude resumes with same prompt but sees all previously modified files
   - Repeat until completion promise found or iteration limit reached

7. **Best use cases**:
   - Getting a test suite to pass (run tests → see failures → fix → repeat)
   - Greenfield projects where requirements are clear and verifiable
   - Overnight builds you can walk away from
   - Tasks with automatic verification (linters, type checkers, test runners)

8. **Poor use cases**:
   - Tasks requiring human judgment or design decisions
   - One-shot operations
   - Tasks with unclear or subjective success criteria
   - Production debugging

9. **Windows compatibility**: The stop hook runs bash. If Claude Code on Windows resolves `bash` to WSL instead of Git Bash, edit the cached plugin's `hooks/hooks.json`:
   ```json
   "command": "\"C:/Program Files/Git/bin/bash.exe\" ${CLAUDE_PLUGIN_ROOT}/hooks/stop-hook.sh"
   ```
   Location: `~/.claude/plugins/cache/claude-plugins-official/ralph-wiggum/<hash>/hooks/hooks.json`

10. **Real-world results**: 6 repositories generated overnight in YC hackathon testing; one $50k contract completed for $297 in API costs. Technique originated from Geoffrey Huntley's Ralph Wiggum coding methodology (ghuntley.com/ralph/).

### Verification
- [ ] `/ralph-loop "task" --max-iterations 10 --completion-promise "DONE"` starts the loop
- [ ] Claude does not exit between iterations until completion promise is output
- [ ] `/cancel-ralph` stops an active loop
- [ ] Files modified in iteration N are visible and readable in iteration N+1
- [ ] Loop terminates when completion promise text appears in transcript

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, ralph-loop, autonomous-agents, iteration, hooks, v2

## Enforcement Loop
- WHERE: Complex development tasks with verifiable completion criteria
- WHEN: Task requires multiple rounds of fix-verify-iterate; tests need to pass; greenfield build
- HOW: Write a precise prompt with explicit completion criteria, then run `/ralph-loop` with `--max-iterations` set as safety net
- CONNECT: CCP-008 (hookify for Stop hooks), CCP-001 (feature-dev for structured approach)
