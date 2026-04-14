---
type: procedure
created: 2026-03-20
status: active
slug: ccp-003-pr-review-toolkit
tags: [procedure, nexus]
---

# CCP-003 — PR Review Toolkit (6 Specialized Agents)

## Problema
A single generic code review misses important dimensions of quality. Comment accuracy, test coverage, silent failures, type design, general quality, and simplification each require different expertise and focus. Using one agent for all of these creates shallow reviews that miss critical issues in each area.

## Procedura

### Prerequisites
- Claude Code installed in an active git repository
- Install: `/plugin install pr-review-toolkit@claude-plugins-official`
- No additional tools required (agents trigger contextually)

### Steps

1. **Understand the 6 specialized agents** — each has a distinct focus and trigger pattern:

   - **comment-analyzer**: Checks comment accuracy vs actual code, detects comment rot, misleading or outdated documentation. Trigger: "Check if the comments are accurate", "Review the documentation I added"

   - **pr-test-analyzer**: Reviews behavioral vs line coverage, identifies critical test gaps, evaluates test quality and edge case handling. Rates gaps 1-10 (10 = critical, must add). Trigger: "Check if the tests are thorough", "Are there any critical test gaps?"

   - **silent-failure-hunter**: Scans for silent failures in catch blocks, inadequate error handling, inappropriate fallbacks, missing error logging. Trigger: "Review the error handling", "Check for silent failures"

   - **type-design-analyzer**: Rates type design on 4 dimensions (1-10 scale each): encapsulation, invariant expression, usefulness, invariant enforcement. Trigger: "Review the UserAccount type design", "Check if this type has strong invariants"

   - **code-reviewer**: CLAUDE.md compliance, style violations, bug detection, general quality. Scores issues 0-100 (91-100 = critical). Trigger: "Review my recent changes", "Check if everything looks good"

   - **code-simplifier**: Identifies unnecessary complexity, redundant code, poor readability. Preserves all functionality while improving structure. Trigger: "Simplify this code", "Make this clearer"

2. **Individual agent usage**: Ask naturally matching the agent's focus area — Claude automatically triggers the appropriate agent:
   ```
   "Can you check if the tests cover all edge cases?"
   → triggers pr-test-analyzer

   "Review the error handling in the API client"
   → triggers silent-failure-hunter
   ```

3. **Comprehensive pre-PR review** — request multiple agents at once:
   ```
   "I'm ready to create this PR. Please:
   1. Review test coverage
   2. Check for silent failures
   3. Verify code comments are accurate
   4. Review any new types
   5. General code review"
   ```

4. **Parallel vs sequential execution**: For speed, explicitly request parallel runs: "Run pr-test-analyzer and comment-analyzer in parallel". For sequential (when one informs the other): "First review test coverage, then check code quality".

5. **Recommended workflow timing**:
   - Before committing: `code-reviewer` + `silent-failure-hunter` (if error handling changed)
   - Before creating PR: `pr-test-analyzer` + `comment-analyzer` + `type-design-analyzer` + `code-reviewer`
   - After passing review: `code-simplifier` for polish

6. **All agents output structured findings**: Clear issue identification, specific file:line references, explanation of why it's a problem, suggested improvements, prioritized by severity.

7. **Proactive triggering**: Claude may trigger these agents autonomously based on context — after writing code it may run `code-reviewer`, after adding docs it may run `comment-analyzer`, after adding new types it may run `type-design-analyzer`.

8. **Targeting specific files**: If an agent analyzes too broadly, specify scope: "Review error handling only in `src/api/client.ts`" or "Check test coverage for recent changes only".

9. **Iteration pattern**: Run agent → fix issues → run same agent again to verify. Do not move to the next agent until current findings are resolved.

### Verification
- [ ] Each agent triggers when asked with its natural language trigger phrase
- [ ] `type-design-analyzer` outputs scores on 4 dimensions (1-10 each)
- [ ] `pr-test-analyzer` rates gaps 1-10 with explanation of criticality
- [ ] `code-simplifier` preserves all functionality when applied
- [ ] All agents provide file:line references in their output

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, pr-review, code-quality, test-coverage, type-design, v2

## Enforcement Loop
- WHERE: Claude Code sessions during feature development and before PR creation
- WHEN: After writing code, before committing, before creating PRs
- HOW: Trigger the appropriate agent based on what changed; run comprehensive review before PR
- CONNECT: CCP-001 (feature-dev), CCP-002 (code-review), CCP-014 (code-simplifier)
