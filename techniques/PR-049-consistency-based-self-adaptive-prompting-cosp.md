---
id: PR-049
title: "Consistency-based Self-adaptive Prompting (COSP)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-049: Consistency-based Self-adaptive Prompting (COSP)

## Obiectiv

COSP (Wan et al., 2023) construiește automat Few-Shot CoT prompts prin: (1) Rulează Zero-Shot CoT cu Self-Consistency pe un set de exemple, (2) Selectează exemplele cu cel mai mare agreement (high consistency), (3) Folosește acele output-uri ca exemplare Few-Shot în prompt-ul final. Bootstrapping: modelul își generează propriile exemple de calitate.

## Pași

### Pas 1: Pregătește un set de input-uri de bootstrapping
Colectează 20-50 input-uri reprezentative (fără label-uri necesare).

### Pas 2: Rulează Zero-Shot CoT cu Self-Consistency pe fiecare
```python
bootstrapping_results = {}
for input_text in bootstrapping_inputs:
    responses = [
        llm(f"{input_text}\nLet's think step by step.", temperature=0.7)
        for _ in range(N)
    ]
    answers = [extract_answer(r) for r in responses]
    agreement = max(Counter(answers).values()) / N
    majority_answer = Counter(answers).most_common(1)[0][0]
    majority_chain = [r for r in responses if extract_answer(r) == majority_answer][0]

    bootstrapping_results[input_text] = {
        "agreement": agreement,
        "chain": majority_chain,
        "answer": majority_answer
    }
```

### Pas 3: Selectează exemplele cu cel mai mare agreement
```python
# Sort by agreement (descending)
sorted_results = sorted(
    bootstrapping_results.items(),
    key=lambda x: x[1]["agreement"],
    reverse=True
)
# Select top-K with highest agreement as exemplars
K = 3
selected_exemplars = sorted_results[:K]
```

### Pas 4: Construiește prompt-ul Few-Shot CoT final
```python
prompt = ""
for input_text, result in selected_exemplars:
    prompt += f"Q: {input_text}\n"
    prompt += f"A: {result['chain']}\n\n"

# Use for new inputs
prompt += f"Q: {{new_input}}\nA: Let's think step by step."
```

### Pas 5: Testează pe input-uri noi
Verifică dacă exemplele auto-generate produc performanță comparabilă cu exemplare manuale.

### Pas 6: Iterează — rulează COSP periodic
Pe măsură ce modelul se actualizează sau task-ul evoluează, re-bootstrap-ează pentru exemplare fresh.

## Verificare

- [ ] Set de bootstrapping >= 20 input-uri
- [ ] Self-Consistency cu N >= 5 per input
- [ ] Exemplare selectate pe baza agreement-ului
- [ ] Prompt-ul final conține exemplare cu high agreement
- [ ] Performanța comparabilă cu Few-Shot CoT manual

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API | Bootstrapping + producție |
| Python | Pipeline COSP |

## Note

- **Când să folosești**: Nu ai exemplare manuale; bootstrapping automat; sisteme self-improving.
- **Când NU**: Ai deja exemplare manuale de calitate; task-uri unde modelul e nesigur pe totul (low agreement universal).
- **Greșeli comune**: Agreement threshold prea scăzut; bootstrapping set nereprezentativ; chain-uri auto-generate cu erori (neverificate).
- **Relație**: PR-045 (Self-Consistency), PR-050 (USP), PR-011 (SG-ICL), PR-032 (Auto-CoT).
