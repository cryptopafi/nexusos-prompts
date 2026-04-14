---
type: procedure
created: 2026-03-17
status: active
slug: mlops-042
tags: [procedure, nexus]
---

# MLOPS-042 — Load-Test a Rust Microservice

## Problema
ML services deployed without load testing fail under production traffic patterns.

## Procedura
### Prerequisites
- Deployed Rust microservice
- load testing tool

### Steps
1. Install a load testing tool: `cargo install drill` or use `wrk`/`k6`.
2. Write a benchmark configuration targeting `/predict` endpoint with realistic payloads.
3. Run baseline test: 10 concurrent users, 30 seconds duration.
4. Increase load incrementally: 50, 100, 200 concurrent users.
5. Record metrics: p50, p95, p99 latency, requests/sec, error rate.
6. Identify the breaking point where error rate exceeds 1%.
7. Document findings and recommend instance sizing.

### Verification
- Load test completes without tool errors.
- Latency and throughput metrics documented for each concurrency level.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- drill, wrk, or k6
- deployed service endpoint

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
