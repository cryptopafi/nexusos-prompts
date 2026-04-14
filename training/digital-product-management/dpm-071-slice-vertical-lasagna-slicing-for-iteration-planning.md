---
type: procedure
created: 2026-03-17
status: active
slug: dpm-071-slice-vertical-lasagna-slicing-for-iteration-planning
tags: [procedure, nexus]
---

# DPM-071 — Slice Vertical (Lasagna Slicing for Iteration Planning)

## Problema
Teams deliver horizontal slices (backend only, frontend only) that don't provide user value until all layers are done.

## Procedura
### Prerequisites
- Epic or large feature
- Architecture understanding
- Sprint planning context

### Steps
1. Identify the layers in your architecture: UI, API, business logic, database, integrations
2. Understand horizontal slicing (one layer at a time) vs. vertical slicing (thin slice through all layers)
3. Take a large feature and identify the smallest user-facing slice that touches all layers
4. Write the vertical slice as a user story with acceptance criteria
5. Ensure the slice is deployable and testable independently
6. Plan the next vertical slice: build on the first, adding more capability
7. Repeat until the full feature is delivered, each sprint producing a usable increment

### Verification
- Vertical slice identified and written as user story
- Slice is independently deployable
- Team delivers value each sprint

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, vertical-slicing, iteration-planning

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Epic or large feature
- Architecture understanding
- Sprint planning context

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
