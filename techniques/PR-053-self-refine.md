---
id: PR-053
title: "Self-Refine"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-053: Self-Refine

## Obiectiv

Self-Refine (Madaan et al., 2023) este un framework iterativ: (1) modelul generează un răspuns inițial, (2) același model dă feedback pe răspuns, (3) modelul îmbunătățește răspunsul pe baza feedback-ului. Ciclul se repetă până la convergență. Self-improvement fără intervenție umană.

## Pași

### Pas 1: Generează răspunsul inițial
```
{TASK}

Initial response:
```

### Pas 2: Cere feedback pe răspuns

**Template Feedback:**
```
I wrote the following response to this task:
Task: {TASK}
Response: {INITIAL_RESPONSE}

Provide specific, actionable feedback on this response. What is good? What could be improved? List specific issues:

Feedback:
1.
2.
3.
```

### Pas 3: Îmbunătățește pe baza feedback-ului

**Template Refinement:**
```
Task: {TASK}
Previous response: {CURRENT_RESPONSE}

Feedback received:
{FEEDBACK}

Write an improved version that addresses all the feedback points:

Improved response:
```

### Pas 4: Iterează (2-3 cicluri)
```python
def self_refine(task, max_iterations=3):
    response = llm(f"Task: {task}\nResponse:")

    for i in range(max_iterations):
        # Get feedback
        feedback = llm(f"""
            Task: {task}
            Response: {response}
            Provide specific feedback on what to improve:
        """)

        # Check if converged
        if "no improvements needed" in feedback.lower():
            break

        # Refine
        response = llm(f"""
            Task: {task}
            Previous: {response}
            Feedback: {feedback}
            Improved response:
        """)

    return response
```

### Pas 5: Variantă single-prompt (shortcut)
```
{TASK}

Write a response, then critique it, then write an improved version.

Draft 1:
[initial response]

Critique of Draft 1:
[specific issues]

Draft 2 (improved):
[refined response addressing all issues]
```

### Pas 6: Aplică la diverse task-uri
- **Code**: Write → Review → Refactor
- **Writing**: Draft → Edit → Polish
- **Analysis**: Initial assessment → Challenge assumptions → Refined analysis
- **Email**: Draft → Check tone → Finalize

## Verificare

- [ ] Răspuns inițial generat
- [ ] Feedback specific și acționabil
- [ ] Răspunsul refinat adresează feedback-ul
- [ ] Minim 1 iterație de refinement
- [ ] Convergență (nu se rotește în cerc)
- [ ] Versiunea finală e superior inițialei

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — feedback și refinement de calitate |
| GPT-4/5 | Excelent |
| Claude Sonnet | Bun — 2 iterații suficiente |

## Note

- **Când să folosești**: Code review, writing, analiză, orice output care beneficiază de iterare; când nu ai human reviewer.
- **Când NU**: Task-uri simple/factuale; când prima versiune e suficient de bună; risk de over-refinement (output devine over-engineered).
- **Greșeli comune**: Feedback vag ("make it better"); prea multe iterații (diminishing returns); neconvergență (modelul oscilează).
- **Relație**: PR-052 (Self-Calibration), PR-056 (CoVe), PR-041 (Metacognitive).
