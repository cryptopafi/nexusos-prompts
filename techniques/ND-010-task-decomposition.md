---
id: ND-010
title: "Task Decomposition in Prompts"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-010: Task Decomposition in Prompts

## Obiectiv

Descompunerea task-urilor complexe în sub-task-uri gestionabile. Un singur prompt complex produce adesea output mediocru. Descompunerea permite focus pe fiecare parte, cu rezultate superioare combinate.

## Pași

### Pas 1: Identifică dacă task-ul e decomposable
Indicatori: task-ul are multiple faze; output-ul are secțiuni distincte; o parte depinde de alta; e prea complex pentru un singur prompt.

### Pas 2: Decomposition Top-Down
Începe cu task-ul complet, descompune în sub-task-uri.

```
# Task complet: "Write a comprehensive market analysis for electric vehicles in Europe"
# Descompunere:
Sub-task 1: Define market scope and segments
Sub-task 2: Analyze current market size and growth rate
Sub-task 3: Identify key players and market share
Sub-task 4: Assess regulatory environment
Sub-task 5: Project future trends (3-5 years)
Sub-task 6: Synthesize findings and recommendations
```

### Pas 3: Definește dependințele
```
Sub-task 1 → independent (start here)
Sub-task 2 → depends on 1 (scope defines what to measure)
Sub-task 3 → depends on 1 (scope defines players)
Sub-task 4 → independent (can run parallel with 2, 3)
Sub-task 5 → depends on 2, 3, 4 (needs all data)
Sub-task 6 → depends on all (synthesis)
```

### Pas 4: Execută sub-task-urile

**Sequential (simplu):**
```python
results = {}
for task in ordered_subtasks:
    context = {k: results[k] for k in task.dependencies if k in results}
    results[task.id] = llm(f"""
        Context from previous steps: {context}
        Current task: {task.description}
        Execute:
    """)
```

**Single-prompt decomposition:**
```
Analyze the electric vehicle market in Europe.

Complete each section in order, using information from previous sections:

## 1. Market Scope
[define scope]

## 2. Market Size (using scope from §1)
[analyze size]

## 3. Key Players (within scope from §1)
[identify players]

## 4. Regulatory Environment
[assess regulations]

## 5. Future Trends (based on §2-4)
[project trends]

## 6. Recommendations (synthesizing §1-5)
[recommendations]
```

### Pas 5: Validează completitudinea
Verifică: fiecare sub-task a fost abordat? Informația curge corect între sub-task-uri? Nicio dependință omisă?

### Pas 6: Reunește output-urile
Dacă execuția e pe prompt-uri separate, combină și editează pentru coerență.

## Verificare

- [ ] Task-ul descompus în 3-7 sub-task-uri
- [ ] Dependințele identificate
- [ ] Fiecare sub-task executat
- [ ] Informația curge corect între sub-task-uri
- [ ] Output-ul final e coerent și complet

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent la descompunere + execuție |
| GPT-4/5 | Excelent |
| LangChain SequentialChain | Automatizare pipeline |

## Note

- **Când să folosești**: Task-uri complexe cu multiple aspecte; analize comprehensive; proiecte mari.
- **Când NU**: Task-uri simple; când descompunerea adaugă overhead fără beneficiu.
- **Greșeli comune**: Sub-task-uri prea granulare; dependințe omise; pierderea contextului între sub-task-uri.
- **Relație**: PR-033 (Least-to-Most), PR-034 (DECOMP), ND-011 (Chaining), PR-035 (Plan-and-Solve).
