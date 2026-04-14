---
type: procedure
created: 2026-03-17
status: active
slug: mlops-032
tags: [procedure, nexus]
---

# MLOPS-032 — Operationalize a Flask ML Microservice

## Problema
Development Flask servers cannot handle production traffic, logging, or monitoring requirements.

## Procedura
### Prerequisites
- Flask ML API (MLOPS-012)
- gunicorn

### Steps
1. Replace `flask run` with Gunicorn: `gunicorn -w 4 -b 0.0.0.0:5000 app:app`.
2. Add structured JSON logging with `python-json-logger`.
3. Implement request validation and error handling middleware.
4. Add `/metrics` endpoint exposing request count, latency, and error rates.
5. Configure health check endpoint for load balancer integration.
6. Write a `Procfile` for Heroku-style deployment: `web: gunicorn app:app`.

### Verification
- Service handles concurrent requests under Gunicorn.
- Metrics endpoint returns request statistics.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Flask
- Gunicorn
- python-json-logger
- MLOPS-012

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
