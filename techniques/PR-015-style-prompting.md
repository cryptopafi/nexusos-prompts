---
id: PR-015
title: "Style Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-015: Style Prompting

## Obiectiv

Style Prompting specifică stilul, tonul sau genul dorit al output-ului. În loc să lași modelul să aleagă un stil default, direcționezi explicit: formal/informal, tehnic/accesibil, concis/detaliat. Controlul stilistic e esențial pentru content creation, comunicare cu audiențe diferite și consistența brand-ului.

## Pași

### Pas 1: Definește dimensiunile stilistice relevante
Alege din aceste axe:
- **Formalitate**: academic / business / conversational / casual
- **Complexitate**: expert / intermediate / beginner
- **Tonalitate**: neutral / enthusiastic / serious / humorous
- **Lungime**: concise (1-2 propoziții) / standard / detailed (multi-paragraf)
- **Perspectivă**: first person / third person / impersonal

### Pas 2: Formulează instrucțiunea de stil explicit

**Template — Stil simplu:**
```
Write in a professional but approachable tone, suitable for a company blog targeting mid-level managers. Use short paragraphs and concrete examples.

Topic: {TOPIC}
```

**Template — Stil academic:**
```
Write in formal academic style with precise terminology. Use passive voice where appropriate. Include hedging language ("suggests", "indicates", "may") when discussing findings that aren't conclusive.

Summarize: {PAPER_ABSTRACT}
```

**Template — Stil conversational:**
```
Explain this concept as if you're talking to a curious friend over coffee. Use analogies from everyday life. Keep it light but accurate. No jargon.

Concept: {TECHNICAL_CONCEPT}
```

### Pas 3: Furnizează un exemplu de stil (opțional dar puternic)
```
Match the style of this example:

Example: "Look, here's the deal with quantum computing — it's not magic, it's just really clever math. Think of it like..."

Now write about {TOPIC} in the same style:
```

### Pas 4: Combină stilul cu constrângeri de format
```
Write a LinkedIn post (max 200 words) about {TOPIC}.
Style: Professional but personal. Use "I" statements. Include one data point. End with a question to drive engagement.
```

### Pas 5: Testează consistența stilistică
Generează 3 output-uri cu aceeași instrucțiune de stil. Verifică dacă tonul, vocabularul și structura sunt consistente.

### Pas 6: Creează un style guide refolosibil
Documentează stilul ca un bloc reutilizabil pe care îl incluzi în multiple prompts.

```
# STYLE GUIDE — Brand Voice
- Tone: Confident, not arrogant
- Vocabulary: Technical terms allowed, but always explained
- Sentence length: Mix short (punchy) with medium
- Avoid: Buzzwords, passive voice, hedging
- Always: Include concrete examples, use active verbs
```

## Verificare

- [ ] Stilul e specificat explicit (nu implicit)
- [ ] Minim 2 dimensiuni stilistice definite (ton, complexitate etc.)
- [ ] Output-ul respectă stilul cerut
- [ ] Consistență verificată pe 3+ output-uri
- [ ] Style guide salvat pentru refolosire (dacă recurent)

## Instrumente

| Model | Note |
|-------|------|
| Claude | Excelent la stiluri nuanțate; respectă bine constraintele de stil |
| GPT-4/5 | Excelent; tinde spre formal by default |
| Modele mici | Stil de bază funcționează, nuanțe pierdute |

## Note

- **Când să folosești**: Content creation, comunicare cu audiențe variate, brand consistency, email drafting, documentation.
- **Când NU**: Task-uri tehnice pure (code, math); când stilul nu contează.
- **Greșeli comune**: Instrucțiuni de stil vagi ("write well"); stil conflictual ("formal but fun"); lipsa unui exemplu de referință.
- **Relație**: PR-014 (Role Prompting), PR-016 (Emotion Prompting).
