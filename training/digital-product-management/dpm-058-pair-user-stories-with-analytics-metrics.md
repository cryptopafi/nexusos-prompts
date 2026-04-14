---
type: procedure
created: 2026-03-17
status: active
slug: dpm-058-pair-user-stories-with-analytics-metrics
tags: [procedure, nexus]
---

# DPM-058 — Pair User Stories with Analytics Metrics

## Problema
User stories are delivered without measuring whether they actually achieve their intended outcome.

## Procedura
### Prerequisites
- User stories in backlog
- Analytics tool configured
- Metric definitions

### Steps
1. For each user story, ask: 'How will we know this story succeeded?'
2. Define a primary metric for each story: the number that should change when the story ships
3. Add the metric to the story's acceptance criteria: 'Success: [metric] improves by [X%] within [Y days]'
4. Ensure analytics events are included in the story's technical requirements
5. After the story ships, set a review date to check the metric
6. If the metric didn't move, investigate: was it a tracking issue, adoption issue, or wrong hypothesis?
7. Create a team habit: no story is 'done' until the metric is reviewed

### Verification
- All stories paired with metrics
- Analytics events in technical requirements
- Post-ship metric review habit established

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, analytics, user-stories

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- User stories in backlog
- Analytics tool configured
- Metric definitions

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
