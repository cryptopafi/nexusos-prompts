---
type: procedure
created: 2026-03-17
status: active
slug: mlops-012
tags: [procedure, nexus]
---

# MLOPS-012 — Build an ML API with Flask

## Problema
ML models sitting in notebooks cannot serve predictions to production applications.

## Procedura
### Prerequisites
- Python 3.9+
- pip install flask
- a trained model pickle file

### Steps
1. Create `app.py` with Flask app and load model at startup with `pickle.load()`.
2. Define a `/predict` POST endpoint that accepts JSON input.
3. Parse input, convert to numpy/pandas format, run `model.predict()`.
4. Return prediction as JSON response with appropriate status codes.
5. Add `/health` GET endpoint returning `{'status': 'ok'}`.
6. Run with `flask run --port 5000` and test with `curl -X POST`.

### Verification
- Health endpoint returns 200.
- Predict endpoint returns valid JSON prediction for test input.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- flask
- pickle or joblib
- trained ML model

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
