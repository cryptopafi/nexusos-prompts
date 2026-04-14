---
type: procedure
created: 2026-03-17
status: active
slug: mlops-041
tags: [procedure, nexus]
---

# MLOPS-041 — Build AWS Step Functions with Rust

## Problema
Orchestrating multi-step ML pipelines in Rust requires Step Functions integration patterns.

## Procedura
### Prerequisites
- AWS account
- Rust toolchain
- MLOPS-038 completed

### Steps
1. Write multiple Rust Lambda functions for pipeline stages: preprocess, train, evaluate.
2. Define Step Functions state machine JSON with Task states for each Lambda.
3. Add Choice state for conditional logic (e.g., retrain if accuracy < threshold).
4. Add Parallel state for running evaluation on multiple datasets simultaneously.
5. Deploy Lambdas with `cargo lambda deploy`.
6. Create state machine with AWS CLI: `aws stepfunctions create-state-machine`.
7. Execute and monitor pipeline in Step Functions console.

### Verification
- State machine executes all stages in sequence.
- Choice state correctly routes based on evaluation metrics.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Rust
- AWS Step Functions
- AWS Lambda
- cargo-lambda

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
