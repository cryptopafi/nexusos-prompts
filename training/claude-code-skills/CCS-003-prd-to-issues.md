---
type: procedure
created: 2026-03-20
status: active
slug: ccs-003-prd-to-issues
tags: [procedure, nexus]
---

# CCS-003 — PRD to Issues

## Problema
A PRD as a single GitHub issue is too large for developers to pick up and implement directly. It needs to be decomposed into independently-grabbable work items with explicit dependency tracking. Without this decomposition, work gets blocked by unresolved dependencies and developers cannot work in parallel. Issues without dependency information create coordination overhead and merge conflicts.

## Procedura

### Prerequisites
- PRD as a GitHub issue (number or URL)
- `gh` CLI authenticated and targeting the correct repository
- Codebase accessible for context exploration

### Steps

1. **Locate the PRD.** Ask the user for the PRD GitHub issue number or URL. Fetch it with `gh issue view <number> --comments` to get full content including any discussion.

2. **Explore the codebase (if needed).** If the codebase context is not already established, explore it to understand current state. This informs realistic slice boundaries and dependency ordering.

3. **Classify each slice as HITL or AFK.** HITL (Human In The Loop) slices require a human decision, design review, or approval before or during implementation. AFK (Away From Keyboard) slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible — autonomous slices accelerate delivery.

4. **Draft vertical slices.** Break the PRD into tracer-bullet issues. Each issue is a thin vertical slice cutting through ALL integration layers end-to-end (schema + API + UI + tests), NOT a horizontal slice of one layer. A completed slice must be demoable or verifiable on its own.

5. **Map dependencies between slices.** Determine which slices must complete before others can start. Build a dependency graph. Look for opportunities to eliminate false dependencies (slices that appear dependent but could run in parallel with slight scope adjustment).

6. **Present the breakdown to the user.** Show a numbered list. For each slice include: title (short descriptive name), type (HITL / AFK), blocked by (which other slices must complete first, or "None — can start immediately"), and user stories covered (numbered references to the parent PRD).

7. **Quiz the user.** Ask: Does the granularity feel right (too coarse / too fine)? Are the dependency relationships correct? Should any slices be merged or split? Are the correct slices marked HITL vs AFK?

8. **Iterate until approved.** Revise the breakdown based on feedback. Do not create issues until the user approves the complete breakdown.

9. **Create issues in dependency order.** Create blockers first so real issue numbers are available for the "Blocked by" field. Use `gh issue create` for each slice.

10. **Apply the issue template.** Each issue body must contain: Parent PRD reference (#issue-number), "What to build" section (end-to-end behavior description, not layer-by-layer implementation, references to PRD sections), acceptance criteria checkboxes, blocked-by field (real issue numbers or "None — can start immediately"), and user stories addressed (numbered from parent PRD).

11. **Do NOT duplicate PRD content.** Reference specific sections of the parent PRD rather than copying them. This keeps issues maintainable when the PRD evolves.

12. **Do NOT close or modify the parent PRD issue.** It remains the canonical source for the full feature scope and all downstream issues link back to it.

### Verification
- [ ] All slices classified as HITL or AFK with reasoning
- [ ] Dependency graph is correct (no circular dependencies, no false dependencies)
- [ ] Issues created in dependency order so blockers have real issue numbers
- [ ] Each issue references parent PRD and does not duplicate its content
- [ ] Parent PRD issue untouched (not closed, not modified)

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, github-issues, planning, vertical-slices, dependency-tracking, v2

## Enforcement Loop
- WHERE: After PRD is approved and ready for implementation breakdown
- WHEN: User says "convert to issues", "create tickets", "break down the PRD into work items"
- HOW: Always get user approval on breakdown before creating any issues; create in dependency order
- CONNECT: CCS-001 (write-a-prd), CCS-002 (prd-to-plan), CCS-008 (triage-issue for bugs found during implementation)
