---
id: PR-029
title: "Complexity-based Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-029: Complexity-based Prompting

## Obiectiv

Complexity-based Prompting (Fu et al., 2023) face două modificări la CoT: (1) selectează exemplele cu cele mai complexe chain-uri de raționament (mai mulți pași = mai bine) și (2) la Self-Consistency, dă prioritate răspunsurilor cu raționament mai complex. Premisa: raționamentul mai detaliat tinde să fie mai corect.

## Pași

### Pas 1: Generează multiple CoT chains pentru aceleași probleme exemplar
Rulează fiecare problemă de exemplu de 3-5 ori cu temperature > 0. Obține chain-uri de raționament de lungimi diferite.

### Pas 2: Selectează chain-urile cele mai complexe ca exemple
Măsoară complexitatea: număr de pași de raționament, număr de operații intermediare.

```python
def complexity_score(chain):
    """Count reasoning steps in a chain"""
    steps = chain.count("\n- ") + chain.count("\nStep")
    operations = len(re.findall(r'[+\-*/=]', chain))
    return steps + operations

# Select top-K most complex chains as exemplars
chains_sorted = sorted(all_chains, key=complexity_score, reverse=True)
selected_exemplars = chains_sorted[:K]
```

### Pas 3: Construiește prompt-ul cu exemplele cele mai complexe

**Template:**
```
Solve each problem with detailed step-by-step reasoning. Show ALL intermediate steps.

Q: {EXEMPLAR_1_QUESTION}
A: {MOST_COMPLEX_CHAIN_FOR_EXEMPLAR_1}

Q: {EXEMPLAR_2_QUESTION}
A: {MOST_COMPLEX_CHAIN_FOR_EXEMPLAR_2}

Q: {NEW_PROBLEM}
A:
```

### Pas 4: La Self-Consistency, prioritizează complexity
Când generezi N răspunsuri, nu selecta doar majority — pondereaza cu complexitatea:

```python
def weighted_vote(responses):
    scored = [(r, complexity_score(r["chain"])) for r in responses]
    # Group by answer
    answer_scores = defaultdict(float)
    for r, score in scored:
        answer_scores[r["answer"]] += score
    return max(answer_scores, key=answer_scores.get)
```

### Pas 5: Aplicare practică (fără programare)
```
Solve this problem. Show the MOST detailed reasoning possible — include every intermediate calculation, every logical step, and explain your reasoning for each step.

{PROBLEM}

Detailed solution:
```

### Pas 6: Evită complexitatea artificială
Complexitatea trebuie să fie genuină (pași necesari), nu artificială (verbose fără substanță).

## Verificare

- [ ] Exemplele selectate au chain-uri complexe (mai mulți pași)
- [ ] Complexitatea e genuină, nu verbose artificială
- [ ] Self-Consistency ponderată cu complexity score
- [ ] Acuratețe superioară vs. CoT cu exemple random
- [ ] Balanța: complex dar nu excesiv

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API | Generare multiple chains |
| Python | Complexity scoring + weighted voting |

## Note

- **Când să folosești**: Math complexă; multi-step reasoning; sisteme automatizate cu Self-Consistency.
- **Când NU**: Task-uri simple; când complexitatea adaugă noise; token budget limitat.
- **Greșeli comune**: Confuzia între complex și verbose; exemplu cu mulți pași dar pași greșiți; over-weighting complexity.
- **Relație**: PR-022 (CoT), PR-045 (Self-Consistency), PR-028 (Uncertainty-Routed).
