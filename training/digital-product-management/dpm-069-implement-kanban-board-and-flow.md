---
type: procedure
created: 2026-03-17
status: active
slug: dpm-069-implement-kanban-board-and-flow
tags: [procedure, nexus]
---

# DPM-069 — Implement Kanban Board and Flow

## Problema
Work is invisible and unmanaged, causing bottlenecks and unpredictable delivery.

## Procedura
### Prerequisites
- Task management tool (Jira, Trello, or physical board)
- Team workflow knowledge

### Steps
1. Map your current workflow into columns: To Do, In Progress, Review, Testing, Done
2. Set WIP (Work in Progress) limits for each column based on team capacity
3. Visualize all current work on the board: one card per work item
4. Implement pull system: team members pull new work only when WIP limit allows
5. Track cycle time: how long does a card take from 'In Progress' to 'Done'?
6. Identify bottlenecks: which column has the most cards stuck?
7. Run weekly flow review: analyze cycle time trends and adjust WIP limits

### Verification
- Kanban board set up with WIP limits
- Cycle time tracked
- Weekly flow review established

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, kanban, flow-management

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Task management tool (Jira, Trello, or physical board)
- Team workflow knowledge

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
