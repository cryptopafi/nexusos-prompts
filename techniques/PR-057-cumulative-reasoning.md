---
id: PR-057
title: "Cumulative Reasoning"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-057: Cumulative Reasoning

## Obiectiv

Cumulative Reasoning (Zhang et al., 2023) generează mai mulți pași potențiali, evaluează fiecare, și acumulează doar pașii validați într-un chain de raționament. Un "verifier" aprobă sau respinge fiecare pas candidat. Raționamentul crește treptat, acumulând doar pași verificați.

## Pași

### Pas 1: Generează pașii candidat (Proposer)
```
Problem: {PROBLEM}

Current verified reasoning so far:
{ACCUMULATED_STEPS}

Generate 3 possible next steps in the reasoning:
Step A: [...]
Step B: [...]
Step C: [...]
```

### Pas 2: Verifică fiecare pas candidat (Verifier)
```
Problem: {PROBLEM}
Verified reasoning so far: {ACCUMULATED_STEPS}

Proposed next step: "{CANDIDATE_STEP}"

Is this step logically correct given the problem and previous steps?
- Is the logic sound?
- Are any calculations correct?
- Does it follow from previous steps?

Verdict: [ACCEPT/REJECT] — Reason: [...]
```

### Pas 3: Acumulează pașii acceptați
```python
def cumulative_reasoning(problem, max_steps=10):
    accumulated = []

    for _ in range(max_steps):
        # Propose candidates
        candidates = llm(f"""
            Problem: {problem}
            Verified steps so far: {accumulated}
            Generate 3 possible next steps:
        """)

        # Verify each
        accepted = None
        for candidate in parse_candidates(candidates):
            verdict = llm(f"""
                Problem: {problem}
                Verified so far: {accumulated}
                Proposed: {candidate}
                Is this step correct? ACCEPT or REJECT:
            """)
            if "ACCEPT" in verdict:
                accepted = candidate
                break

        if accepted:
            accumulated.append(accepted)
        else:
            break  # No valid step found → done or stuck

        # Check if we've reached a conclusion
        if is_final_answer(accepted):
            break

    return accumulated
```

### Pas 4: Variantă single-prompt (simplificată)
```
{PROBLEM}

Build your answer one verified step at a time. For each step:
1. Propose the step
2. Verify it's correct
3. Only if correct, add it to your reasoning chain

Step 1 (proposed): [...]
Verification: [correct/incorrect because...]
Status: [ADDED/REJECTED]

Step 2 (proposed): [...]
Verification: [...]
Status: [...]

[Continue until problem is solved]

Accumulated verified reasoning:
[only the ADDED steps]

Final answer:
```

### Pas 5: Aplică la probleme cu multiple căi
Cumulative Reasoning e puternic pe probleme unde un pas greșit invalidează tot raționamentul (math proofs, logic chains, legal arguments).

### Pas 6: Monitorizează rata de respingere
Dacă >50% din pași sunt respinși, problema e prea dificilă pentru modelul curent sau prompt-ul trebuie reformulat.

## Verificare

- [ ] Fiecare pas propus este verificat independent
- [ ] Doar pașii acceptați sunt acumulați
- [ ] Niciun pas respins nu apare în chain-ul final
- [ ] Raționamentul final e consistent
- [ ] Răspunsul derivă din pașii acumulați

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — verificare riguroasă |
| GPT-4/5 | Excelent |
| Dual model (Sonnet propose + Opus verify) | Cost-eficient |

## Note

- **Când să folosești**: Math proofs; legal reasoning; orice unde un pas greșit e catastrofal; high-stakes reasoning.
- **Când NU**: Task-uri simple; când viteza e prioritară; task-uri creative.
- **Greșeli comune**: Verifier prea permisiv (acceptă totul); verifier prea strict (respinge totul); neconvergență.
- **Relație**: PR-022 (CoT), PR-036 (ToT), PR-055 (Self-Verification), PR-053 (Self-Refine).
