---
type: procedure
created: 2026-03-17
status: active
slug: dpm-029-create-test-driven-user-stories
tags: [procedure, nexus]
---

# DPM-029 — Create Test-Driven User Stories

## Problema
User stories lack testable criteria, making it impossible to know when a story is truly done.

## Procedura
### Prerequisites
- Epic user stories (DPM-028)
- Testing framework knowledge

### Steps
1. Take an epic-level user story and break it into 3-5 smaller user stories
2. For each story, write in format: 'As a [user], I want to [action], so that [benefit]'
3. Write acceptance criteria using Given-When-Then: 'Given [context], When [action], Then [result]'
4. Add negative test cases: 'Given [context], When [wrong action], Then [error handling]'
5. Define edge cases and boundary conditions
6. Include non-functional criteria where relevant: performance, accessibility, security
7. Review stories with QA and engineering to validate testability

### Verification
- Stories written with Given-When-Then criteria
- Negative and edge cases included
- QA review completed

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, user-stories, test-driven

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Epic user stories (DPM-028)
- Testing framework knowledge

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
