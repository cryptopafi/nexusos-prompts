---
id: PR-023
title: "Step-Back Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-023: Step-Back Prompting

## Obiectiv

Step-Back Prompting cere modelului mai întâi o întrebare generică, de nivel înalt (step-back question) despre conceptele sau principiile relevante, apoi folosește acel context pentru a aborda problema specifică. Forțează modelul să "facă un pas înapoi" și să gândească la principii înainte de detalii.

## Pași

### Pas 1: Identifică întrebarea specifică
Problema originală, detaliată, concretă.

### Pas 2: Formulează step-back question
Transformă problema specifică într-o întrebare generală despre principiile/conceptele de bază.

**Exemple de step-back:**
- Specific: "What happens to the pressure of an ideal gas when temperature is doubled at constant volume?"
- Step-back: "What are the fundamental principles governing ideal gas behavior?"

- Specific: "Why is my Python async function returning a coroutine object instead of the actual result?"
- Step-back: "How does async/await work in Python and what are the common patterns for handling coroutines?"

### Pas 3: Aplică template-ul Step-Back

**Template:**
```
I want to solve a specific problem, but first let me understand the general principles.

Step-back question: What are the key principles and concepts related to {GENERAL_TOPIC}?

[Model answers with general principles]

Now, using those principles, answer this specific question:
{SPECIFIC_QUESTION}
```

### Pas 4: Variantă single-prompt
```
Question: {SPECIFIC_QUESTION}

Before answering directly, first step back and consider:
- What general principles or concepts apply here?
- What category of problem is this?
- What approach typically works for this type of problem?

General principles:
[reasoning]

Now apply these principles to answer the specific question:
[answer]
```

### Pas 5: Folosește pentru deep knowledge tasks
```
Specific: "Should I use Redis or Memcached for caching user sessions in a microservices architecture?"

Step back first: What are the key differences between in-memory caching solutions, and what factors determine the best choice for different use cases?

[Principles established]

Now apply to the specific scenario:
[Recommendation with reasoning]
```

### Pas 6: Verifică că step-back-ul adaugă context relevant
Principiile generale trebuie să fie relevante pentru problema specifică. Un step-back prea abstract nu ajută.

## Verificare

- [ ] Step-back question formulată (general, conceptual)
- [ ] Principiile generale sunt relevante pentru problema specifică
- [ ] Răspunsul specific referențiază principiile stabilite
- [ ] Step-back-ul nu e prea abstract/vag
- [ ] Calitatea răspunsului e superioară vs. direct answering

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — produce principii solide |
| GPT-4/5 | Excelent |
| Claude Sonnet | Bun — funcționează bine single-prompt |

## Note

- **Când să folosești**: Probleme tehnice specifice; decizii de design/arhitectură; întrebări scientific; analiza de scenarii.
- **Când NU**: Întrebări factuale simple; task-uri unde principiile sunt evidente; urgențe (overhead de tokens).
- **Greșeli comune**: Step-back prea vag ("What is programming?"); step-back irelevant; saltul direct la răspuns fără a folosi principiile.
- **Relație**: PR-022 (CoT), PR-024 (Analogical), C-062 (Step-Back detailed).
