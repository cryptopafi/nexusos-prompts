---
type: procedure
created: 2026-03-17
status: active
slug: mlops-033
tags: [procedure, nexus]
---

# MLOPS-033 — Build End-to-End MLOps with Hugging Face Spaces

## Problema
Sharing ML demos requires infrastructure setup that delays stakeholder feedback loops.

## Procedura
### Prerequisites
- Hugging Face account
- trained model or pipeline

### Steps
1. Create a new Space on huggingface.co: choose Gradio or Streamlit SDK.
2. Clone the Space repo: `git clone https://huggingface.co/spaces/<user>/<name>`.
3. Write `app.py` with Gradio interface wrapping your model's predict function.
4. Add `requirements.txt` with model dependencies.
5. Push to deploy: `git push` triggers automatic build.
6. Test the live demo URL and share with stakeholders.
7. Add a model card README.md with usage instructions.

### Verification
- Space deploys and loads without errors.
- Demo accepts input and returns predictions in browser.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Hugging Face Spaces
- Gradio or Streamlit

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
