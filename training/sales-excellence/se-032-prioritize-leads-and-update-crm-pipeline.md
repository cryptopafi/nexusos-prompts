---
type: procedure
created: 2026-03-17
status: active
slug: se-032-prioritize-leads-and-update-crm-pipeline
tags: [procedure, nexus]
---

# SE-032 — Prioritize Leads and Update CRM Pipeline

## Problema
CRM pipeline is outdated and lead priorities are unclear, causing reps to work on stale data.

## Procedura
### Prerequisites
- CRM with active pipeline
- Lead scores (SE-031)

### Steps
1. Export current pipeline and sort by lead score, deal value, and close date
2. Apply the Eisenhower matrix: Urgent+Important (close this week), Important (advance this week), Urgent (respond today), Neither (park/disqualify)
3. Update all deal stages in CRM to reflect current reality
4. Remove or close-lost deals that have been stagnant for 2x your average sales cycle
5. Set next actions and due dates for every active deal
6. Create a 'Top 10' deal focus list for the week
7. Schedule 15-min CRM hygiene session every Friday

### Verification
- All deals updated to current stage
- Stale deals removed
- Top 10 focus list created

## Cortex Logging
- collection: procedures
- tags: sales-excellence, training, pipeline-hygiene, prioritization

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing sales-excellence skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- CRM with active pipeline
- Lead scores (SE-031)

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
