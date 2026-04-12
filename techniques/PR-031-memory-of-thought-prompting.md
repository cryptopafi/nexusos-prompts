---
id: PR-031
title: "Memory-of-Thought Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-031: Memory-of-Thought Prompting

## Obiectiv

Memory-of-Thought (Li & Qiu, 2023) construiește Few-Shot CoT prompts dinamic la test time folosind exemplare neetichetate pre-procesate. Înainte de test, rulează CoT pe un set de exemplare de training (fără label-uri gold). La test time, selectează cele mai relevante CoT chains ca exemplare. Combină auto-generarea cu selecția bazată pe similaritate.

## Pași

### Pas 1: Pregătește un pool de exemplare de training (fără label-uri)
Colectează 50-200+ input-uri reprezentative pentru task-ul tău. Nu ai nevoie de label-uri — modelul le va genera.

### Pas 2: Pre-inference — Generează CoT chains pentru tot pool-ul
```python
# Phase 1: Pre-processing (offline, once)
memory = {}
for exemplar in training_pool:
    cot_response = llm(f"{exemplar}\nLet's think step by step.")
    memory[exemplar] = {
        "input": exemplar,
        "cot_chain": cot_response,
        "embedding": embed(exemplar)
    }
```

### Pas 3: La test time — Selectează cele mai relevante memorii
```python
# Phase 2: Test-time retrieval
def get_mot_prompt(test_input, k=3):
    test_embedding = embed(test_input)
    # Find k most similar training exemplars
    similarities = {
        ex: cosine_sim(test_embedding, mem["embedding"])
        for ex, mem in memory.items()
    }
    top_k = sorted(similarities, key=similarities.get, reverse=True)[:k]

    # Build prompt with retrieved CoT chains
    prompt = ""
    for ex in top_k:
        prompt += f"Q: {memory[ex]['input']}\n"
        prompt += f"A: {memory[ex]['cot_chain']}\n\n"
    prompt += f"Q: {test_input}\nA: Let's think step by step."
    return prompt
```

### Pas 4: Construiește prompt-ul final
Template-ul rezultat arată ca un Few-Shot CoT standard, dar exemplele sunt selectate dinamic din "memoria" pre-calculată.

### Pas 5: Actualizează memoria periodic
Pe măsură ce task-ul evoluează, re-generează CoT chains cu modele mai noi sau adaugă exemplare noi.

### Pas 6: Validare — Compară cu Few-Shot CoT static
Testează pe 20+ input-uri: MoT (dinamic) vs. Few-Shot CoT (static, aceleași exemple mereu). MoT ar trebui să fie superior pe input-uri diverse.

## Verificare

- [ ] Pool de training >= 50 exemplare
- [ ] CoT chains pre-generate pentru tot pool-ul
- [ ] Embeddings calculate și stocate
- [ ] Selecția la test time funcționează (top-K retrieval)
- [ ] Performanță superioară vs. Few-Shot CoT static

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM | Pre-generare CoT chains |
| Embedding model | Similaritate pentru retrieval |
| Vector store (FAISS/Chroma) | Stocare și retrieval eficient |
| Python | Pipeline orchestration |

## Note

- **Când să folosești**: Sisteme de producție cu input-uri diverse; când exemplele statice nu acoperă suficientă varietate; RAG-style CoT.
- **Când NU**: Prompting manual; task-uri simple; pool de training mic (<20).
- **Greșeli comune**: CoT chains pre-generate de calitate slabă; embedding model slab; pool prea mic; neactualizarea memoriei.
- **Relație**: PR-009 (KNN), PR-011 (SG-ICL), PR-022 (CoT), PR-032 (Auto-CoT).
