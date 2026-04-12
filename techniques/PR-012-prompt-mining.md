---
id: PR-012
title: "Prompt Mining"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-012: Prompt Mining

## Obiectiv

Prompt Mining descoperă cuvintele și frazele optime din interiorul unui prompt prin analiza unui corpus mare. În loc să ghicești formularea potrivită, analizezi cum apar relațiile input-output în text natural și folosești acele pattern-uri ca "middle words" în prompt. Util mai ales pentru knowledge extraction și fill-in-the-blank tasks.

## Pași

### Pas 1: Definește relația pe care vrei să o capturezi
Exemplu: "X este capitala lui Y" — vrei un prompt care extrage capitalele țărilor.

### Pas 2: Identifică mai multe formulări naturale ale relației
Caută în corpus-uri (Wikipedia, documente) cum e exprimată relația:
- "[Paris] is the capital of [France]"
- "[Paris], capital of [France]"
- "The capital of [France] is [Paris]"
- "[France]'s capital, [Paris]"

### Pas 3: Transformă formulările în template-uri de prompt
```
# Template A (direct):
"What is the capital of {COUNTRY}?"

# Template B (fill-in-the-blank, mined):
"{COUNTRY}'s capital is ___"

# Template C (mined din corpus):
"The capital city of {COUNTRY}, known as ___"

# Template D (reverse):
"___ is the capital of {COUNTRY}"
```

### Pas 4: Testează fiecare template pe un set de validare
Rulează 20+ input-uri cu fiecare template. Măsoară acuratețea.

### Pas 5: Selectează template-ul cu cea mai bună performanță
Template-urile "mined" din text natural performează adesea mai bine decât formulările ad-hoc, pentru că reflectă distribuția din training data.

### Pas 6: Generalizează la alte relații
Aplică aceeași metodă pentru alte relații: "X a fost fondată în Y", "X aparține categoriei Y" etc.

**Template generalizat:**
```
# Relație: [entitate] → [proprietate]
# Formulare 1: "[Entitate]'s [proprietate] is ___"
# Formulare 2: "The [proprietate] of [entitate] is ___"
# Formulare 3: "[Entitate], whose [proprietate] is ___"
# Testează și alege cea mai bună.
```

## Verificare

- [ ] Minim 3 formulări alternative testate
- [ ] Fiecare formulare testată pe 10+ input-uri
- [ ] Template-ul optim selectat pe baza acurateței
- [ ] Formulările provin din text natural (nu inventate)
- [ ] Template-ul documentat pentru refolosire

## Instrumente

| Instrument | Rol |
|-----------|-----|
| Wikipedia / Large corpus | Sursă de formulări naturale |
| LLM (Claude/GPT) | Testare template-uri |
| Script Python | Automatizare testare pe set de validare |

## Note

- **Când să folosești**: Knowledge extraction; fill-in-the-blank; relational queries; când prompt-ul standard nu dă rezultate bune.
- **Când NU**: Task-uri creative; generare de text lung; task-uri fără relație clară input-output.
- **Greșeli comune**: Formulare prea creativă (nu reflectă training data); testarea unui singur template; ignorarea variantelor din corpus.
- **Relație**: PR-008 (instruction selection), PR-051 (prompt paraphrasing).
