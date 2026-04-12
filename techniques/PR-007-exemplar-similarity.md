---
id: PR-007
title: "Exemplar Similarity"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-007: Exemplar Similarity

## Obiectiv

Selectarea exemplelor similare cu input-ul de test îmbunătățește performanța. Când exemplele sunt semantice apropiate de noul input, modelul primește context mai relevant. Această procedură oferă strategii de selectare a exemplelor pe baza similarității.

## Pași

### Pas 1: Analizează input-ul de test
Identifică: domeniu, complexitate, structură, entități cheie, tonalitate.

### Pas 2: Selectează exemple din același domeniu
Dacă input-ul e despre cod Python, exemplele trebuie să fie tot despre cod Python — nu despre JavaScript.

**Template — exemple similare:**
```
Extract the main action from each customer support ticket.

Ticket: "My order #4521 hasn't arrived after 2 weeks."
Action: Track order #4521 and provide shipping update.

Ticket: "Order #7833 was delivered damaged, need replacement."
Action: Initiate replacement for order #7833.

Ticket: "Where is my order #9102? It's been 10 days."
Action:
```

### Pas 3: Potrivește complexitatea
Dacă input-ul de test e complex, exemplele trebuie să fie comparabil complexe. Exemple triviale nu pregătesc modelul pentru input complex.

### Pas 4: Folosește embedding similarity (pentru sisteme automatizate)
În pipelines programatice:
1. Generează embeddings pentru toate exemplele disponibile
2. Generează embedding pentru input-ul de test
3. Selectează top-K cele mai similare (cosine similarity)
4. Folosește-le ca exemplare

**Pseudo-cod:**
```python
from sentence_transformers import SentenceTransformer
import numpy as np

model = SentenceTransformer('all-MiniLM-L6-v2')
example_embeddings = model.encode(all_examples)
test_embedding = model.encode(test_input)
similarities = np.dot(example_embeddings, test_embedding)
top_k_indices = np.argsort(similarities)[-k:]
selected_examples = [all_examples[i] for i in top_k_indices]
```

### Pas 5: Echilibrează similaritate cu diversitate
Nu selecta doar exemple aproape identice — include și variații. Prea multă similaritate = modelul devine rigid.

### Pas 6: Plasează cel mai similar exemplu ultimul
Combină cu PR-003 (ordering) — cel mai relevant exemplu la final exploatează recency bias.

## Verificare

- [ ] Exemplele sunt din același domeniu ca input-ul de test
- [ ] Complexitatea exemplelor e similară cu input-ul
- [ ] Cel mai similar exemplu e plasat ultimul
- [ ] Nu toate exemplele sunt quasi-identice (menține diversitate)
- [ ] Dacă automatizat: embedding similarity > 0.7 pentru top example

## Instrumente

| Instrument | Rol |
|-----------|-----|
| SentenceTransformers | Calcul embedding similarity |
| Claude/GPT | Selecție manuală pe baza judgement |
| FAISS/ChromaDB | Vector store pentru selecție la scară |

## Note

- **Când să folosești**: Când ai un pool mare de exemple din care să alegi; când performanța pe un input specific contează.
- **Când NU**: Când ai doar 3-5 exemple disponibile (nu ai de unde alege).
- **Greșeli comune**: Exemple din domeniu diferit; exemple prea simple pentru input complex; selecție aleatorie când ai alternative.
- **Relație**: PR-009 (KNN selection), PR-010 (Vote-K), PR-003 (ordering).
