---
type: procedure
created: 2026-03-17
status: active
slug: mlops-056
tags: [procedure, nexus]
---

# MLOPS-056 — Implement Monitoring and Logging for ML Systems on AWS

## Problema
ML systems fail silently without monitoring for model degradation, latency spikes, and errors.

## Procedura
### Prerequisites
- AWS account
- deployed ML service
- CloudWatch access

### Steps
1. Create CloudWatch custom metrics for: prediction latency, request count, error rate.
2. Publish metrics from your ML service: `cloudwatch.put_metric_data()`.
3. Create CloudWatch dashboard with key ML system metrics.
4. Set up alarms: latency > 500ms, error rate > 5%, invocation count anomaly.
5. Configure SNS topic for alarm notifications.
6. Add structured logging with request IDs for traceability.
7. Set up log insights queries for debugging production issues.

### Verification
- Dashboard shows real-time ML system metrics.
- Alarm triggers notification on simulated error.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS CloudWatch
- AWS SNS
- structured logging library

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
