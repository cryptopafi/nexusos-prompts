---
id: PR-021
title: "Self-Ask"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-021: Self-Ask

## Obiectiv

Self-Ask cere modelului să decidă dacă are nevoie de sub-întrebări (follow-up questions) înainte de a răspunde la întrebarea principală. Modelul se întreabă singur "Do I need to ask a follow-up question?" și, dacă da, generează și răspunde la sub-întrebări secvențial până ajunge la răspunsul final. Excelent pentru compositional questions.

## Pași

### Pas 1: Identifică întrebări compoziționale
Self-Ask funcționează pe întrebări care necesită rezolvarea unor sub-probleme intermediare.
Exemplu: "What is the population of the country where the Eiffel Tower is located?" necesită: (1) Unde e Eiffel Tower? (2) Ce populație are acea țară?

### Pas 2: Formulează prompt-ul cu Self-Ask pattern

**Template:**
```
Question: Who was the president of the US when the first iPhone was released?

Are follow-up questions needed here: Yes.

Follow-up: When was the first iPhone released?
Intermediate answer: The first iPhone was released on June 29, 2007.

Follow-up: Who was the president of the US in 2007?
Intermediate answer: George W. Bush was the president in 2007.

So the final answer is: George W. Bush.
```

### Pas 3: Furnizează 1-2 exemple Few-Shot cu Self-Ask pattern
```
Question: What is the capital of the country that won the most gold medals at the 2020 Olympics?

Are follow-up questions needed here: Yes.
Follow-up: Which country won the most gold medals at the 2020 Olympics?
Intermediate answer: The United States won the most gold medals (39).
Follow-up: What is the capital of the United States?
Intermediate answer: Washington, D.C.
So the final answer is: Washington, D.C.

---

Question: What is 15 + 27?

Are follow-up questions needed here: No.
So the final answer is: 42.

---

Question: {NEW_QUESTION}

Are follow-up questions needed here:
```

### Pas 4: Integrează cu search (opțional)
În sisteme agentic, sub-întrebările pot fi rutate la un search engine:
```python
def self_ask_with_search(question):
    response = llm(f"Are follow-up questions needed? {question}")
    while "Follow-up:" in response:
        sub_question = extract_followup(response)
        search_result = search_engine(sub_question)
        response = llm(f"Intermediate answer: {search_result}\n{response}")
    return extract_final_answer(response)
```

### Pas 5: Validează lanțul de sub-întrebări
Verifică: (1) Sub-întrebările sunt necesare? (2) Răspunsurile intermediare sunt corecte? (3) Răspunsul final decurge logic din intermediare?

## Verificare

- [ ] Pattern-ul "Are follow-up questions needed?" e inclus
- [ ] Sub-întrebările sunt relevante și necesare
- [ ] Fiecare sub-întrebare primește un răspuns intermediar
- [ ] Răspunsul final decurge din răspunsurile intermediare
- [ ] Întrebările simple NU generează sub-întrebări inutile

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — generează sub-întrebări relevante |
| GPT-4/5 | Excelent |
| Claude Sonnet | Bun, dar uneori sare sub-întrebări necesare |
| + Search API | Ideal pentru fapte verificabile |

## Note

- **Când să folosești**: Întrebări compoziționale multi-hop; knowledge questions cu dependențe; QA cu verificare.
- **Când NU**: Întrebări simple directe; task-uri creative; când nu există sub-probleme naturale.
- **Greșeli comune**: Sub-întrebări inutile pe întrebări simple; răspunsuri intermediare greșite propagate; prea multe niveluri de sub-întrebări.
- **Relație**: PR-022 (CoT), PR-033 (Least-to-Most), PR-034 (Decomposed Prompting).
