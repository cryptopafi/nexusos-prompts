---
id: PR-040
title: "Skeleton-of-Thought"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-040: Skeleton-of-Thought

## Obiectiv

Skeleton-of-Thought (Ning et al., 2023) accelerează generarea răspunsurilor prin paralelizare. Pasul 1: modelul generează un schelet (outline) al răspunsului. Pasul 2: fiecare punct din schelet e expandat în paralel (API calls simultane). Reduce latența semnificativ pe răspunsuri lungi.

## Pași

### Pas 1: Generează scheletul

**Template Step 1 — Skeleton:**
```
I want a detailed answer to: {QUESTION}

First, provide ONLY an outline with numbered main points (no details yet). Each point should be one line.

Skeleton:
1.
2.
3.
...
```

### Pas 2: Expandează fiecare punct în paralel

**Template Step 2 — Expansion (per punct):**
```
Context: I'm writing a detailed answer about "{QUESTION}".

Here is the outline:
{FULL_SKELETON}

Now write ONLY the detailed content for point {N}: "{POINT_N_TITLE}"

Keep it focused, 2-3 paragraphs.
```

### Pas 3: Implementare programatic
```python
import asyncio

async def skeleton_of_thought(question):
    # Step 1: Generate skeleton
    skeleton = await llm_async(f"""
        Provide an outline for answering: {question}
        Just numbered points, one line each.
    """)
    points = parse_outline(skeleton)

    # Step 2: Expand all points in parallel
    tasks = [
        llm_async(f"""
            Outline: {skeleton}
            Write detailed content for point {i+1}: "{point}"
        """)
        for i, point in enumerate(points)
    ]
    expansions = await asyncio.gather(*tasks)

    # Step 3: Assemble
    full_answer = ""
    for point, expansion in zip(points, expansions):
        full_answer += f"\n## {point}\n{expansion}\n"

    return full_answer
```

### Pas 4: Variantă single-prompt (fără paralelizare)
```
{QUESTION}

First, create a skeleton outline of your answer. Then expand each point.

Skeleton:
1. [point]
2. [point]
3. [point]

Expanded answer:

### 1. [point]
[detailed content]

### 2. [point]
[detailed content]

### 3. [point]
[detailed content]
```

### Pas 5: Aplică la content creation
Ideal pentru: articole de blog, rapoarte, documentație, tutoriale — orice cu structură secțională.

```
Write a comprehensive guide about {TOPIC}.

Skeleton (main sections):
1.
2.
3.
4.
5.

[Then expand each section in parallel]
```

### Pas 6: Asigură coerență între secțiuni
Punct slab: secțiunile expandate în paralel pot fi redundante. Adaugă un pas final de editare:
```
Review the assembled answer for: redundancies, gaps, flow between sections. Make minimal edits for coherence.
```

## Verificare

- [ ] Scheletul e generat ÎNAINTE de expansion
- [ ] Fiecare punct din schelet e expandat
- [ ] Dacă paralelizat: toate expansion-urile completate
- [ ] Coerența între secțiuni verificată
- [ ] Latența redusă vs. generare secvențială (dacă paralel)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude API (async) | Ideal pentru paralelizare |
| GPT API (async) | Ideal pentru paralelizare |
| Orice LLM (single) | Funcționează ca outline→expand pattern |

## Note

- **Când să folosești**: Răspunsuri lungi; articole; documentație; orice cu structură secțională; când latența contează.
- **Când NU**: Răspunsuri scurte; task-uri care necesită raționament secvențial (fiecare secțiune depinde de anterioara).
- **Greșeli comune**: Secțiuni redundante (lipsa editării finale); schelet prea detaliat sau prea vag; dependințe între secțiuni ignorate.
- **Relație**: PR-022 (CoT), PR-035 (Plan-and-Solve), C-069 (Skeleton detailed).
