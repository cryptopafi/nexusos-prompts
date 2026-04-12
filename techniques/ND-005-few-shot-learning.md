---
id: ND-005
title: "Few-Shot Learning and In-Context Learning"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-005: Few-Shot Learning and In-Context Learning

## Obiectiv

Few-Shot Learning demonstrează task-ul prin câteva exemple înainte de a prezenta input-ul real. Modelul "învață" pattern-ul in-context (fără modificarea weights-urilor). Această procedură acoperă implementarea practică, inclusiv cu LangChain.

## Pași

### Pas 1: Pregătește exemple de calitate
Fiecare exemplu trebuie să fie: corect, reprezentativ, neambiguu, consistent ca format.

```
# Bun — exemple clare:
Input: "The movie was absolutely fantastic!" → Sentiment: Positive
Input: "I wasted 2 hours of my life." → Sentiment: Negative
Input: "It was an average film." → Sentiment: Neutral
```

### Pas 2: Construiește prompt-ul Few-Shot manual
```
Classify the sentiment of customer reviews.

Review: "Best purchase I've ever made! Works perfectly."
Sentiment: Positive

Review: "Arrived broken. Customer service was unhelpful."
Sentiment: Negative

Review: "Does what it says. Nothing more, nothing less."
Sentiment: Neutral

Review: "{NEW_REVIEW}"
Sentiment:
```

### Pas 3: Implementare cu LangChain (Python)
```python
from langchain.prompts import FewShotPromptTemplate, PromptTemplate

examples = [
    {"review": "Best purchase ever!", "sentiment": "Positive"},
    {"review": "Terrible product.", "sentiment": "Negative"},
    {"review": "It's okay.", "sentiment": "Neutral"},
]

example_template = PromptTemplate(
    template="Review: {review}\nSentiment: {sentiment}",
    input_variables=["review", "sentiment"]
)

few_shot = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_template,
    prefix="Classify the sentiment of customer reviews.\n",
    suffix="\nReview: {input}\nSentiment:",
    input_variables=["input"]
)

prompt = few_shot.format(input="Great quality for the price!")
```

### Pas 4: Dynamic example selection (LangChain)
```python
from langchain.prompts.example_selector import SemanticSimilarityExampleSelector
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings

selector = SemanticSimilarityExampleSelector.from_examples(
    examples, OpenAIEmbeddings(), Chroma, k=3
)

dynamic_prompt = FewShotPromptTemplate(
    example_selector=selector,
    example_prompt=example_template,
    prefix="Classify sentiment:\n",
    suffix="\nReview: {input}\nSentiment:",
    input_variables=["input"]
)
```

### Pas 5: Optimizează exemplele
Aplică PR-002 la PR-008 (quantity, ordering, distribution, quality, format, similarity, instructions) pentru a maximiza performanța.

### Pas 6: Evaluează și iterează
Testează pe 20+ input-uri. Dacă acuratețea < 85%, ajustează exemplele (nu instrucțiunile — exemplele contează mai mult în Few-Shot).

## Verificare

- [ ] 2-5 exemple de calitate incluse
- [ ] Formatul consistent între exemple
- [ ] Testat pe 10+ input-uri
- [ ] Acuratețe >= 85% sau justificare pentru mai puțin
- [ ] Dynamic selection implementat (dacă pool mare)

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LangChain | FewShotPromptTemplate, ExampleSelector |
| Claude/GPT API | Execuție |
| SentenceTransformers | Dynamic selection embeddings |

## Note

- **Când să folosești**: Când Zero-Shot nu e suficient; task-uri cu format specific; clasificare; extracție.
- **Când NU**: Task-uri triviale (Zero-Shot suficient); task-uri de reasoning pur (adaugă CoT).
- **Greșeli comune**: Exemple de calitate slabă; format inconsistent; prea puține exemple per clasă.
- **Relație**: PR-001-013 (Few-Shot design decisions), ND-006 (CoT), ND-004 (Zero-Shot).
