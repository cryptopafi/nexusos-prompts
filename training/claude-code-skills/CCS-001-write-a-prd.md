---
type: procedure
created: 2026-03-20
status: active
slug: ccs-001-write-a-prd
tags: [procedure, nexus]
---

# CCS-001 — Write a PRD

## Problema
Teams starting new features lack a shared, written understanding of the problem and solution, leading to misaligned implementations. A PRD created through structured interview and codebase exploration surfaces hidden assumptions, aligns stakeholders, and provides a stable contract for implementation. Without this artifact, each developer interprets requirements differently and technical debt accumulates from the start.

## Procedura

### Prerequisites
- Access to the GitHub repository
- `gh` CLI authenticated
- User available for interview session
- Codebase accessible for exploration

### Steps

1. **Gather initial problem description.** Ask the user for a long, detailed description of the problem they want to solve and any potential ideas for solutions. Do not shortcut this step — the more detail provided, the fewer clarifying interviews needed later.

2. **Explore the repository.** Before any design decisions, explore the codebase to verify the user's assertions and understand the current state. Look at existing patterns, module structure, API contracts, and any relevant prior art that could inform the solution.

3. **Conduct relentless interview.** Interview the user about every aspect of the plan. Walk down each branch of the decision tree one-by-one, resolving dependencies between decisions. Ask about edge cases, error states, rollback scenarios, and performance requirements. Do not stop until a complete shared understanding is reached.

4. **Identify deep module opportunities.** Sketch out the major modules that need to be built or modified. Actively look for opportunities to extract deep modules — modules with a small, testable interface that encapsulates significant complexity (as opposed to shallow modules where the interface is nearly as complex as the implementation).

5. **Confirm module design with user.** Present the module sketches to the user and check that they match expectations. Confirm which modules the user wants tests written for. This alignment prevents wasted testing effort on trivial pass-through code.

6. **Draft Problem Statement.** Write the problem from the user's perspective — what pain they experience, when it occurs, and its impact. Avoid technical jargon in this section; it should be legible to non-engineers.

7. **Draft Solution section.** Describe the solution from the user's perspective. Focus on observable behavior and outcomes, not implementation mechanics.

8. **Write User Stories.** Create an extensive numbered list of user stories in the format: "As a [actor], I want [feature], so that [benefit]." Cover all actors (admin, end user, API consumer, etc.) and all aspects of the feature. Aim for comprehensiveness — edge cases and secondary paths matter.

9. **Document Implementation Decisions.** List modules to build/modify, interface shapes, architectural decisions, schema changes, and API contracts. Do NOT include specific file paths or code snippets — they become outdated quickly. Describe behaviors and contracts instead.

10. **Document Testing Decisions.** Specify what makes a good test for this feature (behavior-based, through public interfaces), which modules will be tested, and point to prior art in the codebase (similar test patterns that already exist).

11. **Define Out of Scope.** Explicitly list what is NOT being built. This prevents scope creep and gives future developers clear boundaries.

12. **Submit as GitHub issue.** Use `gh issue create` with the completed PRD as the issue body. The issue number becomes the canonical reference for all downstream planning (prd-to-plan, prd-to-issues).

### Verification
- [ ] Problem Statement written from user perspective, not technical perspective
- [ ] User Stories list is comprehensive and numbered
- [ ] Implementation Decisions contain no file paths or code snippets
- [ ] Deep module opportunities identified and confirmed with user
- [ ] PRD submitted as GitHub issue with a retrievable issue number

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, prd, planning, requirements, github-issues, v2

## Enforcement Loop
- WHERE: Start of any new feature or significant enhancement
- WHEN: User says "I want to build X" or "create a PRD" or "plan a new feature"
- HOW: Run the interview + codebase exploration sequence before any implementation work begins
- CONNECT: CCS-002 (prd-to-plan), CCS-003 (prd-to-issues), CCS-004 (grill-me for interview depth)
