---
id: ND-002
title: "Basic Prompt Structures"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-002: Basic Prompt Structures

## Obiectiv

Înțelegerea celor două structuri fundamentale de prompt: single-turn (o singură interacțiune) și multi-turn (conversație). Alegerea structurii potrivite afectează calitatea output-ului, gestionarea contextului și eficiența.

## Pași

### Pas 1: Single-turn — Un prompt, un răspuns
Tot contextul, instrucțiunile și input-ul sunt într-un singur mesaj.

```
# Single-turn prompt:
You are a professional copywriter. Write a product description for a wireless Bluetooth speaker. Include: key features, target audience, and a compelling call-to-action. Keep it under 150 words.

Product name: SoundWave Pro
Price: $79.99
Key specs: 20-hour battery, waterproof (IPX7), 360° sound
```

**Când să folosești**: Task-uri auto-conținute; API calls batch; task-uri simple.

### Pas 2: Multi-turn — Conversație cu context acumulat
Interacțiuni secvențiale unde fiecare mesaj construiește pe anterioarele.

```
# Turn 1:
User: I need to plan a marketing campaign for a new SaaS product.
Assistant: I'd be happy to help. What's the product, target audience, and budget?

# Turn 2:
User: It's a project management tool for small teams. Budget: $5,000/month. Target: startups with 5-20 employees.
Assistant: Here's a preliminary campaign structure...

# Turn 3:
User: Focus more on content marketing and reduce paid ads.
Assistant: Updated plan with content-first approach...
```

**Când să folosești**: Explorare iterativă; refinement; task-uri complexe care necesită clarificări.

### Pas 3: System + User + Assistant (structura API)
```json
{
  "messages": [
    {"role": "system", "content": "You are a data analyst specializing in e-commerce metrics."},
    {"role": "user", "content": "Analyze this sales data: {DATA}"},
    {"role": "assistant", "content": "Based on the data..."}
  ]
}
```

**System**: Setează rolul și regulile permanente
**User**: Input-ul și cerințele
**Assistant**: Răspunsurile (pot fi pre-filled pentru Few-Shot)

### Pas 4: Alege structura potrivită
| Criteriu | Single-turn | Multi-turn |
|----------|-------------|------------|
| Task simplu, definit | ✓ | Overkill |
| Nevoie de clarificări | ✗ | ✓ |
| API automation | ✓ | Complex |
| Explorare creativă | Limitată | ✓ |
| Reproducibilitate | Ușoară | Dificilă |

### Pas 5: Convertește multi-turn în single-turn (când e posibil)
Dacă ai un workflow multi-turn repetitiv, consolidează-l într-un single-turn prompt care include tot contextul necesar.

```
# Multi-turn (3 exchanges) → Single-turn:
You are a data analyst for e-commerce. Analyze this sales data focusing on:
1. Month-over-month growth trends
2. Top performing categories
3. Recommendations for Q4

Constraints: Focus on actionable insights. Include specific numbers.
Data: {FULL_DATA}
```

### Pas 6: Gestionează contextul în multi-turn
Context window e finit. Strategii:
- Sumarizează turnurile anterioare periodic
- Include doar informația relevantă din history
- Folosește system prompt pentru reguli permanente

## Verificare

- [ ] Structura aleasă (single/multi-turn) e potrivită pentru task
- [ ] În single-turn: tot contextul necesar e inclus
- [ ] În multi-turn: contextul e gestionat (nu overflow)
- [ ] System/User/Assistant roles folosite corect (în API)
- [ ] Poți converti între structuri când e necesar

## Instrumente

| Model | Note |
|-------|------|
| Claude API | System + User + Assistant messages |
| GPT API | Aceeași structură |
| Chat interfaces | Multi-turn nativ |

## Note

- **Când să folosești**: La orice interacțiune cu LLM — alegerea structurii e primul pas.
- **Când NU**: N/A — structura e mereu relevantă.
- **Greșeli comune**: Multi-turn pentru task-uri simple; single-turn fără suficient context; context overflow în multi-turn.
- **Relație**: ND-001 (fundamentals), ND-003 (templates), ND-011 (chaining).
