---
id: ND-006
title: "Chain of Thought (CoT) Prompting"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-006: Chain of Thought (CoT) Prompting

## Obiectiv

Chain of Thought încurajează modelul să articuleze raționamentul pas cu pas, producând output-uri mai transparente, logice și verificabile. Această procedură acoperă implementarea practică cu variante Zero-Shot și Few-Shot CoT, plus integrarea cu LangChain.

## Pași

### Pas 1: Zero-Shot CoT — Cel mai simplu start
Adaugă "Let's think step by step" la orice prompt.

```
A store has 150 items. Monday they sell 30% of items. Tuesday they receive 45 new items. Wednesday they sell 20 items. How many items remain?

Let's think step by step.
```

### Pas 2: Few-Shot CoT — Cu exemple de raționament
```
Q: If a shirt costs $25 and is on 20% sale, what's the final price?
A: Let's think step by step.
   - Original price: $25
   - Discount: 20% of $25 = $5
   - Final price: $25 - $5 = $20
   The answer is $20.

Q: If 3 pizzas cost $36 and you split equally among 4 people, how much does each person pay?
A: Let's think step by step.
   - Total cost: $36
   - Per person: $36 / 4 = $9
   The answer is $9 per person.

Q: {NEW_QUESTION}
A: Let's think step by step.
```

### Pas 3: CoT cu LangChain
```python
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

cot_template = PromptTemplate(
    template="""Solve this problem step by step, showing your reasoning clearly.

Problem: {problem}

Step-by-step solution:""",
    input_variables=["problem"]
)

chain = LLMChain(llm=llm, prompt=cot_template)
result = chain.run(problem="If a car travels at 60mph for 2.5 hours...")
```

### Pas 4: Structured CoT (mai controlat)
```
Problem: {PROBLEM}

Solve using this structure:
1. GIVEN: List all known information
2. FIND: State what we need to determine
3. APPROACH: Describe the method
4. SOLVE: Show each calculation
5. VERIFY: Check the answer makes sense
6. ANSWER: State the final answer
```

### Pas 5: Identifică când CoT ajută vs. nu ajută
| Task Type | CoT Benefit |
|-----------|-------------|
| Multi-step math | High |
| Logic puzzles | High |
| Planning | High |
| Simple factual Q&A | Low/None |
| Classification (binary) | Low |
| Creative writing | Low |

### Pas 6: Combină CoT cu alte tehnici
- CoT + Role: "As a physicist, think through this step by step..."
- CoT + Few-Shot: Exemple cu raționament explicit
- CoT + Self-Consistency: Multiple CoT paths → majority vote

## Verificare

- [ ] Thought inducer inclus ("step by step" sau structured)
- [ ] Raționamentul e vizibil în output
- [ ] Fiecare pas e logic corect
- [ ] Răspunsul final e clar marcat
- [ ] Verificat pe 5+ probleme

## Instrumente

| Model | Eficacitate CoT |
|-------|----------------|
| Claude Opus | Excelent — CoT profund |
| GPT-4/5 | Excelent |
| Claude Sonnet | Foarte bun |
| Modele mici | Necesită Few-Shot CoT |

## Note

- **Când să folosești**: Reasoning, math, logic, planning, analysis, debugging.
- **Când NU**: Factual lookup; simple classification; creative generation.
- **Greșeli comune**: CoT pe task-uri triviale; ne-verificarea chain-ului; lipsa structurii.
- **Relație**: PR-022 (CoT original), ND-007 (Self-Consistency), PR-023-041 (CoT variants).
