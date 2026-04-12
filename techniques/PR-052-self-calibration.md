---
id: PR-052
title: "Self-Calibration"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-052: Self-Calibration

## Obiectiv

Self-Calibration (Kadavath et al., 2022) cere modelului mai întâi să răspundă la o întrebare, apoi să evalueze dacă propriul răspuns e corect. Modelul produce un scor de confidență (P(True)) care poate fi folosit pentru a filtra răspunsuri nesigure sau a ruta spre human review.

## Pași

### Pas 1: Obține răspunsul inițial
```
Question: {QUESTION}
Answer:
```

### Pas 2: Cere modelului să-și evalueze propriul răspuns

**Template Self-Calibration:**
```
Question: {QUESTION}

Proposed answer: {MODEL_ANSWER}

Is the above answer correct? Consider:
- Is the reasoning sound?
- Are there any factual errors?
- Am I confident in this answer?

Assessment: [Correct/Uncertain/Likely Incorrect]
Confidence: [0-100%]
```

### Pas 3: Variantă True/False cu P(True)
```
Question: {QUESTION}
Answer: {MODEL_ANSWER}

Is this answer correct? Answer only TRUE or FALSE.
```

Probabilitatea de "TRUE" (P(True)) = scorul de calibrare.

### Pas 4: Implementare cu routing
```python
def calibrated_answer(question):
    # Step 1: Get answer
    answer = llm(f"Question: {question}\nAnswer:")

    # Step 2: Self-calibrate
    calibration = llm(f"""
        Question: {question}
        Answer: {answer}
        Is this answer correct? Respond with a confidence score 0-100.
        Confidence:
    """)
    confidence = parse_score(calibration)

    # Step 3: Route based on confidence
    if confidence >= 80:
        return answer, "high_confidence"
    elif confidence >= 50:
        return answer, "review_recommended"
    else:
        return answer, "low_confidence_escalate"
```

### Pas 5: Aplică la sisteme de producție
```
For each query:
1. Generate answer
2. Self-calibrate (confidence score)
3. If confidence < threshold → flag for human review
4. If confidence >= threshold → auto-respond
```

### Pas 6: Calibrează threshold-ul pe date reale
```python
# Test on labeled data
for item in test_set:
    answer = llm(item["question"])
    confidence = self_calibrate(item["question"], answer)
    is_correct = answer == item["gold_answer"]

    # Plot: confidence vs. actual correctness
    # Find threshold where confidence reliably predicts correctness
```

## Verificare

- [ ] Răspunsul inițial generat
- [ ] Self-calibration aplicată (confidence score)
- [ ] Threshold setat pe baza datelor de validare
- [ ] Routing implementat (high/low confidence)
- [ ] Calibrarea e realistă (nu inflated)

## Instrumente

| Model | Calibrare |
|-------|-----------|
| Claude Opus | Bună — tinde să fie conservativ |
| GPT-4 | Moderată — uneori over-confident |
| Modele mici | Slabă — calibrare nereliabilă |

## Note

- **Când să folosești**: Sisteme QA automatizate; human-in-the-loop; orice sistem unde e important să știi CÂT de sigur e modelul.
- **Când NU**: Task-uri unde totul merge la human review oricum; task-uri creative (nu există "corect").
- **Greșeli comune**: Trust orbesc al scorului de confidență; threshold necalibrat; model over-confident.
- **Relație**: PR-041 (Metacognitive), PR-053 (Self-Refine), PR-055 (Self-Verification), C-065 (Self-Calibration detailed).
