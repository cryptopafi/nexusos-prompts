---
type: procedure
created: 2026-03-17
status: active
slug: mlops-087
tags: [procedure, nexus]
---

# MLOPS-087 — Deploy Hugging Face Spaces Applications

## Problema
ML demos need public hosting for stakeholder review without infrastructure management.

## Procedura
### Prerequisites
- Hugging Face account
- Gradio or Streamlit app

### Steps
1. Create Space: `huggingface-cli repo create my-demo --type space --space-sdk gradio`.
2. Clone: `git clone https://huggingface.co/spaces/<user>/my-demo`.
3. Write `app.py` with Gradio interface for your model.
4. Add `requirements.txt` with all dependencies.
5. Push: `git add . && git commit -m 'Initial app' && git push`.
6. Monitor build logs in Space settings.
7. Share the Space URL for stakeholder feedback.

### Verification
- Space builds and deploys without errors.
- Demo accessible via public URL.

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
