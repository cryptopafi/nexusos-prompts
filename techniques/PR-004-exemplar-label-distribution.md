---
id: PR-004
title: "Exemplar Label Distribution"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-004: Exemplar Label Distribution

## Obiectiv

Distribuția label-urilor în exemplele Few-Shot influențează bias-ul modelului. Dacă incluzi 5 exemple pozitive și 1 negativ, modelul va fi predispus să clasifice ca pozitiv. Această procedură asigură o distribuție echilibrată sau intenționat dezechilibrată (când reflectă realitatea).

## Pași

### Pas 1: Inventariază clasele/label-urile task-ului
Listează toate categoriile posibile de output. Exemplu: Pozitiv, Negativ, Neutru (3 clase).

### Pas 2: Alege strategia de distribuție

**Strategie A — Distribuție uniformă (default):**
Număr egal de exemple per clasă. Cel mai sigur pentru majoritatea task-urilor.

```
Classify sentiment. Categories: Positive, Negative, Neutral.

"Love this product!" → Positive
"Worst purchase ever." → Negative
"It works as expected." → Neutral
"Amazing customer service!" → Positive
"Completely broken on arrival." → Negative
"Average, nothing special." → Neutral

"{NEW_INPUT}" →
```

**Strategie B — Proporțional cu realitatea:**
Dacă 80% din date sunt pozitive în producție, reflectă asta parțial (dar nu complet — ex: 60/40 în loc de 80/20).

**Strategie C — Over-represent clasa rară:**
Dacă o clasă e critică dar rară (ex: fraud detection), dă-i mai multe exemple decât proporția reală.

### Pas 3: Asigură minim 1 exemplu per clasă
NICIODATĂ nu omite o clasă complet. Modelul nu va produce un label pe care nu l-a văzut în exemple.

### Pas 4: Verifică bias-ul pe un set de test
Rulează 10+ input-uri de test și numără distribuția output-urilor. Dacă modelul produce o clasă disproporționat mai mult, ajustează exemplele.

### Pas 5: Ajustează iterativ
Dacă bias există: adaugă exemple din clasa sub-reprezentată sau elimină din clasa supra-reprezentată.

## Verificare

- [ ] Fiecare clasă/label are minim 1 exemplu
- [ ] Distribuția e uniformă SAU justificat dezechilibrată
- [ ] Testat pe 10+ input-uri — distribuția output nu e skewed
- [ ] Clasele rare/critice au suficiente exemple
- [ ] Nu există clasă complet omisă

## Instrumente

| Model | Note |
|-------|------|
| Toate modelele | Universală — toate LLM-urile sunt sensibile la distribuția exemplelor |

## Note

- **Când să folosești**: La orice task de clasificare cu 2+ clase.
- **Când NU**: Task-uri de generare liberă fără categorii discrete.
- **Greșeli comune**: Distribuție accidental dezechilibrată; omiterea unei clase; presupunerea că modelul "știe" toate clasele fără să le vadă.
- **Relație**: Complementar cu PR-002 (quantity), PR-003 (ordering), PR-005 (quality).
