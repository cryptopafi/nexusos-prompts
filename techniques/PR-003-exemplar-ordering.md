---
id: PR-003
title: "Exemplar Ordering"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-003: Exemplar Ordering

## Obiectiv

Ordinea exemplelor într-un Few-Shot prompt afectează semnificativ performanța modelului. Modele LLM au recency bias (favorizează ultimele exemple) și primacy bias (primele exemple setează context-ul). Această procedură oferă strategii de ordonare pentru maximizarea calității.

## Pași

### Pas 1: Identifică tipul de task
- **Clasificare**: ordinea afectează distribuția percepută a label-urilor
- **Generare**: ordinea afectează stilul și formatul output-ului
- **Raționament**: ordinea de la simplu la complex îmbunătățește CoT

### Pas 2: Aplică strategia de ordonare potrivită

**Strategie A — Simplu-la-Complex (pentru raționament):**
```
Solve each math problem.

Problem: 2 + 3 = ?
Solution: 5

Problem: (4 × 3) + 7 = ?
Solution: 19

Problem: If x + 5 = 12, what is x?
Solution: x = 7

Problem: "{COMPLEX_NEW_PROBLEM}"
Solution:
```

**Strategie B — Diversificare alternantă (pentru clasificare):**
```
Classify each email.

Email: "Meeting at 3pm tomorrow" → Category: Internal
Email: "50% OFF SALE NOW!" → Category: Spam
Email: "Q3 report attached" → Category: Internal
Email: "You've won a prize!" → Category: Spam

Email: "{NEW_EMAIL}" → Category:
```

**Strategie C — Cel mai similar la final (pentru precizie):**
Plasează exemplul cel mai similar cu input-ul de test ca ultimul exemplu, exploatând recency bias.

### Pas 3: Testează cu permutări
Dacă rezultatele sunt inconsistente, testează 2-3 ordini diferite ale acelorași exemple. Dacă performanța variază >10%, ordinea contează semnificativ pentru task-ul tău.

### Pas 4: Evită pattern-uri predictibile de label-uri
NU pune toate exemplele pozitive primele și toate negative la final. Alternează label-urile pentru a preveni bias-ul.

### Pas 5: Plasează exemplul cel mai relevant ultimul
Recency bias înseamnă că modelul acordă mai multă atenție ultimului exemplu. Pune exemplul cel mai similar cu noul input în poziția finală.

## Verificare

- [ ] Ordinea exemplelor este intenționată, nu aleatorie
- [ ] Label-urile NU sunt grupate (toate pozitive apoi toate negative)
- [ ] Exemplul cel mai relevant e plasat ultimul (recency bias)
- [ ] Testat cu minim 2 ordini diferite pentru a confirma stabilitate
- [ ] Task-uri de raționament folosesc ordine simplu→complex

## Instrumente

| Model | Sensibilitate la ordine |
|-------|------------------------|
| Claude Opus/Sonnet | Moderată — beneficiază de similar-ultimul |
| GPT-4 | Moderată-Mare |
| Modele mici | Mare — ordinea contează foarte mult |

## Note

- **Când să folosești**: Întotdeauna când folosești 3+ exemple; critic când output-ul pare inconsistent.
- **Când NU**: Cu un singur exemplu; când exemplele sunt identice ca dificultate.
- **Greșeli comune**: Ordine aleatorie fără considerare; toate exemplele de aceeași clasă grupate; cel mai dificil exemplu primul.
- **Relație**: Complementar cu PR-002 (quantity) și PR-007 (similarity).
