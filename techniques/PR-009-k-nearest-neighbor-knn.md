---
id: PR-009
title: "K-Nearest Neighbor (KNN) Exemplar Selection"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-009: K-Nearest Neighbor (KNN) Exemplar Selection

## Obiectiv

Tehnica KNN selectează automat exemplele cele mai similare cu input-ul de test dintr-un pool mare de exemple. Se generează embeddings pentru toate exemplele disponibile și se aleg cele K mai apropiate de input-ul curent. Îmbunătățește semnificativ Few-Shot prompting în sisteme automatizate.

## Pași

### Pas 1: Construiește un pool de exemple etichetate
Creează o colecție de 50-500+ exemple cu input și output corect. Cu cât pool-ul e mai mare, cu atât selecția KNN e mai utilă.

### Pas 2: Generează embeddings pentru pool
Folosește un model de embedding (SentenceTransformers, OpenAI ada-002, Cohere) pentru a genera vectori pentru fiecare exemplu.

```python
from sentence_transformers import SentenceTransformer
import numpy as np

embedder = SentenceTransformer('all-MiniLM-L6-v2')

# Pool-ul de exemple
examples = [
    {"input": "My order is late", "output": "Shipping inquiry"},
    {"input": "How do I reset password?", "output": "Account support"},
    {"input": "Product arrived broken", "output": "Returns/damage"},
    # ... 100+ more
]

# Pre-calculează embeddings
example_texts = [e["input"] for e in examples]
example_embeddings = embedder.encode(example_texts)
```

### Pas 3: La runtime, embed input-ul de test și selectează top-K
```python
def select_knn_examples(test_input, k=3):
    test_embedding = embedder.encode([test_input])
    similarities = np.dot(example_embeddings, test_embedding.T).flatten()
    top_k_idx = np.argsort(similarities)[-k:][::-1]
    return [examples[i] for i in top_k_idx]

# Usage
selected = select_knn_examples("Where is my package?", k=3)
```

### Pas 4: Construiește prompt-ul dinamic
```python
def build_prompt(test_input, selected_examples):
    prompt = "Classify each customer message into a category.\n\n"
    for ex in selected_examples:
        prompt += f'Message: "{ex["input"]}"\nCategory: {ex["output"]}\n\n'
    prompt += f'Message: "{test_input}"\nCategory:'
    return prompt
```

### Pas 5: Setează K optim
- K=3 e un default bun
- Testează K=1,3,5,7 pe un set de validare
- Mai mare K = mai mult context dar mai mulți tokens

### Pas 6: Adaugă diversitate la selecție (opțional)
După selecția KNN pură, verifică dacă exemplele selectate acoperă categorii diferite. Dacă toate 3 sunt din aceeași clasă, înlocuiește ultimul cu un exemplu din altă clasă.

## Verificare

- [ ] Pool de exemple >= 50 entries
- [ ] Embeddings pre-calculate și stocate
- [ ] K testat pe set de validare
- [ ] Selecția include diversitate de clase
- [ ] Latența selecției < 100ms
- [ ] Pipeline end-to-end testat

## Instrumente

| Instrument | Rol |
|-----------|-----|
| SentenceTransformers | Embedding generation (gratuit, local) |
| FAISS | Vector search la scară mare (>10K exemple) |
| ChromaDB | Vector store simplu |
| OpenAI Embeddings | Alternativă cloud |

## Note

- **Când să folosești**: Sisteme automatizate cu pool mare de exemple; când input-urile variază mult; RAG-style pipelines.
- **Când NU**: Prompting manual one-off; pool < 20 exemple (nu e suficient de mare pentru KNN).
- **Greșeli comune**: Pool prea mic; embedding model slab; K prea mare (noise); lipsa diversității în selecție.
- **Relație**: PR-007 (similarity manual), PR-010 (Vote-K), PR-011 (SG-ICL).
