---
id: PR-025
title: "Thread-of-Thought (ThoT) Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-025: Thread-of-Thought (ThoT) Prompting

## Obiectiv

Thread-of-Thought folosește un thought inducer îmbunătățit care direcționează modelul să proceseze informația segment cu segment, "walking through" contextul secvențial. Spre deosebire de CoT clasic care zice generic "think step by step", ThoT specifică CUM să gândească: parcurge informația, analizează fiecare segment, apoi integrează. Optimizat pentru task-uri cu context lung.

## Pași

### Pas 1: Identifică task-uri cu context dens
ThoT e superior CoT pe: documente lungi, conversații cu multiple turns, scenarii cu multe detalii, date tabelare.

### Pas 2: Folosește thought inducer-ul ThoT

**Template ThoT:**
```
{LONG_CONTEXT_OR_DOCUMENT}

Question: {QUESTION}

Walk me through this context in manageable parts, then build your answer:
- First, identify the relevant sections
- Then, analyze each relevant section
- Finally, synthesize the information to answer the question
```

### Pas 3: Variantă pentru conversații/dialoguri
```
{CONVERSATION_TRANSCRIPT}

Question: What was the final agreement reached?

Let me walk through this conversation systematically:
- First, I'll identify each participant's positions
- Then, I'll track how positions evolved through the dialogue
- Finally, I'll identify the consensus or final state
```

### Pas 4: Variantă pentru date/tabele
```
{DATA_TABLE_OR_REPORT}

Question: {ANALYTICAL_QUESTION}

Let me process this data systematically:
- Segment 1: [first portion of data]
- Observations from Segment 1: [...]
- Segment 2: [next portion]
- Observations from Segment 2: [...]
- Integration of all observations:
- Final answer:
```

### Pas 5: Customizează thought inducer-ul per domeniu
- Legal: "Walk through each clause and its implications..."
- Medical: "Examine each symptom and test result sequentially..."
- Code: "Trace through the code execution path..."
- Financial: "Analyze each line item and its impact..."

### Pas 6: Compară cu CoT standard
Rulează același task cu "Let's think step by step" vs. ThoT inducer. Pe context lung, ThoT ar trebui să producă răspunsuri mai complete și mai precise.

## Verificare

- [ ] Thought inducer-ul e specific (nu generic "step by step")
- [ ] Modelul parcurge contextul segment cu segment
- [ ] Nicio secțiune relevantă a contextului nu e omisă
- [ ] Răspunsul final integrează toate observațiile
- [ ] Superior vs. CoT standard pe task-ul specific

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — gestionează context lung nativ |
| GPT-4/5 | Bun — beneficiază de ThoT pe context lung |
| Modele cu context mic | Esențial — ajută la procesarea eficientă |

## Note

- **Când să folosești**: Document analysis; conversație parsing; raport analysis; orice task cu >1000 tokens de context.
- **Când NU**: Întrebări scurte, directe; task-uri unde contextul e minim.
- **Greșeli comune**: Thought inducer generic în loc de specific; omiterea etapei de integrare; segmente prea mari sau prea mici.
- **Relație**: PR-022 (CoT base), PR-026 (Tab-CoT), PR-035 (Plan-and-Solve).
