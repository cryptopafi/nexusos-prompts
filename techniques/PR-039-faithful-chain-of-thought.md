---
id: PR-039
title: "Faithful Chain-of-Thought"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-039: Faithful Chain-of-Thought

## Obiectiv

Faithful CoT (Lyu et al., 2023) generează un chain de raționament care combină limbaj natural cu limbaj simbolic/formal (cod, logică, matematică). Partea simbolică e executabilă și verificabilă, asigurând că raționamentul e "faithful" (fidel) — nu doar plauzibil, ci demontrabil corect.

## Pași

### Pas 1: Cere output hibrid — text + formal

**Template:**
```
Solve this problem using a combination of natural language reasoning AND formal notation (math/code/logic). The formal parts should be executable to verify correctness.

Problem: {PROBLEM}

Natural language reasoning:
[explain the approach in plain language]

Formal verification:
[express key steps in math/code/logic that can be checked]

Final answer:
```

### Pas 2: Exemplu — Math cu notație formală
```
Problem: A rectangular garden is 3 meters longer than it is wide. If the perimeter is 26 meters, what are the dimensions?

Reasoning: The garden has width w and length w+3. The perimeter formula gives us an equation to solve.

Formal:
Let w = width
Length = w + 3
Perimeter: 2(w) + 2(w + 3) = 26
2w + 2w + 6 = 26
4w = 20
w = 5

Verification: width=5, length=8, perimeter = 2(5) + 2(8) = 10 + 16 = 26 ✓

Answer: Width = 5m, Length = 8m
```

### Pas 3: Exemplu — Logic cu notație formală
```
Problem: All employees in Department A have security clearance. John is in Department A. Does John have security clearance?

Reasoning: This is a syllogism — if all members of a set have property P, and x is a member of that set, then x has property P.

Formal logic:
∀x (DeptA(x) → SecurityClearance(x))
DeptA(John)
∴ SecurityClearance(John)

Verification: Modus ponens applied correctly ✓

Answer: Yes, John has security clearance.
```

### Pas 4: Exemplu — Cu cod verificabil
```
Problem: What is the probability of getting at least 2 heads in 5 fair coin flips?

Reasoning: We need 1 minus the probability of 0 or 1 heads. Use binomial distribution.

Formal (Python):
```python
from math import comb
# P(X >= 2) = 1 - P(X=0) - P(X=1)
p_0 = comb(5,0) * (0.5**0) * (0.5**5)  # = 0.03125
p_1 = comb(5,1) * (0.5**1) * (0.5**4)  # = 0.15625
p_at_least_2 = 1 - p_0 - p_1  # = 0.8125
print(f"P(X >= 2) = {p_at_least_2}")  # 0.8125
```

Answer: 81.25% probability
```

### Pas 5: Verifică partea formală
Partea simbolică trebuie să fie corectă independent de textul natural. Dacă codul/formula dă un rezultat diferit de text, codul e sursa de adevăr.

### Pas 6: Alege nivelul de formalitate per task
- Math: formule algebrice + cod Python
- Logic: notație predicat + truth tables
- Code: pseudocod + implementare reală
- Data: SQL queries + rezultate așteptate

## Verificare

- [ ] Output-ul conține ATÂT text CÂT ȘI formal
- [ ] Partea formală e corectă și executabilă
- [ ] Textul natural e consistent cu partea formală
- [ ] Dacă există discrepanță, partea formală prevalează
- [ ] Verificarea e explicită (✓/✗)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — ambele registre (text + formal) |
| GPT-4/5 | Excelent |
| + Code execution | Ideal — verificare automată |

## Note

- **Când să folosești**: Task-uri unde corectitudinea e critică; math, logic, data analysis; audit trails.
- **Când NU**: Task-uri pur creative; când audiența nu înțelege notație formală.
- **Greșeli comune**: Parte formală decorativă (nu verificată); text și formal inconsistente; notație greșită.
- **Relație**: PR-022 (CoT), PR-038 (Program-of-Thoughts), PR-026 (Tab-CoT).
