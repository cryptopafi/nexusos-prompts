---
id: PR-055
title: "Self-Verification"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-055: Self-Verification

## Obiectiv

Self-Verification (Weng et al., 2022) generează multiple soluții candidat cu CoT, apoi verifică fiecare candidat substituind răspunsul înapoi în problemă pentru a verifica consistența. Soluțiile care "trec" verificarea sunt prioritizate. Backward verification: dacă răspunsul e corect, trebuie să satisfacă condițiile problemei.

## Pași

### Pas 1: Generează N soluții candidat
```python
candidates = []
for _ in range(N):
    solution = llm(f"{problem}\nLet's solve step by step.", temperature=0.7)
    candidates.append(solution)
```

### Pas 2: Pentru fiecare candidat, verifică prin substituție

**Template Verification:**
```
Problem: "A number plus 15 equals 42. What is the number?"
Candidate answer: 27

Verification: Substitute the answer back into the problem.
27 + 15 = 42 ✓
The answer checks out.
```

```
Problem: "If 3 workers can build a wall in 12 hours, how long for 4 workers?"
Candidate answer: 9 hours

Verification: 3 workers × 12 hours = 36 worker-hours total.
4 workers × 9 hours = 36 worker-hours. ✓
The answer is consistent.
```

### Pas 3: Implementare automată
```python
def self_verify(problem, candidates):
    verified = []
    for candidate in candidates:
        answer = extract_answer(candidate)
        check = llm(f"""
            Problem: {problem}
            Proposed answer: {answer}

            Substitute this answer back into the problem conditions.
            Does it satisfy ALL conditions? Answer PASS or FAIL with explanation.
        """)
        if "PASS" in check:
            verified.append(candidate)

    if verified:
        return verified[0]  # Return first verified solution
    else:
        return candidates[0]  # Fallback to first if none verify
```

### Pas 4: Variantă single-prompt
```
{PROBLEM}

Step 1: Solve the problem.
[solution]
Answer: [answer]

Step 2: Verify by substituting the answer back into the original problem.
Check: [substitute and verify]
Result: [PASS/FAIL]

Step 3: If FAIL, re-solve with the error identified.
```

### Pas 5: Aplică la non-math tasks
- **Code**: Rulează codul pe test cases → verifică output
- **Logic**: Substituie concluzia înapoi în premise → verifică consistența
- **Constraints**: Verifică dacă răspunsul respectă toate constrângerile din problem statement

### Pas 6: Selectează cel mai bun candidat verificat
Dacă multiple candidați trec verificarea, selectează cel cu raționamentul cel mai clar sau cel care apare cel mai frecvent.

## Verificare

- [ ] Multiple soluții candidat generate (N >= 3)
- [ ] Fiecare candidat verificat prin substituție
- [ ] Verificarea e riguroasă (toate condițiile check-uite)
- [ ] Candidatele FAIL sunt eliminate
- [ ] Candidatul verificat selectat ca răspuns final

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Toate modelele | Universală — verificarea prin substituție e model-agnostic |
| + Code execution | Ideal pentru verificare automată |

## Note

- **Când să folosești**: Math, logic, constraint satisfaction, code verification — orice cu condiții verificabile.
- **Când NU**: Task-uri fără condiții verificabile (creative, opinion); task-uri unde substituția nu e posibilă.
- **Greșeli comune**: Verificare superficială; nu toate condițiile check-uite; confusion dacă niciun candidat nu trece.
- **Relație**: PR-054 (RCoT), PR-056 (CoVe), PR-045 (Self-Consistency).
