---
id: PR-008
title: "Instruction Selection for Few-Shot"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-008: Instruction Selection for Few-Shot

## Obiectiv

Deși instrucțiunile sunt esențiale în Zero-Shot, beneficiul lor în Few-Shot (unde exemplele demonstrează deja task-ul) e dezbătut. Uneori instrucțiunile ajută, alteori adaugă noise. Această procedură ajută la deciderea dacă și ce instrucțiuni să incluzi cu exemplele Few-Shot.

## Pași

### Pas 1: Testează Few-Shot fără instrucțiuni
Rulează prompt-ul doar cu exemple, fără instrucțiune explicită. Dacă modelul înțelege task-ul doar din exemple, instrucțiunea poate fi opțională.

**Test A — fără instrucțiune:**
```
"The sunset was breathtaking." → Positive
"I hated every minute." → Negative
"It was fine." → Neutral

"The service exceeded expectations." →
```

### Pas 2: Adaugă instrucțiune minimală
Adaugă o singură propoziție care descrie task-ul. Compară calitatea.

**Test B — cu instrucțiune:**
```
Classify the sentiment of each text as Positive, Negative, or Neutral.

"The sunset was breathtaking." → Positive
"I hated every minute." → Negative
"It was fine." → Neutral

"The service exceeded expectations." →
```

### Pas 3: Testează instrucțiune detaliată
Adaugă criterii specifice, edge cases, definiții.

**Test C — instrucțiune detaliată:**
```
Classify sentiment. Rules:
- Positive: clear expression of satisfaction, joy, or approval
- Negative: dissatisfaction, complaint, or criticism
- Neutral: factual statements without emotional charge, mixed feelings

[... exemple ...]
```

### Pas 4: Compară cele 3 variante pe 5+ input-uri
Evaluează: acuratețe, consistență, aderare la format. Alege varianta cu cel mai bun raport calitate/tokens.

### Pas 5: Reguli de decizie
- **Instrucțiune necesară**: task ambiguu, edge cases frecvente, output format strict
- **Instrucțiune opțională**: task evident din exemple, format simplu
- **Instrucțiune contra-productivă**: când exemplele sunt suficient de clare și instrucțiunea introduce ambiguitate

### Pas 6: Dacă incluzi instrucțiune, plasează-o ÎNAINTE de exemple
Ordinea optimă: Instrucțiune → Exemple → Input nou.

## Verificare

- [ ] Testat cu și fără instrucțiune
- [ ] Dacă instrucțiunea e inclusă, e plasată înainte de exemple
- [ ] Instrucțiunea nu contrazice exemplele
- [ ] Instrucțiunea nu introduce ambiguitate pe care exemplele n-o au
- [ ] Varianta optimă documentată

## Instrumente

| Model | Preferință |
|-------|-----------|
| Claude | Beneficiază de instrucțiuni concise + exemple |
| GPT-4/5 | Similar — instrucțiunile ajută la edge cases |
| Modele mici | Instrucțiunile ajută mai mult (exemplele singure nu sunt suficiente) |

## Note

- **Când să folosești**: La orice Few-Shot prompt, ca pas de optimizare.
- **Când NU**: N/A — decizia de a include sau nu instrucțiune e mereu relevantă.
- **Greșeli comune**: Instrucțiuni lungi care consumă tokens inutil; instrucțiuni care contrazic exemplele; presupunerea că instrucțiunea e mereu necesară.
- **Relație**: PR-001 (Few-Shot), PR-012 (prompt mining), ND-012 (instruction engineering).
