---
id: PR-033
title: "Least-to-Most Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-033: Least-to-Most Prompting

## Obiectiv

Least-to-Most (Zhou et al., 2022) descompune o problemă în sub-probleme ordonate de la cea mai simplă la cea mai complexă, rezolvându-le secvențial. Fiecare sub-problemă rezolvată devine context pentru următoarea. Doi pași: (1) descompunere, (2) rezolvare secvențială. Superior CoT pe probleme cu dependințe între sub-pași.

## Pași

### Pas 1: Step 1 — Descompune problema

**Template descompunere:**
```
Break down this problem into a series of sub-problems that can be solved sequentially, from simplest to most complex. List only the sub-problems, don't solve them yet.

Problem: {COMPLEX_PROBLEM}

Sub-problems (from simplest to most complex):
1.
2.
3.
...
```

### Pas 2: Step 2 — Rezolvă secvențial, construind pe anterioare

**Template rezolvare:**
```
Now solve each sub-problem in order. Use the answer from each sub-problem to help solve the next one.

Sub-problem 1: {SIMPLEST_SUB_PROBLEM}
Answer: [solve]

Sub-problem 2: {NEXT_SUB_PROBLEM}
(Using the answer from sub-problem 1)
Answer: [solve]

Sub-problem 3: {MOST_COMPLEX_SUB_PROBLEM}
(Using answers from sub-problems 1 and 2)
Answer: [solve]

Final answer to the original problem:
```

### Pas 3: Exemplu complet
```
Problem: "How many words are in the sentence 'The quick brown fox jumps over the lazy dog' that contain the letter 'o'?"

Sub-problems:
1. What are all the words in the sentence?
2. Which of those words contain the letter 'o'?
3. How many such words are there?

Solution:
1. Words: The, quick, brown, fox, jumps, over, the, lazy, dog
2. Words with 'o': brown, fox, over, dog
3. Count: 4

Final answer: 4 words contain the letter 'o'.
```

### Pas 4: Variantă single-prompt
```
{PROBLEM}

Solve this by breaking it into simpler sub-problems. Start with the easiest sub-problem and work your way up, using each answer to help with the next.

Sub-problem 1 (simplest):
[solve]

Sub-problem 2 (using answer 1):
[solve]

[...continue until original problem is solved...]

Final answer:
```

### Pas 5: Aplică la task-uri non-math
- **Code**: "First write the helper function, then the main function that uses it"
- **Writing**: "First outline the structure, then fill in each section"
- **Research**: "First define terms, then identify relationships, then draw conclusions"

### Pas 6: Verifică ordinea sub-problemelor
Fiecare sub-problemă trebuie să poată fi rezolvată doar cu informația din sub-problemele anterioare. Dacă sub-problema 3 necesită informație din sub-problema 5, ordinea e greșită.

## Verificare

- [ ] Descompunerea produce sub-probleme ordonate corect
- [ ] Fiecare sub-problemă e rezolvabilă independent (cu context anterior)
- [ ] Răspunsurile anterioare sunt folosite explicit în sub-problemele următoare
- [ ] Ordinea e genuinin de la simplu la complex
- [ ] Răspunsul final integrează toate sub-răspunsurile

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus/Sonnet | Excelent — descompunere naturală |
| GPT-4/5 | Excelent |
| Modele mici | Moderat — descompunerea poate fi slabă |

## Note

- **Când să folosești**: Probleme compoziționale; coding (build up); math cu multiple dependințe; task-uri unde ordinea contează.
- **Când NU**: Probleme care nu se descompun natural; task-uri paralele (fără dependințe).
- **Greșeli comune**: Sub-probleme ordonate greșit; sub-probleme prea granulare; saltul peste Step 1 (descompunere).
- **Relație**: PR-034 (DECOMP), PR-035 (Plan-and-Solve), PR-022 (CoT), ND-010 (Task Decomposition).
