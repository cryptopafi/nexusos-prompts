---
type: procedure
created: 2026-03-20
status: active
slug: ccs-002-prd-to-plan
tags: [procedure, nexus]
---

# CCS-002 — PRD to Plan

## Problema
A PRD describes what to build but not how to sequence the work. Without an explicit phased plan, implementations drift into horizontal layering (build all schema first, then all API, then all UI) which produces large, hard-to-test pull requests and delayed integration feedback. Vertical slice planning ensures each phase is shippable, testable, and demoable on its own.

## Procedura

### Prerequisites
- PRD already in context (as text or GitHub issue number)
- `gh` CLI authenticated for issue fetching
- Codebase accessible for architecture exploration
- `./plans/` directory will be created if missing

### Steps

1. **Confirm PRD is in context.** If the PRD is not already in the conversation, ask the user to paste it or point to the file/GitHub issue number. Fetch with `gh issue view <number>` if needed.

2. **Explore the codebase.** If not already done, explore the repository to understand current architecture, existing patterns, and integration layers. Pay attention to route structures, database schema, data models, authentication approach, and third-party service boundaries.

3. **Identify durable architectural decisions.** Before slicing phases, extract high-level decisions that are unlikely to change throughout the entire implementation: route structures / URL patterns, database schema shape, key data models, authentication/authorization approach, third-party service boundaries. These go in the plan header so every phase can reference them.

4. **Draft vertical slices (tracer bullets).** Break the PRD into phases where each phase is a thin vertical slice cutting through ALL integration layers end-to-end — NOT a horizontal slice of one layer. A slice that only builds the schema is not a vertical slice. A slice that builds schema + API endpoint + UI component + test IS a vertical slice.

5. **Apply vertical slice rules.** Each slice must: deliver a complete path through every layer (schema, API, UI, tests); be demoable or verifiable on its own; avoid specific file names, function names, or implementation details likely to change; include only durable decisions (route paths, schema shapes, data model names).

6. **Prefer many thin slices over few thick ones.** If a proposed slice requires more than one sprint of work, split it. The goal is rapid feedback loops and incremental de-risking.

7. **Quiz the user on the breakdown.** Present the proposed phases as a numbered list. For each phase show: title (short descriptive name) and user stories covered (which numbered stories from the PRD this phase addresses). Ask: Does the granularity feel right? Should any phases be merged or split?

8. **Iterate until approved.** Revise the breakdown based on user feedback. Do not proceed to file creation until the user approves the breakdown.

9. **Create `./plans/` directory** if it does not exist.

10. **Write the plan file.** Name the file after the feature (e.g., `./plans/user-onboarding.md`). Structure: header with source PRD reference + architectural decisions, then one section per phase with title, user stories covered, description of end-to-end behavior (not layer-by-layer implementation), and acceptance criteria checkboxes.

11. **Keep phase descriptions behavior-focused.** Describe what the system does after this phase, not which files to edit. This makes the plan durable across refactors.

12. **Reference the plan in subsequent work.** When dispatching implementation work, always reference the plan file as the source of truth for phase scope and acceptance criteria.

### Verification
- [ ] Architectural decisions section populated in plan header
- [ ] Each phase is a vertical slice (touches all layers, not one layer)
- [ ] Each phase has concrete acceptance criteria checkboxes
- [ ] No specific file paths or function names in phase descriptions
- [ ] User approved the phase breakdown before file was written

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, planning, vertical-slices, tracer-bullets, prd, v2

## Enforcement Loop
- WHERE: After a PRD is created or received
- WHEN: User says "break this down", "create a plan", "plan phases", or mentions "tracer bullets"
- HOW: Always explore codebase first, always quiz user before writing the file
- CONNECT: CCS-001 (write-a-prd), CCS-003 (prd-to-issues), CCS-007 (TDD for implementation)
