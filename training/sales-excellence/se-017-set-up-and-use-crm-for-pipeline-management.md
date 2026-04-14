---
type: procedure
created: 2026-03-17
status: active
slug: se-017-set-up-and-use-crm-for-pipeline-management
tags: [procedure, nexus]
---

# SE-017 — Set Up and Use CRM for Pipeline Management

## Problema
Pipeline lives in spreadsheets or heads, making forecasting and management impossible.

## Procedura
### Prerequisites
- CRM account (HubSpot, Salesforce, or Pipedrive)
- Deal stage definitions

### Steps
1. Define 5-7 pipeline stages matching your sales process (e.g., Lead, Qualified, Demo, Proposal, Negotiation, Won, Lost)
2. Configure stages in CRM with entry criteria for each stage
3. Set required fields per stage (e.g., Proposal stage requires budget confirmed)
4. Import or create all active deals into CRM pipeline
5. Set up a pipeline view sorted by stage and expected close date
6. Create a weighted pipeline report (stage probability x deal value)
7. Schedule daily 10-min pipeline review to update deal stages and next actions

### Verification
- Pipeline stages configured in CRM
- All active deals entered
- Weighted pipeline report generated

## Cortex Logging
- collection: procedures
- tags: sales-excellence, training, crm, pipeline-management

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing sales-excellence skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- CRM account (HubSpot, Salesforce, or Pipedrive)
- Deal stage definitions

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
