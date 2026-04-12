---
id: PR-010
title: "Vote-K Exemplar Selection"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-010: Vote-K Exemplar Selection

## Obiectiv

Vote-K (Su et al., 2022) este o metodă de selecție a exemplelor care combină similaritatea cu un mecanism de vot. Modelul propune exemplele cele mai utile dintr-un pool, apoi selecția finală se face pe baza frecvenței cu care anumite exemple sunt "votate" ca relevante pe un set de validare. E mai robust decât KNN simplu.

## Pași

### Pas 1: Pregătește pool-ul de exemple și set de validare
Pool: 50+ exemple etichetate. Set de validare: 20+ input-uri cu output-uri cunoscute.

### Pas 2: Pentru fiecare input din validare, cere modelului să aleagă cele mai utile exemple
```
Given this task input, which of the following examples would be most helpful to see before answering?

Task input: "How do I cancel my subscription?"

Available examples:
A) "Reset my password" → Account Support
B) "I want to stop my monthly plan" → Cancellation
C) "Product is defective" → Returns
D) "Change my plan to annual" → Billing
E) "Cancel auto-renewal" → Cancellation

Select the 3 most relevant examples (by letter):
```

### Pas 3: Agregă voturile pe tot setul de validare
Numără de câte ori fiecare exemplu e selectat. Exemplele cu cele mai multe voturi sunt "universal utile."

### Pas 4: Construiește pool-ul filtrat
Păstrează doar exemplele cu voturi > threshold (ex: selectate în >30% din cazuri). Elimină exemplele care nu sunt niciodată selectate.

### Pas 5: La runtime, combină Vote-K cu KNN
1. Filtrează pool-ul prin Vote-K (elimină exemple inutile)
2. Din pool-ul filtrat, aplică KNN pentru selecția finală
3. Construiește prompt-ul cu exemplele selectate

**Template:**
```python
# Pool filtrat prin Vote-K
voted_pool = [ex for ex in all_examples if ex["vote_count"] > threshold]

# KNN pe pool-ul filtrat
selected = knn_select(test_input, voted_pool, k=3)

# Build prompt
prompt = build_few_shot_prompt(selected, test_input)
```

### Pas 6: Re-evaluează periodic
Pe măsură ce task-ul evoluează, re-rulează procesul de vot cu input-uri noi.

## Verificare

- [ ] Pool >= 50 exemple
- [ ] Set de validare >= 20 input-uri
- [ ] Voturi agregate pe tot setul de validare
- [ ] Threshold de filtrare setat
- [ ] Pipeline-ul Vote-K + KNN funcțional end-to-end
- [ ] Performanța comparată cu KNN simplu (Vote-K trebuie să fie >= KNN)

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM (Claude/GPT) | Procesul de vot |
| Python | Agregare voturi, pipeline |
| SentenceTransformers | KNN post-filtrare |

## Note

- **Când să folosești**: Pipeline-uri automatizate cu pool mare; când KNN singur nu e suficient de bun; când exemplele au calitate variabilă.
- **Când NU**: Prompting manual; pool < 30 exemple; one-off tasks.
- **Greșeli comune**: Threshold prea agresiv (elimini prea multe exemple); set de validare prea mic; re-evaluare insuficientă.
- **Relație**: PR-009 (KNN), PR-007 (similarity), PR-011 (SG-ICL).
