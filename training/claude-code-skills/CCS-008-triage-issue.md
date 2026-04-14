---
type: procedure
created: 2026-03-20
status: active
slug: ccs-008-triage-issue
tags: [procedure, nexus]
---

# CCS-008 — Triage Issue (Bug Investigation and Fix Planning)

## Problema
When a bug is reported, developers often jump straight to a fix without fully understanding the root cause, producing patches that address symptoms rather than causes. Without a written fix plan, the same class of bug recurs. Without TDD-based fix planning, the fix is unverified and future regressions go undetected. A structured triage process produces a GitHub issue with a complete root cause analysis and TDD fix plan before any code is changed.

## Procedura

### Prerequisites
- Brief description of the problem from the user
- `gh` CLI authenticated and targeting the correct repository
- Codebase accessible for deep exploration
- `git log` access for recent change history

### Steps

1. **Capture the problem minimally.** Get a brief description of the issue from the user. If they haven't provided one, ask ONE question: "What's the problem you're seeing?" Do NOT ask follow-up questions yet — start investigating immediately. This is a mostly hands-off workflow.

2. **Explore and diagnose the codebase.** Use an exploration sub-agent to deeply investigate. The goal is to find: WHERE the bug manifests (entry points, UI, API responses); WHAT code path is involved (trace the flow from trigger to symptom); WHY it fails (the root cause, not just the symptom); WHAT related code exists (similar patterns, tests, adjacent modules).

3. **Examine the full evidence set.** Look at: related source files and their dependencies; existing tests (what's tested, what's missing around the bug area); recent changes to affected files via `git log` on relevant files; error handling in the code path; similar patterns elsewhere in the codebase that work correctly.

4. **Identify the fix approach.** Based on investigation, determine: the minimal change needed to fix the root cause (not the symptom); which modules/interfaces are affected; what behaviors need to be verified via tests; whether this is a regression (recent change broke it), missing feature (was never implemented), or design flaw (structural issue).

5. **Design TDD fix plan with vertical slices.** Create a concrete, ordered list of RED-GREEN cycles. Each cycle is one vertical slice: RED (describe a specific test that captures the broken/missing behavior) + GREEN (describe the minimal code change to make that test pass). Rules: tests verify behavior through public interfaces, not implementation details; one test at a time, vertical slices; each test survives internal refactors.

6. **Apply durability rules to the fix plan.** Only suggest fixes that would survive radical codebase changes. Describe behaviors and contracts, not internal structure. Tests must assert on observable outcomes (API responses, UI state, user-visible effects), not internal state. A good fix plan reads like a spec; a bad one reads like a diff.

7. **Include a refactor step if needed.** After all RED-GREEN cycles, add a REFACTOR step for any cleanup required to leave the code in a clean state. Refactor only after all tests are GREEN.

8. **Create the GitHub issue immediately.** Use `gh issue create` with the template. Do NOT ask the user to review before creating — just create it and share the URL. The issue contains: Problem (actual behavior, expected behavior, reproduction steps); Root Cause Analysis (code path involved, why current code fails, contributing factors — no file paths or line numbers); TDD Fix Plan (numbered RED-GREEN cycles); Acceptance Criteria (checkboxes including "all new tests pass" and "existing tests still pass").

9. **Keep root cause analysis durable.** Do NOT include specific file paths, line numbers, or implementation details in the issue. These couple the issue to current code layout and make it useless after refactors. Describe modules, behaviors, and contracts instead.

10. **Print the issue URL and one-line root cause summary.** After creating the issue, immediately output: the issue URL for easy navigation, and a single sentence summarizing the root cause. This gives the user an immediate executive summary.

### Verification
- [ ] Root cause identified (not just symptom)
- [ ] TDD fix plan uses vertical slices (not all-tests-first)
- [ ] Root cause analysis contains no file paths or line numbers
- [ ] Issue created immediately without asking user to review first
- [ ] Issue URL and one-line root cause summary printed after creation

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, bug-triage, debugging, tdd, github-issues, root-cause-analysis, v2

## Enforcement Loop
- WHERE: Any reported bug or unexpected behavior
- WHEN: User says "bug", "triage", "investigate", "something is broken", or files an issue report
- HOW: Investigate first with minimal questions; create issue immediately; never skip root cause analysis
- CONNECT: CCS-007 (TDD for the fix implementation), CCS-003 (prd-to-issues for larger fix scopes)
