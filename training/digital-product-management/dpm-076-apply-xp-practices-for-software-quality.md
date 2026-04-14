---
type: procedure
created: 2026-03-17
status: active
slug: dpm-076-apply-xp-practices-for-software-quality
tags: [procedure, nexus]
---

# DPM-076 — Apply XP Practices for Software Quality

## Problema
Software quality is an afterthought, caught by QA at the end instead of built in from the start.

## Procedura
### Prerequisites
- Engineering team buy-in
- XP knowledge
- CI/CD pipeline (DPM-050)

### Steps
1. Implement TDD rigorously: no production code without a failing test first
2. Set up continuous integration with automated test runs on every commit
3. Practice pair programming on all complex or high-risk code changes
4. Implement refactoring as a regular practice: 10% of each sprint for code improvement
5. Set up code review standards: every change reviewed by at least one other developer
6. Track quality metrics: defect escape rate, test coverage, mean time to recovery
7. Run monthly quality retrospectives: are XP practices improving quality metrics?

### Verification
- TDD practiced consistently
- Quality metrics tracked
- Monthly quality retro running

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, xp, software-quality

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Engineering team buy-in
- XP knowledge
- CI/CD pipeline (DPM-050)

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
