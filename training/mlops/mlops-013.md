---
type: procedure
created: 2026-03-17
status: active
slug: mlops-013
tags: [procedure, nexus]
---

# MLOPS-013 — Build an ML API with FastAPI

## Problema
Flask APIs lack automatic validation, async support, and auto-generated docs needed for production ML services.

## Procedura
### Prerequisites
- Python 3.9+
- pip install fastapi uvicorn
- a trained model

### Steps
1. Create `main.py` with FastAPI app and Pydantic request/response models.
2. Define input schema: `class PredictRequest(BaseModel): features: List[float]`.
3. Load model at startup using FastAPI lifespan or global scope.
4. Create `/predict` POST endpoint with type-annotated request body.
5. Add `/health` and `/docs` endpoints (docs auto-generated).
6. Run with `uvicorn main:app --reload` and test at `http://localhost:8000/docs`.

### Verification
- Swagger UI accessible at /docs.
- POST to /predict returns typed JSON response.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- fastapi
- uvicorn
- pydantic

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
