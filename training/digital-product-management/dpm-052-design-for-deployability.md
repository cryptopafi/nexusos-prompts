---
type: procedure
created: 2026-03-17
status: active
slug: dpm-052-design-for-deployability
tags: [procedure, nexus]
---

# DPM-052 — Design for Deployability

## Problema
Product architecture makes deployments risky and slow because deployability was not a design consideration.

## Procedura
### Prerequisites
- Architecture documentation
- Deployment history
- Engineering team access

### Steps
1. Review the last 5 deployments: what went wrong, how long did they take, what was rolled back?
2. Identify deployability blockers: monolithic architecture, database migrations, shared state, long test suites
3. Design for independent deployability: loosely coupled services, backward-compatible APIs, database versioning
4. Implement blue-green or canary deployment strategy to reduce risk
5. Set up automated rollback: if health checks fail within 5 minutes, auto-revert
6. Create a deployment checklist: pre-deploy checks, deploy steps, post-deploy verification
7. Measure deployment frequency and failure rate monthly (DORA metrics)

### Verification
- Deployment retrospective completed
- Deployability improvements designed
- DORA metrics baseline established

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, deployability, architecture

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Architecture documentation
- Deployment history
- Engineering team access

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
