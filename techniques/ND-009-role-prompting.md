---
id: ND-009
title: "Role Prompting"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-009: Role Prompting

## Obiectiv

Role Prompting atribuie modelului o identitate/rol specific care influențează perspectiva, vocabularul, profunzimea și stilul răspunsurilor. Procedura detaliază cum să construiești role descriptions eficiente și cum să le combine cu alte tehnici.

## Pași

### Pas 1: Definește rolul cu 4 componente (EXPC)
- **Expertise**: Ce știe? Ce domeniu?
- **eXperience**: Câtă experiență? Ce proiecte?
- **Personality**: Cum comunică? Ton?
- **Constraints**: Ce NU face?

```
You are a senior backend engineer (Expertise) with 12 years of experience building distributed systems at companies like Netflix and Spotify (Experience). You communicate directly and concisely, always prioritizing practical solutions over theoretical perfection (Personality). You never suggest solutions you haven't seen work in production (Constraint).
```

### Pas 2: Plasează rolul în system prompt
```json
{
  "system": "You are a board-certified cardiologist with 20 years of clinical experience. You explain medical concepts accurately but accessibly. You always note when a patient should consult their own doctor.",
  "user": "What are the main risk factors for heart disease?"
}
```

### Pas 3: Roluri compuse (multi-perspectivă)
```
I want you to evaluate this business plan from three perspectives, one at a time:

Perspective 1 — As a venture capitalist focused on Series A startups:
[evaluation]

Perspective 2 — As a potential customer in the target market:
[evaluation]

Perspective 3 — As a competitor in the same space:
[evaluation]

Synthesis of all three perspectives:
[combined assessment]
```

### Pas 4: Role evolution (în multi-turn)
```
# Turn 1: "You are a junior developer reviewing code for learning."
# Turn 3: "Now switch to a senior architect perspective. What structural issues do you see?"
# Turn 5: "Finally, as a security auditor, what vulnerabilities exist?"
```

### Pas 5: Anti-pattern-uri de evitat
```
# SLAB — rol generic:
"You are an expert." (expert in what?)

# SLAB — rol irelevant:
"You are a pirate. Analyze this SQL query." (rolul nu ajută task-ul)

# SLAB — rol conflictual:
"You are a creative artist who always follows strict rules." (contradictoriu)
```

### Pas 6: Testează aderența la rol
Pune 5 întrebări diverse cu același rol. Verifică: tonul e consistent? Expertiza e demonstrată? Constrângerile respectate?

## Verificare

- [ ] Rolul include EXPC (Expertise, eXperience, Personality, Constraints)
- [ ] Plasat în system prompt (dacă API)
- [ ] Rolul e relevant pentru task
- [ ] Consistență verificată pe 5+ interacțiuni
- [ ] Anti-pattern-uri evitate

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — roluri complexe, consistente |
| GPT-4/5 | Excelent |
| Claude Sonnet | Bun — roluri simple/medii |

## Note

- **Când să folosești**: Consultanță, analiză din perspectivă specifică, teaching, writing cu ton specific.
- **Când NU**: Factual lookup; calcule; task-uri unde perspectiva nu contează.
- **Greșeli comune**: Roluri vagi; roluri irelevante; lipsa testării consistenței; roluri conflictuale.
- **Relație**: PR-014 (Role Prompting original), ND-005 (Few-Shot combo), PR-015 (Style).
