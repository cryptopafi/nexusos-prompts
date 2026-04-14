---
type: procedure
created: 2026-03-20
status: active
slug: ccs-007-tdd
tags: [procedure, nexus]
---

# CCS-007 — TDD (Test-Driven Development)

## Problema
Developers writing tests after implementation produce tests that verify implementation details rather than behavior, creating a fragile test suite that breaks on refactors even when behavior is unchanged. Writing all tests first (horizontal slicing) compounds this by forcing tests to be written for imagined behavior, not actual behavior. True TDD requires vertical slices: one test, one implementation, repeat — so each test responds to what was learned from the previous cycle.

## Procedura

### Prerequisites
- Public interface agreed upon (or designed via CCS-005)
- Test framework available in the project
- Behaviors to test identified and prioritized
- User available for planning confirmation

### Steps

1. **Plan before writing any code.** Confirm with the user what interface changes are needed. Confirm which behaviors to test (you cannot test everything — prioritize critical paths and complex logic). Identify opportunities for deep modules (small interface, large implementation). Design interfaces for testability. List the behaviors to test as a flat list — these are behaviors, not implementation steps.

2. **Get user approval on the plan.** Do not write any tests or implementation code until the user has approved the behavior list and interface shape.

3. **Write ONE test (the tracer bullet).** Write a single test that confirms one thing about the system. This is your tracer bullet — it proves the path works end-to-end. The test must: describe behavior, not implementation; use the public interface only; survive an internal refactor where the implementation is completely rewritten; read like a specification ("user can checkout with valid cart").

4. **Make the tracer bullet test FAIL (RED).** Verify the test actually fails before writing implementation. A test that passes immediately either tests nothing or tests existing behavior (which is still valuable, but different).

5. **Write minimal code to make the test pass (GREEN).** Write only enough code to make the current test pass. Do not anticipate future tests. Do not add features the current test does not require. Keep it minimal.

6. **Avoid horizontal slicing.** Never write all tests first and then all implementation. This produces tests that test the shape of things (data structures, function signatures) rather than user-facing behavior. Tests written in bulk are insensitive to real changes — they pass when behavior breaks, fail when behavior is fine.

7. **Repeat the RED-GREEN cycle for each remaining behavior.** One test at a time. Only enough code to pass the current test. Do not anticipate future tests. Keep each test focused on observable behavior (what the system does, not how it does it).

8. **Apply the checklist per cycle.** After each RED-GREEN pair: Does the test describe behavior, not implementation? Does the test use public interface only? Would the test survive an internal refactor? Is the code minimal for this test? Were any speculative features added (if yes, remove them)?

9. **Mock only at system boundaries.** Mock external APIs (payment, email, SMS), databases (or use a test database), time/randomness, and file system (sometimes). Never mock your own classes/modules, internal collaborators, or anything you control. Mock at the outermost boundary, not inside the system.

10. **Use dependency injection for mockability.** Pass external dependencies in rather than creating them internally. Prefer SDK-style interfaces over generic fetchers — create specific functions for each external operation so each is independently mockable without conditional logic.

11. **Design interfaces for testability.** Accept dependencies, don't create them. Return results, don't produce side effects where avoidable (side effects require querying external state to verify). Keep surface area small: fewer methods = fewer tests needed, fewer params = simpler test setup.

12. **Refactor after all tests are GREEN.** Look for refactor candidates: duplication (extract function/class), long methods (break into private helpers, keep tests on public interface), shallow modules (combine or deepen), feature envy (move logic to where data lives), primitive obsession (introduce value objects). Never refactor while RED — get to GREEN first, then clean up.

13. **Apply deep module thinking during refactor.** Ask: Can I reduce the number of methods? Can I simplify the parameters? Can I hide more complexity inside? A deep module has a small interface and large implementation. A shallow module has a large interface and thin implementation — avoid.

### Verification
- [ ] Each test describes behavior (WHAT) not implementation (HOW)
- [ ] No mocks of internal collaborators — only system boundary mocks
- [ ] RED state verified before writing GREEN implementation
- [ ] No speculative features added beyond what current test requires
- [ ] Refactor only happens after full GREEN state

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, tdd, testing, red-green-refactor, deep-modules, mocking, v2

## Enforcement Loop
- WHERE: Any feature build or bug fix where correctness matters
- WHEN: User says "build with TDD", "red-green-refactor", "test-first", or wants integration tests
- HOW: Enforce one-test-one-implementation vertical slices; reject all-tests-first horizontal slicing
- CONNECT: CCS-005 (interface design precedes TDD), CCS-008 (triage uses TDD fix plans), CCS-009 (architecture improvement uses TDD), CCS-006 (refactor planning)
