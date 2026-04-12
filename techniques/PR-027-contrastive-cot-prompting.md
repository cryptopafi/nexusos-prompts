---
id: PR-027
title: "Contrastive CoT Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-027: Contrastive CoT Prompting

## Obiectiv

Contrastive CoT include atât exemple cu raționament corect CÂT ȘI exemple cu raționament incorect în prompt. Arată modelului nu doar CUM să gândească, ci și CUM SĂ NU gândească. Exemplele contrastante ajută modelul să evite greșeli comune de reasoning.

## Pași

### Pas 1: Identifică erori comune de raționament pe task-ul tău
Ce greșeli face modelul frecvent? Clasifică: erori aritmetice, saltul peste pași, confuzia de unități, interpretarea greșită a cerințelor.

### Pas 2: Creează perechi contrastante (corect + incorect)

**Template:**
```
I'll show you examples of correct and incorrect reasoning. Learn from both.

CORRECT Example:
Q: A store sells 3 packs of 12 eggs. Each egg costs $0.50. What is the total cost?
A: ✓ First, total eggs: 3 × 12 = 36 eggs. Then cost: 36 × $0.50 = $18.00.
The answer is $18.00.

INCORRECT Example (common mistake):
Q: A store sells 3 packs of 12 eggs. Each egg costs $0.50. What is the total cost?
A: ✗ Cost per pack: 12 × $0.50 = $6.00. The answer is $6.00.
This is WRONG because it only calculated one pack instead of all three.

Now solve:
Q: {NEW_PROBLEM}
A:
```

### Pas 3: Etichetează clar corect vs. incorect
Folosește markeri vizuali clari: ✓/✗, CORRECT/INCORRECT, RIGHT/WRONG. Modelul trebuie să distingă instant.

### Pas 4: Explică DE CE raționamentul incorect e greșit
Nu doar arăta greșeala — explică eroarea. Aceasta ajută modelul să înțeleagă pattern-ul de evitat.

```
INCORRECT reasoning: ✗
"The percentage increase from 80 to 100 is 20%"
WHY this is wrong: The increase is 20 units, but percentage = (20/80) × 100 = 25%, not 20%. The mistake is dividing by the new value instead of the original.

CORRECT reasoning: ✓
"The percentage increase from 80 to 100 is (100-80)/80 × 100 = 25%"
```

### Pas 5: Alternează corect și incorect
Nu pune toate exemplele corecte primele. Alternează pentru impact maxim:
1. Corect exemplu 1
2. Incorect exemplu 1 (arată greșeala pe același tip de problemă)
3. Corect exemplu 2
4. Incorect exemplu 2

### Pas 6: Testează dacă reduce erorile specifice
Compară acuratețea cu CoT standard vs. Contrastive CoT pe 10+ probleme din aceeași categorie de eroare.

## Verificare

- [ ] Minim 1 pereche contrastantă (corect + incorect)
- [ ] Exemplele incorecte sunt etichetate clar ca WRONG/INCORRECT
- [ ] Explicație a erorii incluse (nu doar exemplul greșit)
- [ ] Erorile arătate sunt relevante pentru task (greșeli comune reale)
- [ ] Reduce rata de eroare vs. CoT standard

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Toate modelele | Universală — toate beneficiază de contrastive learning |
| Modele mici | Impact mai mare — erorile sunt mai frecvente |

## Note

- **Când să folosești**: Task-uri cu erori recurente specifice; math; reasoning logic; orice domeniu unde greșelile sunt predictibile.
- **Când NU**: Task-uri unde modelul nu face erori sistematice; token budget limitat (contrastive examples consumă mult).
- **Greșeli comune**: Exemplu incorect neetichetat (modelul îl copiază); erori inventate care nu reflectă greșeli reale; prea multe exemple contrastante (confusion).
- **Relație**: PR-022 (CoT), PR-045 (Self-Consistency), C-063 (Contrastive CoT detailed).
