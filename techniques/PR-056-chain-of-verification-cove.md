---
id: PR-056
title: "Chain-of-Verification (CoVe)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-056: Chain-of-Verification (CoVe)

## Obiectiv

CoVe (Dhuliawala et al., 2023) combate halucinările prin: (1) generează un răspuns inițial, (2) generează întrebări de verificare pentru fiecare claim din răspuns, (3) răspunde independent la fiecare întrebare de verificare, (4) revizuiește răspunsul original pe baza verificărilor. Systematic fact-checking.

## Pași

### Pas 1: Generează răspunsul inițial
```
Question: Name some famous scientists born in Poland.
Initial answer: Marie Curie, Nicolaus Copernicus, Frédéric Chopin, Albert Einstein
```

### Pas 2: Generează întrebări de verificare pentru fiecare claim

**Template:**
```
I gave this answer: "{INITIAL_ANSWER}"

Generate a verification question for EACH specific claim in this answer:

1. Verification Q: "Was Marie Curie born in Poland?"
2. Verification Q: "Was Nicolaus Copernicus born in Poland?"
3. Verification Q: "Was Frédéric Chopin born in Poland?"
4. Verification Q: "Was Albert Einstein born in Poland?"
```

### Pas 3: Răspunde la fiecare întrebare de verificare INDEPENDENT
Crucial: fiecare verificare se face fără a vedea răspunsul original (evită confirmation bias).

```
Q: Was Frédéric Chopin born in Poland?
A: Yes, Chopin was born in Poland (however, he was a composer, not a scientist).

Q: Was Albert Einstein born in Poland?
A: No, Einstein was born in Ulm, Germany.
```

### Pas 4: Revizuiește răspunsul pe baza verificărilor

**Template Revision:**
```
Original answer: "Marie Curie, Nicolaus Copernicus, Frédéric Chopin, Albert Einstein"

Verification results:
1. Marie Curie — born in Poland ✓, scientist ✓
2. Copernicus — born in Poland ✓, scientist ✓
3. Chopin — born in Poland ✓, but NOT a scientist ✗ (composer)
4. Einstein — NOT born in Poland ✗ (born in Germany)

Revised answer (removing incorrect claims):
"Marie Curie and Nicolaus Copernicus. Chopin was born in Poland but was a composer, not a scientist. Einstein was born in Germany, not Poland."
```

### Pas 5: Implementare completă
```python
def chain_of_verification(question):
    # Step 1: Initial answer
    answer = llm(f"Answer: {question}")

    # Step 2: Generate verification questions
    verif_qs = llm(f"""
        Answer: {answer}
        Generate a verification question for each claim:
    """)

    # Step 3: Answer verifications independently
    verifications = []
    for vq in parse_questions(verif_qs):
        # Independent verification (no access to original answer)
        result = llm(f"Answer this factual question: {vq}")
        verifications.append(result)

    # Step 4: Revise
    revised = llm(f"""
        Original answer: {answer}
        Verification results: {verifications}
        Revise the answer, removing or correcting any claims that failed verification:
    """)
    return revised
```

### Pas 6: Aplică la outputs cu risc de halucinare
CoVe e esențial pentru: liste (modelul tinde să includă items greșite), biografii, date istorice, claims factuale specifice.

## Verificare

- [ ] Răspuns inițial generat
- [ ] Întrebări de verificare generate pentru fiecare claim
- [ ] Verificările făcute INDEPENDENT (fără contaminare)
- [ ] Răspunsul revizuit reflectă rezultatele verificărilor
- [ ] Claims false eliminate sau corectate

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — verificare riguroasă |
| GPT-4/5 | Excelent |
| + Search API | Ideal — verificare pe fapte reale |

## Note

- **Când să folosești**: Fact-checking; liste; orice output cu claims verificabile; anti-halucinare.
- **Când NU**: Opinii; text creativ; task-uri unde claims nu sunt verificabile.
- **Greșeli comune**: Verificări non-independente (vede răspunsul original → confirmation bias); verificări superficiale; nu toți claims verificați.
- **Relație**: PR-054 (RCoT), PR-055 (Self-Verification), PR-053 (Self-Refine).
