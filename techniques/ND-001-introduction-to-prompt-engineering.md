---
id: ND-001
title: "Introduction to Prompt Engineering"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-001: Introduction to Prompt Engineering

## Obiectiv

Conceptele fundamentale ale prompt engineering-ului: ce este un prompt, de ce contează formularea, și cum interacționează prompt-ul cu modelele AI. Stabilește vocabularul și cadrul mental pentru toate tehnicile ulterioare.

## Pași

### Pas 1: Înțelege componentele unui prompt
Un prompt poate conține: instrucțiune, context, input, format de output. Nu toate sunt obligatorii.

```
[Instrucțiune] Summarize the following article in 3 bullet points.
[Context] The article is from a medical journal aimed at general practitioners.
[Input] {ARTICLE_TEXT}
[Output format] Use bullet points, max 20 words each.
```

### Pas 2: Înțelege cum procesează modelul un prompt
Modelul generează text token cu token, bazându-se pe distribuția probabilistică din training data. Prompt-ul tău "direcționează" această distribuție. Un prompt bun = distribuție strânsă pe output-ul dorit.

### Pas 3: Principiul CLAR (Clar, Limitat, Acționabil, Relevant)
```
# SLAB:
"Tell me about Python"

# BUN (CLAR):
"Explain the difference between Python lists and tuples.
Include: when to use each, performance implications, and mutability.
Format: comparison table followed by 2-sentence summary."
```

### Pas 4: Testează iterativ
Prompt engineering e empiric. Scrie → testează → observă → ajustează → repetă.

**Workflow:**
1. Scrie prima versiune a prompt-ului
2. Testează pe 3 input-uri diferite
3. Identifică ce nu funcționează
4. Ajustează prompt-ul
5. Re-testează

### Pas 5: Documentează prompt-urile eficiente
Salvează prompt-urile care funcționează bine cu: task, model, versiune, rezultate observate.

```
# Prompt Registry Entry
Task: Sentiment classification
Model: Claude Sonnet
Prompt v1.2: "Classify as positive/negative/neutral: {text}"
Accuracy: ~92% pe test set (50 items)
Notes: Funcționează mai bine cu instrucțiunea înainte de text
```

### Pas 6: Cunoaște limitările
- Modelele nu au memorie persistentă (fiecare call e independent)
- Context window e finit (verifică limita modelului)
- Modelele pot halucina (verifică output-urile factuale)
- Output-ul e probabilistic, nu determinist

## Verificare

- [ ] Poți identifica cele 4 componente ale unui prompt
- [ ] Poți rescrie un prompt vag într-unul CLAR
- [ ] Ai testat minim 3 variante ale unui prompt
- [ ] Ai documentat cel puțin 1 prompt eficient
- [ ] Înțelegi limitările modelelor AI

## Instrumente

| Model | Rol |
|-------|-----|
| Claude / GPT / orice LLM | Practică directă |
| Playground APIs | Testare rapidă |

## Note

- **Când să folosești**: Ca bază înainte de orice altă tehnică; training pentru persoane noi.
- **Când NU**: Dacă ești deja experimentat — treci direct la tehnicile specifice (PR/ND-004+).
- **Greșeli comune**: Prompt-uri vagi; lipsa testării iterative; presupunerea că un prompt funcționează universal.
- **Relație**: Fundament pentru toate celelalte proceduri. Start here.
