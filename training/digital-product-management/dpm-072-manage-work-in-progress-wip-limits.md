---
type: procedure
created: 2026-03-17
status: active
slug: dpm-072-manage-work-in-progress-wip-limits
tags: [procedure, nexus]
---

# DPM-072 — Manage Work in Progress (WIP Limits)

## Problema
Teams start too many things at once, causing context switching and slow delivery of everything.

## Procedura
### Prerequisites
- Kanban board (DPM-069)
- Team capacity data
- Flow metrics

### Steps
1. Count current WIP: how many items are 'in progress' right now?
2. Compare to team capacity: WIP should be <= number of team members (or pairs)
3. Set explicit WIP limits per board column and enforce them
4. When a column hits its WIP limit, the team must finish something before starting new work
5. Track the impact: measure cycle time before and after WIP limits
6. Address resistance: explain that WIP limits increase throughput by reducing context switching
7. Adjust WIP limits monthly based on flow data and team feedback

### Verification
- WIP limits set and enforced
- Cycle time measured before/after
- Monthly adjustment cadence established

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, wip-limits, flow-optimization

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Kanban board (DPM-069)
- Team capacity data
- Flow metrics

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
