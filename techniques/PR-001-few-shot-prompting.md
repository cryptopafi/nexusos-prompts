---
id: PR-001
title: "Few-Shot Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-001: Few-Shot Prompting

## Obiectiv

Few-Shot Prompting furnizează modelului câteva exemple (exemplars) de input-output înainte de task-ul real. Modelul învață pattern-ul din exemple și îl aplică la noul input — fără fine-tuning. Este fundamentul In-Context Learning (ICL) și funcționează pentru clasificare, extracție, traducere, reformulare și orice task cu un pattern repetabil.

## Pași

### Pas 1: Definește task-ul și formatul dorit
Scrie explicit ce transformare vrei: input → output. Stabilește formatul (Q/A, Input/Output, text liber etc.).

### Pas 2: Selectează 3-5 exemple reprezentative
Alege exemple care acoperă cazurile tipice. Diversifică categoriile/label-urile. Evită exemple ambigue.

### Pas 3: Formatează exemplele uniform
Folosește un separator clar și consistent între exemple.

**Template:**
```
Classify the sentiment of each review.

Review: "The battery lasts all day and the screen is gorgeous."
Sentiment: Positive

Review: "Broke after two weeks. Terrible quality control."
Sentiment: Negative

Review: "It's okay for the price, nothing special."
Sentiment: Neutral

Review: "{NEW_INPUT}"
Sentiment:
```

### Pas 4: Plasează instrucțiunea ÎNAINTE de exemple
Instrucțiunea generală (ce task, ce format) vine prima. Exemplele urmează. Input-ul nou vine ultimul.

### Pas 5: Testează cu 3-5 input-uri noi
Verifică dacă modelul respectă pattern-ul din exemple. Dacă nu, ajustează exemplele (vezi PR-002 până la PR-007 pentru optimizare).

### Pas 6: Optimizează numărul de exemple
Mai multe exemple = mai bine (de obicei), dar cu diminishing returns. Testează cu 1, 3, 5 exemple și compară calitatea.

## Verificare

- [ ] Minim 2 exemple incluse în prompt
- [ ] Exemplele acoperă categoriile/cazurile principale
- [ ] Formatul este identic între toate exemplele
- [ ] Instrucțiunea precede exemplele
- [ ] Output-ul modelului respectă formatul din exemple
- [ ] Testat pe minim 3 input-uri noi

## Instrumente

| Model | Eficacitate | Note |
|-------|-------------|------|
| Claude (Opus/Sonnet) | Excelent | Funcționează bine și cu 2 exemple |
| GPT-4/GPT-5 | Excelent | Sensibil la format |
| Modele mici (<7B) | Moderat | Necesită mai multe exemple (5+) |

## Note

- **Când să folosești**: Orice task cu un pattern clar input→output, clasificare, extracție, formatare.
- **Când NU**: Task-uri creative deschise; task-uri unde Zero-Shot e suficient.
- **Greșeli comune**: Exemple inconsistente ca format; exemple cu erori; prea puține categorii reprezentate; ordinea exemplelor nefavorabilă (vezi PR-003).
- **Relație**: Complementar cu Chain-of-Thought (PR-022) — combinate în Few-Shot CoT.
