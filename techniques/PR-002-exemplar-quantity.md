---
id: PR-002
title: "Exemplar Quantity"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-002: Exemplar Quantity

## Obiectiv

Determină numărul optim de exemple (exemplars) într-un Few-Shot prompt. Mai multe exemple îmbunătățesc performanța, dar cu diminishing returns și cost crescut de tokens. Această procedură oferă un framework sistematic pentru a găsi punctul optim.

## Pași

### Pas 1: Stabilește baseline cu zero-shot
Rulează task-ul fără exemple pentru a avea un punct de referință. Notează calitatea output-ului.

### Pas 2: Testează incremental (1, 3, 5, 7 exemple)
Adaugă exemple progresiv și evaluează calitatea la fiecare pas. Notează unde calitatea se stabilizează.

**Template testare:**
```
[Instrucțiune identică pentru toate variantele]

# Varianta 1-shot:
Example: "Input A" → "Output A"
Now process: "{test_input}"

# Varianta 3-shot:
Example 1: "Input A" → "Output A"
Example 2: "Input B" → "Output B"
Example 3: "Input C" → "Output C"
Now process: "{test_input}"

# Varianta 5-shot:
[... 5 exemple ...]
Now process: "{test_input}"
```

### Pas 3: Evaluează cost/beneficiu
Calculează: tokens consumate per variantă vs. îmbunătățirea calității. Dacă 3 exemple dau 95% din calitatea a 7 exemple, folosește 3.

### Pas 4: Ajustează pentru complexitatea task-ului
- Task simplu (clasificare binară): 2-3 exemple suficiente
- Task mediu (extracție structurată): 3-5 exemple
- Task complex (raționament multi-step): 5-7 exemple
- Task cu multe clase: minim 1 exemplu per clasă

### Pas 5: Verifică sensibilitatea modelului
Modelele mari (>70B, GPT-4, Claude Opus) beneficiază mai puțin de exemple suplimentare. Modelele mici necesită mai multe. Ajustează în funcție de model.

### Pas 6: Documentează configurația optimă
Salvează: task, model, număr optimal de exemple, calitate obținută. Refolosește la task-uri similare.

## Verificare

- [ ] Baseline zero-shot documentat
- [ ] Testat cu minim 3 cantități diferite de exemple
- [ ] Calitatea evaluată pe minim 3 input-uri de test per variantă
- [ ] Cost/beneficiu calculat
- [ ] Configurația optimă documentată

## Instrumente

| Model | Recomandare |
|-------|-------------|
| Claude Opus/Sonnet | 2-3 exemple pentru task-uri simple, 3-5 pentru complexe |
| GPT-4/GPT-5 | Similar cu Claude |
| Modele mici (<13B) | 5-7 exemple recomandate |

## Note

- **Când să folosești**: La orice implementare Few-Shot, ca pas de optimizare.
- **Când NU**: Dacă task-ul funcționează bine zero-shot, nu adăuga exemple inutil.
- **Greșeli comune**: Presupunerea că "mai multe = mai bine" fără testare; folosirea aceluiași număr de exemple pentru toate task-urile; ignorarea costului de tokens.
- **Relație**: Prerequisite pentru PR-001; complementar cu PR-003 (ordering), PR-004 (distribution).
