---
id: PR-011
title: "Self-Generated In-Context Learning (SG-ICL)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-011: Self-Generated In-Context Learning (SG-ICL)

## Obiectiv

SG-ICL folosește modelul însuși pentru a genera exemplele de Few-Shot. În loc să furnizezi exemple manual, ceri LLM-ului să creeze exemple relevante, apoi le folosești ca exemplare în prompt-ul final. Util când nu ai exemple pregătite sau când domeniul e nou.

## Pași

### Pas 1: Descrie task-ul clar pentru modelul generator
Formulează o instrucțiune care descrie exact ce tip de exemple ai nevoie.

**Prompt Step 1 — Generare exemple:**
```
Generate 5 diverse examples of customer support tickets with their correct category labels. Categories: Billing, Technical, Shipping, Returns, Account.

Each example should be realistic and clearly belong to its category. Format:
Ticket: "[customer message]"
Category: [label]

Generate now:
```

### Pas 2: Revizuiește exemplele generate
Verifică manual: sunt corecte? sunt diverse? acoperă toate categoriile? Elimină exemplele slabe sau duplicate.

### Pas 3: Folosește exemplele generate în prompt-ul final
```
Classify each customer ticket into a category.

Ticket: "I was charged twice for the same order"
Category: Billing

Ticket: "The app crashes when I try to upload photos"
Category: Technical

Ticket: "My package says delivered but I never received it"
Category: Shipping

Ticket: "{ACTUAL_INPUT}"
Category:
```

### Pas 4: Iterează — generează mai multe și selectează cele mai bune
Generează 10-15 exemple, apoi selectează cele mai bune 3-5 pe baza diversității și clarității.

### Pas 5: Adaugă constrângeri la generare pentru calitate
```
Generate 5 examples of [task]. Requirements:
- Each example must be unambiguous (clearly one category)
- Cover all [N] categories at least once
- Vary in length and complexity
- Include one edge case
```

### Pas 6: Validează pe input-uri reale
Testează prompt-ul cu exemple generate pe input-uri reale. Dacă performanța e sub așteptări, înlocuiește exemplele generate cu exemple reale pentru cazurile problematice.

## Verificare

- [ ] Exemple generate de model, nu inventate de om (economisește timp)
- [ ] Fiecare exemplu generat verificat manual ca corect
- [ ] Toate categoriile/clasele acoperite
- [ ] Cel puțin 1 edge case inclus
- [ ] Performanța pe input-uri reale comparabilă cu exemple manuale

## Instrumente

| Model | Rol |
|-------|-----|
| Claude Opus/GPT-4 | Generare exemple de înaltă calitate |
| Claude Sonnet/GPT-3.5 | Generare exemple simple (cost redus) |
| Orice LLM | Model target care folosește exemplele |

## Note

- **Când să folosești**: Nu ai exemple reale; domeniu nou; prototipare rapidă; automatizare pipeline Few-Shot.
- **Când NU**: Ai deja exemple reale de calitate — cele reale sunt aproape mereu mai bune.
- **Greșeli comune**: Folosirea exemplelor generate fără revizuire; exemple prea similare între ele; bias-ul modelului se perpetuează în exemple.
- **Relație**: PR-001 (Few-Shot), PR-024 (Analogical Prompting — similar concept).
