---
type: procedure
created: 2026-03-17
status: active
slug: mlops-060
tags: [procedure, nexus]
---

# MLOPS-060 — Implement Least-Privilege Security for AWS Lambda ML

## Problema
Over-permissioned Lambda functions create security risks in ML inference pipelines.

## Procedura
### Prerequisites
- AWS account
- Lambda ML function deployed

### Steps
1. Audit current Lambda execution role: `aws iam get-role-policy`.
2. Remove wildcard permissions (`*`) and scope to specific resources.
3. Create minimal IAM policy: only S3 bucket for model, CloudWatch for logs.
4. Add resource-level ARNs instead of `*`: specific S3 paths, DynamoDB tables.
5. Enable CloudTrail logging for the Lambda function.
6. Use environment variables for configuration instead of hardcoded values.
7. Test that function still works with restricted permissions.
8. Set up IAM Access Analyzer to detect unused permissions.

### Verification
- Lambda function operates correctly with minimal permissions.
- IAM Access Analyzer shows no findings.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS IAM
- AWS Lambda
- AWS CloudTrail

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
