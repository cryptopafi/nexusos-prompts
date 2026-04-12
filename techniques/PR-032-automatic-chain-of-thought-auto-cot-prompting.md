---
id: PR-032
title: "Automatic Chain-of-Thought (Auto-CoT) Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-032: Automatic Chain-of-Thought (Auto-CoT) Prompting

## Obiectiv

Auto-CoT (Zhang et al., 2022) automatizează crearea exemplelor Few-Shot CoT. Clusterizează întrebările de training în grupuri, selectează un reprezentant din fiecare cluster, generează automat CoT chains cu Zero-Shot CoT, apoi le folosește ca exemplare. Elimină nevoia de CoT chains scrise manual.

## Pași

### Pas 1: Colectează un set de întrebări de training
Adună 50-200+ întrebări similare cu cele pe care le vei folosi în producție.

### Pas 2: Clusterizează întrebările
Grupează întrebările pe similaritate pentru a asigura diversitate în exemplele selectate.

```python
from sklearn.cluster import KMeans
from sentence_transformers import SentenceTransformer

embedder = SentenceTransformer('all-MiniLM-L6-v2')
embeddings = embedder.encode(all_questions)

# Cluster into K groups (K = number of desired exemplars)
K = 5  # will select 5 diverse exemplars
kmeans = KMeans(n_clusters=K, random_state=42)
clusters = kmeans.fit_predict(embeddings)
```

### Pas 3: Selectează un reprezentant din fiecare cluster
Alege întrebarea cea mai apropiată de centroidul clusterului (cea mai "tipică").

```python
representatives = []
for k in range(K):
    cluster_mask = clusters == k
    cluster_embeddings = embeddings[cluster_mask]
    cluster_questions = [q for q, c in zip(all_questions, clusters) if c == k]

    # Find closest to centroid
    centroid = kmeans.cluster_centers_[k]
    distances = np.linalg.norm(cluster_embeddings - centroid, axis=1)
    best_idx = np.argmin(distances)
    representatives.append(cluster_questions[best_idx])
```

### Pas 4: Generează CoT chains automat
```python
auto_cot_exemplars = []
for question in representatives:
    cot = llm(f"{question}\nLet's think step by step.")
    auto_cot_exemplars.append({
        "question": question,
        "cot_chain": cot
    })
```

### Pas 5: Construiește prompt-ul Few-Shot CoT
```python
prompt = ""
for ex in auto_cot_exemplars:
    prompt += f"Q: {ex['question']}\nA: {ex['cot_chain']}\n\n"
prompt += f"Q: {{NEW_QUESTION}}\nA: Let's think step by step."
```

### Pas 6: Validează și filtrează chains defecte
Verifică automat: CoT chain-ul conține un răspuns final? Lungimea e rezonabilă? Dacă nu, regenerează pentru acel cluster.

```python
for ex in auto_cot_exemplars:
    if not has_final_answer(ex["cot_chain"]) or len(ex["cot_chain"]) < 50:
        ex["cot_chain"] = llm(f"{ex['question']}\nLet's think step by step.", temperature=0.3)
```

## Verificare

- [ ] Set de training >= 50 întrebări
- [ ] Clusterizare aplicată (K clusters)
- [ ] Un reprezentant selectat din fiecare cluster
- [ ] CoT chains generate automat pentru fiecare reprezentant
- [ ] Chains validate (au răspuns final, lungime rezonabilă)
- [ ] Performanță comparabilă cu Few-Shot CoT manual

## Instrumente

| Instrument | Rol |
|-----------|-----|
| scikit-learn (KMeans) | Clusterizare |
| SentenceTransformers | Embeddings |
| LLM | Generare CoT chains |
| Python | Pipeline |

## Note

- **Când să folosești**: Când ai multe întrebări de training dar nu vrei să scrii CoT manual; bootstrapping rapid de prompt-uri.
- **Când NU**: Când ai deja CoT chains manuale de calitate; domenii critice unde CoT automat poate conține erori.
- **Greșeli comune**: K prea mic (lipsă diversitate); CoT chains nesupravegheate (pot conține erori); lipsa validării.
- **Relație**: PR-022 (CoT), PR-011 (SG-ICL), PR-031 (MoT), PR-030 (Active Prompting).
