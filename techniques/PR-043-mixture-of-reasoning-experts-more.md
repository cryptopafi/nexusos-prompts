---
id: PR-043
title: "Mixture of Reasoning Experts (MoRE)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-043: Mixture of Reasoning Experts (MoRE)

## Obiectiv

MoRE (Si et al., 2023) creează un set de "experți de raționament" — fiecare expert folosește un tip diferit de prompting specializat: retrieval augmentation pentru factual, CoT pentru math/logic, generated knowledge pentru commonsense. Fiecare expert rezolvă problema, apoi se selectează cel mai bun răspuns.

## Pași

### Pas 1: Definește tipurile de raționament necesare
Categorii standard:
- **Factual**: Retrieval-Augmented Generation (RAG)
- **Math/Logic**: Chain-of-Thought (CoT)
- **Commonsense**: Generated Knowledge Prompting
- **Analytical**: Step-Back + Plan-and-Solve
- **Creative**: Role + Style Prompting

### Pas 2: Creează un prompt specializat per expert

**Expert 1 — Factual (RAG-style):**
```
Search for and cite specific facts to answer this question. Only include verifiable information.
Question: {Q}
```

**Expert 2 — Mathematical/Logical (CoT):**
```
Solve this step by step, showing all calculations and logical deductions.
Question: {Q}
```

**Expert 3 — Commonsense (Generated Knowledge):**
```
First, generate relevant background knowledge about the topic. Then use that knowledge to answer.
Question: {Q}
```

### Pas 3: Rulează toți experții pe aceeași problemă
```python
def more_solve(question, experts):
    results = {}
    for name, prompt_template in experts.items():
        prompt = prompt_template.format(Q=question)
        results[name] = llm(prompt)
    return results
```

### Pas 4: Selectează cel mai bun răspuns
Opțiuni de selecție:
- **Majority vote**: Dacă 2/3 experți concordă, acel răspuns
- **Router**: Clasifică întâi tipul de problemă, selectează expertul potrivit
- **Meta-evaluator**: Un prompt final evaluează răspunsurile

```
I have 3 expert answers to this question:
Question: {Q}

Expert 1 (Factual): {answer_1}
Expert 2 (Logical): {answer_2}
Expert 3 (Commonsense): {answer_3}

Evaluate each answer for accuracy and completeness. Select the best one and explain why.
Best answer:
```

### Pas 5: Implementare cu router (eficient)
```python
def classify_question_type(question):
    classification = llm(f"""
    Classify this question type: {question}
    Types: factual, mathematical, commonsense, analytical
    Type:
    """)
    return classification.strip()

def more_with_router(question):
    q_type = classify_question_type(question)
    expert = experts[q_type]
    return llm(expert.format(Q=question))
```

### Pas 6: Evaluează acuratețea per expert
Monitorizează care expert "câștigă" cel mai des. Dacă un expert nu câștigă niciodată, elimină-l sau îmbunătățește-l.

## Verificare

- [ ] Minim 2 experți cu prompt-uri distincte
- [ ] Fiecare expert folosește un stil de raționament diferit
- [ ] Mecanism de selecție a răspunsului final definit
- [ ] Performanță superioară vs. single expert
- [ ] Router funcțional (dacă folosit)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus/Sonnet | Excelent — switch-ul între stiluri e natural |
| GPT-4/5 | Excelent |
| Multi-model | Ideal — fiecare expert pe modelul optim |

## Note

- **Când să folosești**: Probleme care combină factual + logic + commonsense; sisteme QA robuste; task-uri cu tipuri variate.
- **Când NU**: Task-uri cu un singur tip de raționament clar; budget limitat.
- **Greșeli comune**: Experți prea similari; router slab; evaluator biased.
- **Relație**: PR-042 (DENSE), PR-045 (Self-Consistency), PR-048 (DiVeRSe).
