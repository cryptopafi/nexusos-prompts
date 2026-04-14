---
type: procedure
created: 2026-03-17
status: active
slug: mlops-029
tags: [procedure, nexus]
---

# MLOPS-029 — Prototype AI APIs in Cloud Shell

## Problema
Setting up local environments for quick AI API experiments wastes time when cloud shells are available.

## Procedura
### Prerequisites
- Cloud provider account (AWS/GCP/Azure)

### Steps
1. Open Cloud Shell in your provider's console.
2. Install required Python packages: `pip install --user transformers fastapi uvicorn`.
3. Write a minimal FastAPI app with a Hugging Face pipeline endpoint.
4. Run the app with port forwarding: `uvicorn app:app --port 8080`.
5. Use Cloud Shell's web preview to test the API.
6. Save the prototype code to a git repository for future development.

### Verification
- API responds in Cloud Shell web preview.
- Code committed to repository for follow-up.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Cloud Shell
- Python
- FastAPI

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
