---
id: PR-026
title: "Tabular Chain-of-Thought (Tab-CoT)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-026: Tabular Chain-of-Thought (Tab-CoT)

## Obiectiv

Tab-CoT cere modelului să structureze raționamentul ca un tabel Markdown. Structura tabelară forțează modelul să organizeze informația în coloane (step, reasoning, result), prevenind salturi logice și îmbunătățind claritatea. Zero-Shot CoT variant — nu necesită exemple.

## Pași

### Pas 1: Formulează problema cu instrucțiunea Tab-CoT

**Template de bază:**
```
{PROBLEM}

Solve this step by step. Present your reasoning as a markdown table with columns: Step | Action | Result. Then provide the final answer.
```

### Pas 2: Exemplu complet de output dorit
```
Problem: A shop buys 50 shirts at $12 each, sells 35 at $25 each and 10 at a 40% discount from selling price. What is the total profit?

| Step | Action | Result |
|------|--------|--------|
| 1 | Calculate cost: 50 × $12 | $600 |
| 2 | Revenue from 35 full price: 35 × $25 | $875 |
| 3 | Discounted price: $25 × 0.60 | $15 |
| 4 | Revenue from 10 discounted: 10 × $15 | $150 |
| 5 | Total revenue: $875 + $150 | $1,025 |
| 6 | Unsold shirts: 50 - 35 - 10 = 5 | 5 shirts |
| 7 | Profit: $1,025 - $600 | $425 |

**Final answer: $425 profit**
```

### Pas 3: Customizează coloanele per task type

**Analiză/comparație:**
```
| Factor | Option A | Option B | Winner |
```

**Debugging:**
```
| Line | Code | Expected | Actual | Issue |
```

**Decision-making:**
```
| Criterion | Weight | Score A | Score B | Weighted A | Weighted B |
```

### Pas 4: Aplică la analiză multi-factor
```
Evaluate these three cloud providers for our use case.

Present your analysis as a table:
| Criterion | AWS | GCP | Azure | Notes |
|-----------|-----|-----|-------|-------|

Then provide your recommendation based on the table.
```

### Pas 5: Combină cu alte tehnici
Tab-CoT + Role: "As a financial analyst, analyze this using a structured table..."
Tab-CoT + RE2: Repetă problema + instrucțiune tabelară

### Pas 6: Validează completitudinea tabelului
Verifică: fiecare pas e listat? nicio tranziție lipsă? numerele din Result sunt corecte? concluzia finală e consistentă cu tabelul?

## Verificare

- [ ] Output-ul e formatat ca tabel Markdown valid
- [ ] Fiecare pas de raționament e o linie separată
- [ ] Coloanele sunt relevante pentru task
- [ ] Nu există salturi logice (fiecare pas decurge din anteriorul)
- [ ] Răspunsul final e consistent cu ultima linie din tabel

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude | Excelent — produce tabele Markdown curate |
| GPT-4/5 | Excelent |
| Modele mici | Moderat — tabelele pot fi malformate |

## Note

- **Când să folosești**: Math, comparații, decision matrices, debugging, orice reasoning structurat; când vrei output ușor de auditat.
- **Când NU**: Generare creativă; răspunsuri scurte; când formatul tabelar e forțat.
- **Greșeli comune**: Coloane prea multe (greu de urmărit); tabel fără concluzie finală; forțarea formatului pe task-uri unde nu se potrivește.
- **Relație**: PR-022 (CoT base), PR-025 (ThoT), ND-017 (prompt formatting).
