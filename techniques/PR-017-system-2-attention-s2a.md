---
id: PR-017
title: "System 2 Attention (S2A)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-017: System 2 Attention (S2A)

## Obiectiv

System 2 Attention este o tehnică în doi pași: (1) cere LLM-ului să rescrie prompt-ul eliminând informațiile irelevante sau misleading, apoi (2) folosește prompt-ul rescris pentru a genera răspunsul final. Inspirat de System 1/System 2 thinking (Kahneman), forțează modelul să "gândească lent" și să se concentreze pe ce contează.

## Pași

### Pas 1: Identifică prompt-uri cu informații potențial distractoare
S2A e util când prompt-ul conține: detalii irelevante, framing bias, informații contradictorii, context excesiv.

### Pas 2: Step 1 — Cere modelului să rescrie prompt-ul

**Template Step 1 — Filtrare:**
```
Read the following passage and question. Rewrite the passage keeping ONLY the information that is directly relevant to answering the question. Remove any opinions, irrelevant details, or potentially biasing information.

Passage: "John is a well-known and beloved community leader who has contributed greatly to local charities. He recently proposed a new city budget that would increase property taxes by 15% while reducing funding for public schools by 20%. The weather was particularly nice during the city council meeting."

Question: "Is John's proposed budget beneficial for residents?"

Rewritten passage (relevant info only):
```

### Pas 3: Step 2 — Răspunde pe baza prompt-ului filtrat
Folosește output-ul din Step 1 ca input pentru generarea răspunsului final.

```
Based on the following relevant information, answer the question objectively.

Relevant information: "John proposed a city budget that would increase property taxes by 15% while reducing funding for public schools by 20%."

Question: "Is John's proposed budget beneficial for residents?"

Answer:
```

### Pas 4: Implementare single-prompt (shortcut)
Poți combina ambii pași într-un singur prompt:

```
You will be given a passage and a question. First, identify and extract ONLY the facts relevant to the question (ignore opinions, emotional language, and irrelevant details). Then, answer the question based solely on those facts.

Passage: "{PASSAGE}"
Question: "{QUESTION}"

Relevant facts:
[list them]

Answer based on relevant facts only:
```

### Pas 5: Validează filtrarea
Verifică manual: a eliminat modelul informațiile irelevante? A păstrat tot ce e relevant? A eliminat bias-ul?

### Pas 6: Folosește pentru scenarii cu bias frecvent
S2A e deosebit de puternic pentru:
- Evaluări de propuneri (elimină popularity bias)
- Analiză de date cu context narativ (elimină storytelling)
- Întrebări factuale cu context emoțional

## Verificare

- [ ] Prompt-ul original conținea informații irelevante/misleading
- [ ] Step 1 (filtrare) a eliminat corect noise-ul
- [ ] Step 2 (răspuns) se bazează doar pe informații relevante
- [ ] Răspunsul final e mai obiectiv decât fără S2A
- [ ] Nicio informație relevantă nu a fost eliminată accidental

## Instrumente

| Model | Note |
|-------|------|
| Claude Opus | Excelent la filtrare — identifică bine bias-ul |
| GPT-4/5 | Bun la separarea faptelor de opinii |
| Claude Sonnet | Bun pentru single-prompt variant |

## Note

- **Când să folosești**: Prompt-uri cu context excesiv; scenarii cu bias (popularitate, framing); evaluări obiective; fact-checking.
- **Când NU**: Prompt-uri deja concise și factuale; task-uri creative unde "bias-ul" e dorit (storytelling).
- **Greșeli comune**: Filtrare prea agresivă (elimini informații relevante); presupunerea că S2A e mereu necesar; overhead de tokens pe prompt-uri simple.
- **Relație**: PR-019 (RaR — rescrie promptul diferit), PR-054 (RCoT — verificare inversă).
