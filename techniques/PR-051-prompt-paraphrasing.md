---
id: PR-051
title: "Prompt Paraphrasing"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-051: Prompt Paraphrasing

## Obiectiv

Prompt Paraphrasing transformă prompt-ul original prin reformulare (schimbarea cuvintelor), menținând sensul. Multiple parafrazări rulează pe LLM, apoi răspunsurile sunt agregate. Util ca ensemble technique: sensibilitatea modelelor la formulare (prompt sensitivity) e transformată în avantaj prin diversitate.

## Pași

### Pas 1: Scrie prompt-ul original
```
Original: "Classify the sentiment of the following movie review as positive, negative, or neutral."
```

### Pas 2: Generează 3-5 parafrazări

**Manual:**
```
Paraphrase 1: "Determine whether this movie review expresses a positive, negative, or neutral opinion."
Paraphrase 2: "Read this movie review and identify its sentiment: positive, negative, or neutral."
Paraphrase 3: "What is the overall tone of this movie review? Choose: positive, negative, or neutral."
Paraphrase 4: "Analyze the sentiment expressed in the following film critique. Label it positive, negative, or neutral."
```

**Automat:**
```
Generate 5 paraphrases of this prompt, each using different wording but keeping the same meaning:

Original prompt: "{ORIGINAL_PROMPT}"

Paraphrases:
1.
2.
3.
4.
5.
```

### Pas 3: Rulează fiecare variantă pe același input
```python
paraphrases = [original, para1, para2, para3, para4]
responses = [llm(f"{p}\n\nReview: {test_input}") for p in paraphrases]
```

### Pas 4: Agregă rezultatele
```python
answers = [extract_answer(r) for r in responses]
final = Counter(answers).most_common(1)[0][0]
```

### Pas 5: Identifică formularea optimă
Dacă o parafrazare produce constant rezultate mai bune, adoptă-o ca prompt principal.

```python
# A/B test each paraphrase on validation set
scores = {}
for i, para in enumerate(paraphrases):
    correct = sum(1 for item in val_set if evaluate(para, item))
    scores[i] = correct / len(val_set)

best = max(scores, key=scores.get)
```

### Pas 6: Aplică la prompt optimization
Generează 10 parafrazări, testează pe 20 input-uri, selectează top 3 pentru ensemble-ul de producție.

## Verificare

- [ ] Minim 3 parafrazări generate
- [ ] Fiecare parafrazare menține sensul original
- [ ] Toate parafrazările testate pe aceleași input-uri
- [ ] Rezultatele agregate sau cea mai bună selectată
- [ ] Formularea optimă documentată

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Toate modelele | Universală — exploatează prompt sensitivity |
| LLM (generator) | Generare automată de parafrazări |

## Note

- **Când să folosești**: Optimizare prompt-uri; când output-ul variază inexplicabil; ensemble low-cost.
- **Când NU**: Prompt deja optimizat; task-uri insensibile la formulare; budget minim.
- **Greșeli comune**: Parafrazări care schimbă sensul; prea similare (nu adaugă diversitate); netestat sistematic.
- **Relație**: PR-012 (Prompt Mining), PR-044 (Max MI), PR-042 (DENSE).
