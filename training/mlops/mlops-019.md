---
type: procedure
created: 2026-03-17
status: active
slug: mlops-019
tags: [procedure, nexus]
---

# MLOPS-019 — Build DataOps Pipelines with AWS Step Functions

## Problema
Ad-hoc data processing scripts cannot be orchestrated, monitored, or retried at production scale.

## Procedura
### Prerequisites
- AWS account
- AWS CLI configured
- basic Lambda knowledge

### Steps
1. Define a Step Functions state machine in JSON/YAML with sequential data processing steps.
2. Create Lambda functions for each pipeline stage: ingest, validate, transform, load.
3. Wire Lambdas into the state machine using Task states.
4. Add error handling with Catch/Retry blocks for transient failures.
5. Deploy with AWS CLI or CloudFormation.
6. Execute the pipeline and monitor in Step Functions console.
7. Add CloudWatch alarms for pipeline failures.

### Verification
- State machine executes all steps to completion.
- Failed steps retry and error handling triggers correctly.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS Step Functions
- AWS Lambda
- AWS CloudWatch

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
