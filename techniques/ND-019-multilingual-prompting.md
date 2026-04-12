---
id: ND-019
title: "Multilingual and Cross-lingual Prompting"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-019: Multilingual and Cross-lingual Prompting

## Obiectiv

Tehnici de prompting care funcționează across limbile naturale: prompting în limba modelului (EN) cu input în altă limbă, prompting multilingval, translation chains, și adaptarea culturală a prompt-urilor.

## Pași

### Pas 1: Prompting în EN cu input non-EN
Instrucțiunile în engleză funcționează cel mai bine pe cele mai multe modele, chiar dacă input-ul e într-o altă limbă.

```
Analyze the sentiment of this Romanian text. Respond in English.

Text: "Produsul este excelent, funcționează perfect și livrarea a fost rapidă."

Sentiment:
Analysis:
```

### Pas 2: Prompting în limba target-ului
Dacă vrei output într-o limbă specifică, scrie prompt-ul în acea limbă SAU specifică limba de output.

```
# Option A — Prompt in target language:
Analizează textul de mai jos și identifică sentimentul (pozitiv/negativ/neutru).
Text: "{TEXT}"
Sentiment:

# Option B — EN prompt, specify output language:
Analyze the sentiment of this text. Respond in Romanian.
Text: "{TEXT}"
```

### Pas 3: Cross-lingual transfer
Folosește exemplare într-o limbă (EN) pentru a procesa input în altă limbă:

```
Classify the sentiment of each text.

"This product is amazing!" → Positive
"Terrible experience, never again." → Negative
"It's okay, nothing special." → Neutral

"Ce produs minunat, sunt foarte mulțumit!" →
```

### Pas 4: Translation chain
Dacă modelul e slab pe o limbă, folosește un chain:
```python
def cross_lingual_analyze(text, source_lang):
    # Step 1: Translate to English
    en_text = llm(f"Translate from {source_lang} to English:\n{text}")

    # Step 2: Analyze in English (strongest)
    analysis = llm(f"Analyze this text for key themes and sentiment:\n{en_text}")

    # Step 3: Translate analysis back
    result = llm(f"Translate this analysis to {source_lang}:\n{analysis}")

    return result
```

### Pas 5: Cultural adaptation
```
Write a marketing message for {PRODUCT} targeting {COUNTRY} market.

Consider:
- Local cultural norms and communication style
- Common expressions and idioms in {LANGUAGE}
- Avoid phrases that might be culturally inappropriate
- Adapt humor/formality level to {CULTURE} preferences

Output in {LANGUAGE}:
```

### Pas 6: Evaluare multi-lingvistică
Testează pe native speakers sau cu back-translation:
```python
def back_translation_check(original, target_lang):
    translated = llm(f"Translate to {target_lang}: {original}")
    back = llm(f"Translate to English: {translated}")
    # Compare original with back-translation for meaning preservation
    return original, back, similarity_score(original, back)
```

## Verificare

- [ ] Instrucțiunile sunt în limba optimă (EN pentru instrucțiuni tehnice)
- [ ] Limba de output specificată explicit
- [ ] Cross-lingual exemplare funcționează
- [ ] Cultural adaptation considerată (dacă relevant)
- [ ] Testat cu native speakers (dacă posibil)

## Instrumente

| Model | Capabilitate multilingvală |
|-------|---------------------------|
| Claude Opus/Sonnet | Excelent — 50+ limbi |
| GPT-4/5 | Excelent |
| Modele mici | Variabil — testează specific pe limba ta |

## Note

- **Când să folosești**: Input non-EN; audiență multilingvală; localizare; traduceri.
- **Când NU**: Totul e în EN; limba nu variază.
- **Greșeli comune**: Presupunerea că modelul e la fel de bun pe toate limbile; lipsa testării pe native speakers; ignorarea nuanțelor culturale.
- **Relație**: ND-018 (Task-Specific), ND-015 (Length — limbi diferite au lungimi diferite).
