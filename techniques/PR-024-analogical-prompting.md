---
id: PR-024
title: "Analogical Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-024: Analogical Prompting

## Obiectiv

Analogical Prompting cere modelului să genereze automat probleme similare (analogii) cu CoT chains, apoi să le folosească ca exemplare Few-Shot pentru a rezolva problema reală. Combină SG-ICL (PR-011) cu CoT (PR-022) — modelul creează propriile exemple de raționament.

## Pași

### Pas 1: Prezintă problema de rezolvat
Scrie problema fără exemple, dar cu o instrucțiune care cere generarea de analogii.

### Pas 2: Cere modelului să genereze probleme similare rezolvate

**Template:**
```
Problem to solve: {ACTUAL_PROBLEM}

Before solving this, generate 2-3 similar but simpler problems. For each, show the complete solution with step-by-step reasoning. Then use the patterns from those solutions to solve the original problem.

Similar problem 1:
[Model generates a similar problem and solves it with CoT]

Similar problem 2:
[Model generates another similar problem and solves it with CoT]

Now solve the original problem using the same reasoning patterns:
```

### Pas 3: Variantă explicită cu analogie
```
I need to solve this problem:
{PROBLEM}

First, think of an analogous, simpler problem from the same domain. Solve that problem step by step. Then apply the same approach to the original problem.

Analogous problem:
[...]
Solution to analogous problem:
[step by step]

Now applying the same approach to the original:
[step by step]
Final answer:
```

### Pas 4: Aplică la code/design problems
```
I need to implement: {COMPLEX_FEATURE}

Before writing the code, recall a similar but simpler problem you know how to solve. Show the solution for that simpler problem, then adapt it for my specific case.

Similar simpler problem:
[...]
Solution:
[code]

Now adapt for my case:
[code]
```

### Pas 5: Validează calitatea analogiilor
Verifică: (1) Analogia e genulin similară (nu trivial diferită), (2) Raționamentul e transferabil, (3) Soluția problemei originale referențiază analogia.

### Pas 6: Combină cu Step-Back
Step-back identification + analogie: "What type of problem is this? → Generate similar problem → Solve → Transfer"

## Verificare

- [ ] Modelul generează probleme similare (nu identice, nu complet diferite)
- [ ] Fiecare analogie include CoT complet
- [ ] Raționamentul e transferat la problema originală
- [ ] Analogiile sunt din același domeniu/tip
- [ ] Problema originală e rezolvată corect

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — generează analogii relevante |
| GPT-4/5 | Excelent |
| Claude Sonnet | Bun — analogii mai simple dar funcționale |

## Note

- **Când să folosești**: Probleme complexe fără exemple disponibile; math/logic; design problems; debugging; teaching.
- **Când NU**: Când ai deja exemple reale (folosește-le pe acelea); task-uri triviale; când token budget e limitat.
- **Greșeli comune**: Analogii prea simple (nu pregătesc pentru problema reală); analogii din domeniu diferit; modelul sare direct la soluție fără a folosi analogia.
- **Relație**: PR-011 (SG-ICL), PR-022 (CoT), PR-023 (Step-Back).
