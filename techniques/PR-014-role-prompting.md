---
id: PR-014
title: "Role Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-014: Role Prompting

## Obiectiv

Role Prompting atribuie modelului un rol specific (expert, profesie, persona) pentru a direcționa stilul, cunoștințele și perspectiva output-ului. Modelul "adoptă" expertiza asociată rolului, producând răspunsuri mai specializate și mai consistente decât un prompt generic.

## Pași

### Pas 1: Identifică expertiza necesară pentru task
Ce tip de specialist ar rezolva cel mai bine acest task? Doctor? Programator senior? Analist financiar? Editor?

### Pas 2: Formulează rolul cu specificitate
Roluri vagi ("be an expert") sunt mai puțin eficiente decât roluri specifice.

**Template — Role Assignment:**
```
You are a senior Python developer with 15 years of experience in building scalable web applications. You specialize in FastAPI, PostgreSQL, and microservices architecture. You write clean, well-documented code and always consider edge cases.

Review this code and suggest improvements:
{CODE}
```

### Pas 3: Adaugă context relevant pentru rol
Include detalii care fac rolul mai concret: experiență, specializare, stil de lucru.

**Template — Role cu context:**
```
You are a medical researcher specializing in clinical trials methodology. You have published 50+ papers in peer-reviewed journals. You are meticulous about statistical rigor and always cite specific evidence levels.

Evaluate this study design:
{STUDY_DESCRIPTION}
```

### Pas 4: Folosește system prompt pentru rol (când e disponibil)
În API calls, plasează rolul în `system` message, nu în `user` message:
```json
{
  "system": "You are a senior financial analyst at a top-tier investment bank. You analyze data objectively and always provide risk assessments.",
  "user": "Analyze this quarterly report: {REPORT}"
}
```

### Pas 5: Combină cu alte tehnici
Role + CoT: "As a [role], think through this step by step..."
Role + Few-Shot: Exemple din perspectiva rolului
Role + Constraints: "As a [role], always include [specific element]..."

### Pas 6: Validează consistența rolului
Testează 5+ prompts cu același rol. Output-ul trebuie să fie consistent ca perspectivă și ton. Dacă nu, rolul e prea vag.

## Verificare

- [ ] Rolul e specific (nu generic "expert")
- [ ] Include specializare și context
- [ ] Plasat în system prompt (dacă disponibil)
- [ ] Consistență verificată pe 5+ input-uri
- [ ] Rolul e relevant pentru task (nu decorativ)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus/Sonnet | Excelent — adoptă roluri complex |
| GPT-4/5 | Excelent |
| Modele mici | Moderat — roluri simple funcționează, complexe nu |

## Note

- **Când să folosești**: Orice task care beneficiază de perspectivă specializată; writing, analysis, code review, advice.
- **Când NU**: Task-uri pur factuale (math, lookup); când rolul introduce bias nedorit.
- **Greșeli comune**: Roluri prea vagi ("be smart"); roluri irelevante pentru task; multiple roluri conflictuale; rolul contrazice instrucțiunile.
- **Relație**: PR-015 (Style Prompting), PR-016 (Emotion Prompting), ND-009 (Role Prompting detailed).
