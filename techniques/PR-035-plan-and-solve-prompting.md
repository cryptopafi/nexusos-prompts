---
id: PR-035
title: "Plan-and-Solve Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-035: Plan-and-Solve Prompting

## Obiectiv

Plan-and-Solve (Wang et al., 2023) îmbunătățește Zero-Shot CoT înlocuind "Let's think step by step" cu "Let's first understand the problem and devise a plan to solve it. Then, let's carry out the plan." Forțează modelul să planifice ÎNAINTE de a executa, reducând erori de omisiune și salturi logice.

## Pași

### Pas 1: Folosește thought inducer-ul Plan-and-Solve

**Template de bază:**
```
{PROBLEM}

Let's first understand the problem and devise a plan to solve it. Then, let's carry out the plan step by step.
```

### Pas 2: Variantă PS+ (Plan-and-Solve Plus)
Adaugă constrângeri de calitate la plan:

```
{PROBLEM}

Let's first understand the problem and devise a plan to solve it. Then, let's carry out the plan step by step.
- Extract all relevant variables and their values
- Calculate intermediate results carefully
- Double-check each calculation
- State the final answer clearly
```

### Pas 3: Exemplu de output dorit
```
Problem: A car travels at 60 km/h for 2.5 hours, then at 80 km/h for 1.5 hours. What is the average speed?

Plan:
1. Calculate distance for first segment
2. Calculate distance for second segment
3. Calculate total distance
4. Calculate total time
5. Calculate average speed = total distance / total time

Execution:
1. Distance 1 = 60 × 2.5 = 150 km
2. Distance 2 = 80 × 1.5 = 120 km
3. Total distance = 150 + 120 = 270 km
4. Total time = 2.5 + 1.5 = 4 hours
5. Average speed = 270 / 4 = 67.5 km/h

Final answer: 67.5 km/h
```

### Pas 4: Aplică la task-uri non-math
```
Task: Write a comprehensive market analysis for {PRODUCT}.

Let's first understand what's needed and create a plan:

Plan:
1. Define the target market and segmentation
2. Analyze competitive landscape
3. Identify market size and growth trends
4. Assess customer needs and pain points
5. Formulate strategic recommendations

Now executing each step:
[...]
```

### Pas 5: Aplică la coding
```
Implement a function that {SPEC}.

Let's first understand the requirements and plan the implementation:

Plan:
1. Input validation — what inputs are expected, edge cases
2. Core algorithm — what approach to use
3. Error handling — what can go wrong
4. Return format — what to return
5. Tests — verify correctness

Implementation:
[...]
```

### Pas 6: Verifică aderența la plan
Citește output-ul: a urmat modelul planul? A adăugat pași neplanificați? A omis pași din plan? Planul trebuie să fie un contract pe care modelul îl respectă.

## Verificare

- [ ] "Understand + plan + execute" structure prezentă
- [ ] Planul e creat ÎNAINTE de execuție
- [ ] Fiecare pas din plan e executat
- [ ] Niciun pas din plan nu e omis
- [ ] PS+ constrângeri aplicate (dacă folosite)
- [ ] Superior vs. "step by step" simplu

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus/Sonnet | Excelent — planifică natural |
| GPT-4/5 | Excelent |
| Modele mici | Bun — PS+ ajută să structureze |

## Note

- **Când să folosești**: Orice task multi-step; project planning; coding; analiză; replacer pentru "step by step" generic.
- **Când NU**: Task-uri single-step; răspunsuri factuale scurte.
- **Greșeli comune**: Plan prea vag; planul nu e urmat; saltul direct la execuție fără plan.
- **Relație**: PR-022 (CoT), PR-033 (Least-to-Most), PR-036 (ToT).
