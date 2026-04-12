---
id: ND-018
title: "Prompts for Specific Tasks"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-018: Prompts for Specific Tasks

## Obiectiv

Prompt templates optimizate pentru cele mai comune task-uri NLP: classification, summarization, NER, translation, QA, generation. Fiecare task type are pattern-uri specifice care maximizează calitatea.

## Pași

### Pas 1: Classification
```
Classify the following text into EXACTLY ONE of these categories: {CATEGORIES}

Rules:
- Choose the MOST fitting category
- If unclear, choose the closest match
- Respond with ONLY the category name

Text: "{INPUT_TEXT}"
Category:
```

### Pas 2: Summarization
```
Summarize the following {CONTENT_TYPE} in {LENGTH}.

Requirements:
- Capture the main point first
- Include key supporting details
- Maintain factual accuracy
- Use {AUDIENCE_LEVEL} language

Content:
"""
{CONTENT}
"""

Summary:
```

### Pas 3: Named Entity Recognition (NER)
```
Extract all named entities from the following text. Categorize each as: PERSON, ORGANIZATION, LOCATION, DATE, MONEY, or OTHER.

Format each as: [ENTITY] → TYPE

Text: "{INPUT_TEXT}"

Entities:
```

### Pas 4: Translation
```
Translate the following text from {SOURCE_LANG} to {TARGET_LANG}.

Requirements:
- Preserve the original tone and register
- Keep proper nouns unchanged
- Maintain formatting (paragraphs, bullet points)
- If a term has no direct translation, provide the closest equivalent with the original in parentheses

Text:
"""
{TEXT}
"""

Translation:
```

### Pas 5: Question Answering
```
Answer the following question based ONLY on the provided context. If the answer is not found in the context, say "Not found in provided context."

Context:
"""
{CONTEXT}
"""

Question: {QUESTION}

Answer:
```

### Pas 6: Text Generation (creative)
```
Write a {CONTENT_TYPE} about {TOPIC}.

Specifications:
- Length: {LENGTH}
- Tone: {TONE}
- Audience: {AUDIENCE}
- Must include: {REQUIRED_ELEMENTS}
- Style reference: {STYLE_EXAMPLE}

{CONTENT_TYPE}:
```

## Verificare

- [ ] Template potrivit pentru task type selectat
- [ ] Placeholders populate corect
- [ ] Output-ul respectă formatul specificat
- [ ] Testat pe 5+ input-uri din domeniul task-ului
- [ ] Template documentat în registru

## Instrumente

| Task | Cel mai bun model |
|------|-------------------|
| Classification | Orice LLM (Sonnet/GPT-3.5 suficient) |
| Summarization | Modele mari (Opus/GPT-4) pentru nuanță |
| NER | Modele mari sau fine-tuned |
| Translation | Modele multilinguale (Claude, GPT-4) |
| QA | Modele cu context mare + RAG |
| Generation | Depinde de calitatea dorită |

## Note

- **Când să folosești**: La orice task standard NLP; ca punct de start rapid.
- **Când NU**: Task-uri unice fără template standard; task-uri care necesită custom reasoning.
- **Greșeli comune**: Template generic pe task specific; lipsa constrângerilor; neadaptarea la domeniu.
- **Relație**: ND-004 (Zero-Shot), ND-005 (Few-Shot), ND-012 (Instructions).
