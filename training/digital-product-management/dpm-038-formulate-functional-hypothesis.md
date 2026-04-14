---
type: procedure
created: 2026-03-17
status: active
slug: dpm-038-formulate-functional-hypothesis
tags: [procedure, nexus]
---

# DPM-038 — Formulate Functional Hypothesis

## Problema
Product features are assumed to work correctly without explicit functional hypotheses that can be tested.

## Procedura
### Prerequisites
- Feature specification
- Technical requirements
- User scenarios

### Steps
1. Identify the core function the feature must perform
2. State the functional hypothesis: 'We believe [feature] will correctly [function] when [condition]'
3. Define functional success criteria: accuracy, reliability, performance thresholds
4. Identify edge cases: what happens at boundaries, with bad input, or under load?
5. Design functional tests: unit tests, integration tests, and user acceptance tests
6. Map each test to a hypothesis and success criterion
7. Run tests and document: pass/fail per hypothesis, bugs found, fixes needed

### Verification
- Functional hypotheses documented
- Tests designed and executed
- Pass/fail results recorded

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, functional-testing, hypothesis-testing

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Feature specification
- Technical requirements
- User scenarios

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
