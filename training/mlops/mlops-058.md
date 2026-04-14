---
type: procedure
created: 2026-03-17
status: active
slug: mlops-058
tags: [procedure, nexus]
---

# MLOPS-058 — Provision EC2 and EBS for ML Workloads

## Problema
ML training on undersized instances wastes time; oversized instances waste money.

## Procedura
### Prerequisites
- AWS account
- understanding of ML compute requirements

### Steps
1. Select instance type based on workload: `p3.2xlarge` (GPU), `r5.xlarge` (memory), `c5.xlarge` (CPU).
2. Launch EC2 instance with appropriate AMI (Deep Learning AMI for GPU).
3. Attach EBS volume sized for dataset: `aws ec2 create-volume --size 500 --volume-type gp3`.
4. Mount EBS: `sudo mkfs -t xfs /dev/xvdf && sudo mount /dev/xvdf /data`.
5. Configure security group: allow SSH (22) and restrict to your IP.
6. Set up auto-stop with CloudWatch alarm on low CPU utilization.

### Verification
- Instance launches and SSH connection succeeds.
- EBS volume mounted and writable at /data.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS EC2
- AWS EBS
- SSH

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
