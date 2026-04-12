---
id: PR-020
title: "Re-reading (RE2)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-020: Re-reading (RE2)

## Obiectiv

RE2 adaugă "Read the question again:" la prompt și repetă întrebarea, forțând modelul să proceseze input-ul de două ori. Simpla repetare a întrebării îmbunătățește comprehension-ul, mai ales pe task-uri de reasoning și math. E cea mai simplă tehnică de îmbunătățire — zero overhead cognitiv, funcționează universal.

## Pași

### Pas 1: Formulează prompt-ul normal
Scrie prompt-ul ca de obicei, cu toată informația necesară.

### Pas 2: Adaugă "Read the question again:" + repetă întrebarea

**Template:**
```
A store has 45 apples. They sell 12 in the morning and receive a shipment of 30 in the afternoon. Then they sell 18 more before closing. How many apples does the store have at closing?

Read the question again: A store has 45 apples. They sell 12 in the morning and receive a shipment of 30 in the afternoon. Then they sell 18 more before closing. How many apples does the store have at closing?

Answer:
```

### Pas 3: Variantă cu focus pe elementele cheie
```
{QUESTION}

Read the question again carefully, paying special attention to:
- The specific numbers mentioned
- The sequence of events
- What exactly is being asked

{QUESTION_REPEATED}

Answer:
```

### Pas 4: Aplică la reading comprehension
```
Passage: "{LONG_PASSAGE}"

Question: "{QUESTION}"

Read the question again: "{QUESTION}"

Based on the passage, answer:
```

### Pas 5: Combină cu alte tehnici
RE2 + CoT:
```
{QUESTION}

Read the question again: {QUESTION}

Let's think step by step:
```

RE2 + RaR:
```
{QUESTION}

Read the question again: {QUESTION}

First rephrase the question, then answer it:
```

### Pas 6: Evaluează impactul
Compară acuratețea cu și fără RE2 pe 10+ întrebări. Improvement-ul e tipic 2-5% pe reasoning/math tasks.

## Verificare

- [ ] Întrebarea e repetată literal (nu parafrazată)
- [ ] "Read the question again:" e inclus ca prefix
- [ ] Repetarea e plasată înainte de "Answer:" / instrucțiunea finală
- [ ] Testat pe task-uri de reasoning (unde RE2 ajută cel mai mult)
- [ ] Comparație cu baseline fără RE2

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Toate modelele | Universală — zero-cost improvement |
| Modele mici | Impact mai mare pe modele mici |
| Modele mari | Impact modest dar consistent |

## Note

- **Când să folosești**: Math, logic, reading comprehension, orice task unde modelul "scapă" detalii; cost zero de implementare.
- **Când NU**: Task-uri creative; prompt-uri foarte scurte (o singură propoziție); când token budget e foarte restrictiv.
- **Greșeli comune**: Parafrazarea în loc de repetare literală; uitarea de a include "Read the question again:" (doar repetarea fără instrucțiune e mai puțin eficientă); folosirea pe task-uri unde nu adaugă valoare.
- **Relație**: PR-019 (RaR), PR-016 (Emotion — alt mod de a crește atenția), PR-022 (CoT).
