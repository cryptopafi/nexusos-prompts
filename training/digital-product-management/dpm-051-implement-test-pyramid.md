---
type: procedure
created: 2026-03-17
status: active
slug: dpm-051-implement-test-pyramid
tags: [procedure, nexus]
---

# DPM-051 — Implement Test Pyramid

## Problema
Testing is unbalanced (too many manual tests, too few automated), causing slow feedback and missed bugs.

## Procedura
### Prerequisites
- Development environment
- Testing frameworks
- Current test suite inventory

### Steps
1. Audit current test distribution: how many unit, integration, and end-to-end tests exist?
2. Define target pyramid: ~70% unit tests, ~20% integration tests, ~10% E2E tests
3. Identify areas with no unit tests and create a plan to add them
4. Replace brittle E2E tests with more reliable integration tests where possible
5. Set up test coverage tracking (target 80%+ for critical paths)
6. Configure tests to run in the CI/CD pipeline with fast feedback (unit < 5 min, integration < 15 min)
7. Create a team testing standard document: what must be tested, at what level, before merge

### Verification
- Test audit completed
- Target pyramid defined
- Coverage tracking and CI integration configured

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, testing, quality-engineering

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Development environment
- Testing frameworks
- Current test suite inventory

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
