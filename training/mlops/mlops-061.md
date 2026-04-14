---
type: procedure
created: 2026-03-17
status: active
slug: mlops-061
tags: [procedure, nexus]
---

# MLOPS-061 — Monitor Data Drift and Model Performance on SageMaker

## Problema
Models degrade silently when input data distributions shift from training data patterns.

## Procedura
### Prerequisites
- AWS SageMaker endpoint deployed
- baseline data statistics

### Steps
1. Create a SageMaker Model Monitor baseline from training data.
2. Configure data capture on the endpoint to log inputs and outputs.
3. Schedule monitoring job: `create_monitoring_schedule()` for hourly/daily checks.
4. Define constraints: feature distributions, missing value thresholds, data types.
5. Set up CloudWatch alarms for constraint violations.
6. Review monitoring reports: distribution comparisons, violation details.
7. Implement automated retraining trigger when drift exceeds threshold.

### Verification
- Model Monitor detects injected drift in test data.
- CloudWatch alarm fires on constraint violation.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS SageMaker Model Monitor
- CloudWatch

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
