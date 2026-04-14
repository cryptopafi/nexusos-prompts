---
type: procedure
created: 2026-03-17
status: active
slug: mlops-014
tags: [procedure, nexus]
---

# MLOPS-014 — Create Python Virtual Environments and Manage Dependencies

## Problema
System-wide Python installs cause dependency conflicts across ML projects.

## Procedura
### Prerequisites
- Python 3.9+

### Steps
1. Create a virtual environment: `python -m venv .venv`.
2. Activate it: `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows).
3. Install packages: `pip install pandas scikit-learn`.
4. Freeze dependencies: `pip freeze > requirements.txt`.
5. Test reproducibility: deactivate, create new venv, `pip install -r requirements.txt`.
6. Add `.venv/` to `.gitignore`.

### Verification
- New venv installs all packages from requirements.txt without errors.
- Python path points to venv binary.

## Cortex Logging
- collection: procedures
- tags: mlops, training, basic

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Python venv (stdlib)

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: BASIC
