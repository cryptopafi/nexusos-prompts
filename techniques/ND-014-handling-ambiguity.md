---
id: ND-014
title: "Handling Ambiguity and Improving Clarity"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-014: Handling Ambiguity and Improving Clarity

## Obiectiv

Identificarea și rezolvarea ambiguităților din prompt-uri care duc la răspunsuri inconsistente sau irelevante. Tehnici pentru scrierea de prompt-uri clare care produc output predictibil.

## Pași

### Pas 1: Identifică tipurile de ambiguitate
```
# Ambiguitate lexicală (cuvânt cu multiple sensuri):
"Analyze this bank." — Bancă financiară sau malul râului?

# Ambiguitate structurală (structură gramaticală):
"I saw the man with the telescope." — Cine are telescopul?

# Ambiguitate de scop:
"Improve this code." — Performanță? Readability? Security? Toate?

# Ambiguitate de format:
"Summarize this." — Câte cuvinte? Ce format? Ce aspect?
```

### Pas 2: Aplică testul de interpretare multiplă
Citește prompt-ul și întreabă: "Poate fi interpretat în 2+ moduri?" Dacă da, clarifică.

```
# Ambiguu: "List important Python features"
# Întrebare: Important for whom? For beginners? For performance? For data science?
# Clar: "List 5 Python features most valuable for a backend developer building REST APIs"
```

### Pas 3: Tehnici de dezambiguare

**Specificare explicită:**
```
# Ambiguu: "Write a short article about AI"
# Clar: "Write a 500-word article about the impact of AI on healthcare diagnostics in 2025. Target audience: hospital administrators with no technical background."
```

**Definirea termenilor:**
```
# Ambiguu: "Identify edge cases"
# Clar: "Identify edge cases (unusual inputs that might cause errors or unexpected behavior), including: empty inputs, maximum-size inputs, special characters, and concurrent access scenarios."
```

**Exemplu de output dorit:**
```
# Ambiguu: "Format the data nicely"
# Clar: "Format the data as a markdown table like this example:
| Name | Age | Score |
|------|-----|-------|
| Alice | 30 | 95 |"
```

### Pas 4: Checklist de claritate (pre-flight)
Înainte de a trimite un prompt, verifică:
- [ ] Cine e audiența?
- [ ] Ce format e cerut exact?
- [ ] Care sunt limitele (lungime, scop)?
- [ ] Ce NU trebuie inclus?
- [ ] E clar CE succes arată?

### Pas 5: Cere modelului să identifice ambiguitatea
```
Before answering, identify any ambiguities in this request. List them and state the interpretation you'll use.

Request: "{AMBIGUOUS_PROMPT}"

Ambiguities found:
1. [ambiguity] → I'll interpret as: [interpretation]
2. [ambiguity] → I'll interpret as: [interpretation]

Answer (based on stated interpretations):
```

### Pas 6: Iterează pe baza output-ului
Dacă output-ul nu e ce ai așteptat, analizează CE a interpretat modelul diferit și ajustează prompt-ul specific.

## Verificare

- [ ] Prompt-ul testat cu testul de interpretare multiplă
- [ ] Termenii ambigui definiți explicit
- [ ] Scopul specificat (ce include, ce exclude)
- [ ] Format de output specificat (exemplu inclus dacă complex)
- [ ] Output consistent pe 5+ runs

## Instrumente

| Model | Note |
|-------|------|
| Toate modelele | Claritatea ajută universal |

## Note

- **Când să folosești**: Orice prompt, mai ales cele complexe; când output-ul e inconsistent; comunicare cross-team.
- **Când NU**: N/A — claritatea e mereu benefică.
- **Greșeli comune**: Presupunerea că modelul "știe ce vreau"; termeni nedefiniți; scop nemenționat.
- **Relație**: ND-012 (Instruction Engineering), ND-015 (Length Management), PR-019 (RaR).
