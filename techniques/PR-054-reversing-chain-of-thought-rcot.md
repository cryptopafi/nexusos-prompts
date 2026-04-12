---
id: PR-054
title: "Reversing Chain-of-Thought (RCoT)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-054: Reversing Chain-of-Thought (RCoT)

## Obiectiv

RCoT (Xue et al., 2023) verifică un răspuns cerându-i modelului să RECONSTRUIASCĂ problema originală din răspuns. Dacă problema reconstruită se potrivește cu originala, răspunsul e probabil corect. Dacă nu, există o eroare. Verificare prin inversare.

## Pași

### Pas 1: Obține răspunsul cu CoT standard
```
Problem: {ORIGINAL_PROBLEM}
Let's think step by step.
[CoT reasoning]
Answer: {ANSWER}
```

### Pas 2: Cere reconstrucția problemei din răspuns

**Template RCoT:**
```
Given this answer and reasoning, reconstruct what the original problem must have been:

Reasoning: {COT_CHAIN}
Answer: {ANSWER}

What problem was being solved? Write it out:
```

### Pas 3: Compară problema reconstruită cu originala
```
Original problem: {ORIGINAL}
Reconstructed problem: {RECONSTRUCTED}

Are these the same problem? If not, what's different?
This difference likely indicates an error in the reasoning.

Comparison:
- Same: [aspects that match]
- Different: [aspects that don't match]
- Likely error: [what went wrong]
```

### Pas 4: Variantă single-prompt
```
Problem: {PROBLEM}

Step 1: Solve this step by step.
[reasoning]
Answer: [answer]

Step 2: Now, using ONLY the answer and reasoning above, reconstruct what the original problem was.
Reconstructed problem: [...]

Step 3: Compare the reconstructed problem with the original. Do they match?
- If YES: The answer is likely correct.
- If NO: There's an error. Identify it and re-solve.
```

### Pas 5: Exemplu complet
```
Problem: "A train travels 120 km at 60 km/h, then 80 km at 40 km/h. What's the average speed?"

Solve:
Total distance = 120 + 80 = 200 km
Time 1 = 120/60 = 2h, Time 2 = 80/40 = 2h, Total time = 4h
Average speed = 200/4 = 50 km/h

Reconstruct from answer:
"Something traveled 200 km total in 4 hours at an average speed of 50 km/h"
→ Matches original intent ✓ → Answer likely correct

[If the model had incorrectly averaged 60+40/2=50, reconstruction would reveal:]
"Something traveled at a speed that's the mean of two speeds"
→ Doesn't match! Original asks about distance-weighted average, not simple mean.
```

### Pas 6: Automatizează la scară
```python
def rcot_verify(problem, cot_answer):
    reconstructed = llm(f"From this answer, reconstruct the original problem:\n{cot_answer}")
    comparison = llm(f"""
        Original: {problem}
        Reconstructed: {reconstructed}
        Do these match? Answer YES or NO with explanation.
    """)
    return "YES" in comparison
```

## Verificare

- [ ] Răspunsul CoT obținut
- [ ] Problema reconstruită din răspuns
- [ ] Comparația originală vs. reconstruită efectuată
- [ ] Dacă nu se potrivesc, eroarea identificată
- [ ] Dacă eroare, re-solvat corect

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent la reconstrucție și comparație |
| GPT-4/5 | Excelent |
| Orice LLM | Funcționează, dar calitatea reconstrucției variază |

## Note

- **Când să folosești**: Verificarea răspunsurilor pe math/logic; QA cu validare; audit trail.
- **Când NU**: Task-uri creative (reconstrucția nu e determinatică); task-uri triviale.
- **Greșeli comune**: Reconstrucție prea vagă; comparația superficială; presupunerea că match = corect (ambele pot fi greșite consistent).
- **Relație**: PR-055 (Self-Verification), PR-056 (CoVe), PR-017 (S2A).
