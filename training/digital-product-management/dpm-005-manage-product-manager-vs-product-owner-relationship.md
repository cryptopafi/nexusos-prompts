---
type: procedure
created: 2026-03-20
status: active
slug: dpm-005-manage-product-manager-vs-product-owner-relationship
tags: [procedure, nexus]
---

# DPM-005 — Manage Product Manager vs Product Owner Relationship
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
Organizations conflate the PM (strategic, outward-facing) and PO (tactical, team-facing) roles — causing either role duplication or dangerous gaps where strategic direction and backlog management both suffer.

## Procedura
### Prerequisites
- Existing or planned PM and PO roles in the organization
- Scrum or Kanban framework in use
- Access to product strategy artifacts and backlog tool (Jira, Linear)

### Steps
1. **Define the strategic vs. tactical split** — Document the boundary: PM owns the "what" and "why" (vision, strategy, market analysis, customer segments, competitive positioning, pricing). PO owns the "how" and "when" at team level (user story refinement, sprint backlog, acceptance criteria, capacity planning, release coordination). Create a one-page T-chart reference.
2. **Map the 5-7 key handoff points** — Document each: (a) PM delivers prioritized roadmap themes → PO breaks into epics, (b) PM provides customer context/JTBD → PO writes stories, (c) PM sets success metrics → PO defines acceptance criteria, (d) PO surfaces technical constraints → PM adjusts prioritization, (e) PO reports velocity → PM updates roadmap timeline. Specify input, output, and cadence for each in a Notion table.
3. **Establish two-tier backlog governance** — Strategic backlog (PM-owned): themes, epics, opportunities scored by RICE, reviewed monthly. Team backlog (PO-owned): refined user stories ready for sprint, reviewed weekly in grooming. PM attends grooming bi-weekly for context; PO attends roadmap review monthly for feasibility flags.
4. **Align on prioritization authority** — Document explicitly: PM has final authority on WHAT (feature priority, roadmap sequence). PO has authority on HOW stories are structured and sprint scope based on capacity. Conflict resolution: PO presents data (velocity, complexity), PM makes trade-off with documented rationale.
5. **Create a weekly PM/PO sync** — 30 min, standing agenda: roadmap changes, upcoming grooming topics, blocked items, stakeholder requests. This is the single most important meeting for PM/PO health. Use a shared running doc for agenda and action items. Cancel only if both agree nothing changed.
6. **Implement a translation layer test** — The PO must explain strategic rationale for every sprint item. Test: randomly pick 3 sprint items, ask PO "Why is this more important than X?" They should answer with customer/business context, not "PM said so." If they cannot, improve the handoff process (Step 2).
7. **Handle single-person PM/PO scenarios** — When one person fills both: explicitly timebox modes. Mornings = strategic PM work (customer calls, competitive analysis, stakeholder meetings). Afternoons = tactical PO work (grooming, story writing, sprint support). Use calendar blocking to enforce. Never mix modes in the same meeting.
8. **Measure relationship health** — Track: (a) Story rejection rate in sprint review — target <10% (high = poor context transfer), (b) Unplanned work % per sprint — target <15% (high = PM inserting mid-sprint), (c) PM/PO sync attendance — 100% target. Review quarterly with joint retro.
9. **Evolve the model as you scale** — 1 team: PM=PO often. 2-3 teams: dedicated PO per team, one PM across. 4+ teams: Group PM → PMs → POs hierarchy. Document current stage and triggers for transitioning. Reference Spotify model or SAFe for scaled frameworks.

### Verification
- [ ] PM/PO responsibility T-chart documented and agreed by both parties
- [ ] Handoff points mapped with input/output/cadence
- [ ] Two-tier backlog governance operational
- [ ] Weekly PM/PO sync established with shared agenda doc
- [ ] Story rejection rate and unplanned work % baselined

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, product-owner, role-clarity, scrum, backlog-management

## Enforcement Loop
- WHERE: Any team with separate PM and PO roles, or single person filling both
- WHEN: At role assignment, quarterly reviews, or when sprint health metrics decline
- HOW: Steps 1-9; health metrics as ongoing diagnostic
- CONNECT: DPM-001 (PM roles), DPM-004 (cross-functional alignment), DPM-067 (Scrum), DPM-073 (backlog)

## Dependente
- Organizational clarity on PM/PO role separation
- Backlog tool (Jira, Linear, Shortcut)
- Sprint metrics visibility (velocity, rejection rate)

## Metrics
- Story rejection rate: target <10%
- Unplanned work per sprint: target <15%
- PM/PO sync attendance: target 100%
- Team satisfaction with product direction: quarterly survey >4.0/5.0
