---
id: PR-041
title: "Metacognitive Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-041: Metacognitive Prompting

## Obiectiv

Metacognitive Prompting (Wang & Zhao, 2024) face modelul să miroreze procesele metacognitive umane printr-un chain de 5 pași: (1) clarificarea întrebării, (2) judecata preliminară, (3) evaluarea răspunsului, (4) confirmarea deciziei, (5) evaluarea confidenței. Forțează auto-reflecție sistematică.

## Pași

### Pas 1: Clarificare — Ce se cere exact?
```
Step 1 — Clarify the question:
- What exactly is being asked?
- What are the key concepts involved?
- Are there any ambiguities to resolve?
- What type of answer is expected (factual, analytical, opinion)?
```

### Pas 2: Judecata preliminară — Prima evaluare
```
Step 2 — Preliminary judgment:
- Based on my knowledge, what is my initial assessment?
- What evidence or reasoning supports this?
- What assumptions am I making?
```

### Pas 3: Evaluare critică — Verificarea propriei gândiri
```
Step 3 — Critical evaluation:
- Is my preliminary judgment well-supported?
- Are there alternative perspectives I haven't considered?
- Could my reasoning be flawed? How?
- What information might I be missing?
```

### Pas 4: Confirmarea deciziei
```
Step 4 — Decision confirmation:
- After critical evaluation, do I maintain my original judgment?
- If not, how has it changed and why?
- Is my final answer consistent with all the evidence?
```

### Pas 5: Evaluarea confidenței
```
Step 5 — Confidence assessment:
- How confident am I in this answer? (Low/Medium/High)
- What would increase my confidence?
- What are the main sources of uncertainty?
```

### Pas 6: Template complet integrat

**Template Metacognitive:**
```
{QUESTION}

Answer this using metacognitive reasoning:

1. CLARIFY: What exactly is this question asking? What are the key concepts?
[clarification]

2. INITIAL JUDGMENT: What is my preliminary answer and why?
[first assessment with reasoning]

3. CRITICAL EVALUATION: Challenge my own reasoning. What could be wrong? What alternatives exist?
[self-critique]

4. FINAL DECISION: After reflection, what is my final answer?
[confirmed or revised answer]

5. CONFIDENCE: How confident am I? What uncertainties remain?
[confidence level + uncertainty sources]
```

## Verificare

- [ ] Toți cei 5 pași sunt prezenți
- [ ] Clarificarea identifică corect ce se cere
- [ ] Evaluarea critică e genuină (nu doar "I'm confident")
- [ ] Decizia finală reflectă evaluarea critică
- [ ] Confidența e realistă, nu inflated

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — auto-reflecție naturală |
| GPT-4/5 | Bun |
| Modele mici | Moderat — tind să sară evaluarea critică |

## Note

- **Când să folosești**: Decizii importante; analize unde greșeala e costisitoare; medical/legal reasoning; evaluări complexe.
- **Când NU**: Task-uri simple/factuale; când viteza e prioritară; high-volume automated tasks.
- **Greșeli comune**: Auto-evaluarea superficială ("I'm confident"); lipsa evaluării critice reale; prea mulți tokens pe task-uri simple.
- **Relație**: PR-022 (CoT), PR-052 (Self-Calibration), PR-053 (Self-Refine).
