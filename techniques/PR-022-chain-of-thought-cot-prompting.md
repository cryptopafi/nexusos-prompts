---
id: PR-022
title: "Chain-of-Thought (CoT) Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-022: Chain-of-Thought (CoT) Prompting

## Obiectiv

Chain-of-Thought încurajează modelul să-și exprime procesul de gândire pas cu pas înainte de a da răspunsul final. Poate fi Few-Shot (cu exemple de reasoning) sau Zero-Shot ("Let's think step by step"). Este cea mai influentă tehnică de prompting pentru task-uri de reasoning, math și logică.

## Pași

### Pas 1: Identifică task-uri care beneficiază de CoT
CoT ajută la: matematică, logică, planificare, analiză multi-factor, debugging, decision-making. NU ajută semnificativ la: recall factual simplu, clasificare binară, generare creativă.

### Pas 2: Zero-Shot CoT — Cea mai simplă variantă
Adaugă "Let's think step by step" la finalul oricărui prompt.

**Template Zero-Shot CoT:**
```
A farmer has 3 fields. The first field produces 120 kg of wheat, the second produces 85 kg, and the third produces 95 kg. If he sells wheat at $2.50 per kg but keeps 30% for personal use, how much money does he make?

Let's think step by step.
```

### Pas 3: Few-Shot CoT — Cu exemple de reasoning
Furnizează 2-3 exemple care arată procesul de gândire.

**Template Few-Shot CoT:**
```
Q: If a store has 40 items and sells 15, then receives a shipment of 25, how many items are in stock?
A: Let's work through this step by step.
- Start: 40 items
- After selling 15: 40 - 15 = 25 items
- After shipment of 25: 25 + 25 = 50 items
The answer is 50 items.

Q: A company has 3 teams of 8 people. If 5 people leave and 3 new people are hired, how many people work there?
A: Let's work through this step by step.
- Start: 3 teams x 8 people = 24 people
- After 5 leave: 24 - 5 = 19 people
- After 3 hired: 19 + 3 = 22 people
The answer is 22 people.

Q: {NEW_PROBLEM}
A: Let's work through this step by step.
```

### Pas 4: Structurează chain-ul de gândire
Un bun CoT are: (1) Restare problemei, (2) Identificare date relevante, (3) Pași de raționament secvențiali, (4) Răspuns final clar marcat.

### Pas 5: Variante de thought inducer
- "Let's think step by step" (clasic, Wei et al.)
- "Let's work through this systematically"
- "Let's break this down"
- "Let's solve this step by step, showing our work"
- "Think carefully and show your reasoning"

### Pas 6: Verifică calitatea chain-ului
Citește raționamentul: Sunt pașii corecți? Lipsesc pași? Există salturi logice? Dacă da, îmbunătățește exemplele sau adaugă constrângeri.

## Verificare

- [ ] Thought inducer inclus ("step by step" sau equivalent)
- [ ] Modelul produce raționament vizibil, nu doar răspuns
- [ ] Fiecare pas de raționament e corect
- [ ] Nu există salturi logice
- [ ] Răspunsul final e clar marcat ("The answer is...")
- [ ] Task-ul beneficiază de CoT (nu e trivial)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — CoT nativ puternic |
| GPT-4/5 | Excelent |
| Claude Sonnet | Foarte bun — optim raport calitate/cost |
| Modele mici (<13B) | Necesită Few-Shot CoT (Zero-Shot insuficient) |

## Note

- **Când să folosești**: Orice task de reasoning, math, logică, planificare, analiză multi-step.
- **Când NU**: Recall factual simplu; clasificare trivială; când viteza e prioritară (CoT adaugă tokens).
- **Greșeli comune**: Folosirea pe task-uri triviale (overhead inutil); ne-verificarea chain-ului (poate conține erori); un singur "step by step" fără a citi output-ul.
- **Relație**: Fundament pentru PR-023 la PR-032; complementar cu PR-001 (Few-Shot).
