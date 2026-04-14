---
type: procedure
created: 2026-03-20
status: active
slug: ccs-006-request-refactor-plan
tags: [procedure, nexus]
---

# CCS-006 — Request Refactor Plan

## Problema
Refactoring without a written plan leads to large, risky changes where intermediate states break existing behavior. Developers underestimate the scope of changes, skip testing considerations, and produce commits that are hard to review or roll back. A detailed refactor RFC with tiny commits ensures each step leaves the codebase in a working state, following Martin Fowler's principle of making each refactoring step as small as possible.

## Procedura

### Prerequisites
- Clear description of the area to be refactored and why
- `gh` CLI authenticated for issue creation
- Codebase accessible for exploration and test coverage analysis

### Steps

1. **Gather detailed problem description.** Ask the user for a long, detailed description of the problem they want to solve and any potential ideas for solutions. Do not accept "the code is messy" — require specifics about what friction is experienced, what bugs it causes or risks, and what the desired end state looks like.

2. **Explore the repository.** Verify the user's assertions about the current state of the code. Look at the affected area thoroughly — understand how it is currently structured, what it touches, what depends on it, and what patterns it uses.

3. **Present alternative approaches.** Before locking into the user's proposed solution, ask whether they have considered other options. Present 2-3 alternatives with their trade-offs. A refactor plan is expensive to execute — the chosen approach should be the best available, not just the first one considered.

4. **Conduct detailed implementation interview.** Interview the user thoroughly about the implementation. Cover: What exactly will change vs stay the same? What are the public interfaces that cannot break? What internal structures will change? How will the system behave differently (or identically) after the refactor?

5. **Hammer out exact scope.** Nail down precisely what is in scope and out of scope. The out-of-scope list is as important as the in-scope list. Any ambiguity here leads to scope creep during execution.

6. **Assess test coverage.** Look in the codebase for test coverage of the area being refactored. If coverage is insufficient, ask the user what their plans for testing are before the refactor begins. Refactoring untested code is high-risk — tests must exist or be written first to verify behavior is preserved.

7. **Break into tiny commits.** Apply Martin Fowler's advice: make each refactoring step as small as possible so you can always see the program working. Each commit should: leave the codebase in a working state (all tests pass), be a single logical change (rename, extract, move — not multiple operations at once), be independently reviewable, and be revertable without pulling a chain of other commits.

8. **Identify parallel-safe commits.** Some commits can be executed in any order; others have strict dependencies. Map these dependencies so execution can be optimized and blocked commits are identified before execution starts.

9. **Document as GitHub issue.** Create the RFC as a GitHub issue with `gh issue create`. Include: Problem Statement (from developer perspective), Solution (from developer perspective), Commits (long detailed plan with each tiny commit described in plain English), Decision Document (modules affected, interfaces modified, architectural decisions — no file paths or code snippets), Testing Decisions (what makes a good test, which modules to test, prior art in codebase), Out of Scope (explicit list), and Further Notes (optional).

10. **Enforce durable descriptions.** The Decision Document must not include specific file paths or code snippets — they become outdated quickly. Describe behaviors, interfaces, and modules instead.

11. **Verify commit plan completeness.** Before submitting the issue, walk through the commit plan mentally: does each commit leave the tests green? Is any step larger than it needs to be? Is there a commit that does two things that could be split?

### Verification
- [ ] Alternative approaches considered before committing to a solution
- [ ] Test coverage assessed; plan for testing included if coverage is insufficient
- [ ] Each commit in the plan leaves the codebase in a working state
- [ ] Decision Document contains no file paths or code snippets
- [ ] Out of Scope section explicitly defined

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, refactoring, rfc, planning, tiny-commits, github-issues, v2

## Enforcement Loop
- WHERE: Before starting any refactor involving more than 2 files or a public interface change
- WHEN: User says "plan a refactor", "create a refactoring RFC", "how do I safely refactor X"
- HOW: Never skip test coverage assessment; always break into tiny commits; always explore alternatives
- CONNECT: CCS-007 (TDD — write tests before refactoring), CCS-009 (architecture improvement that triggers refactors)
