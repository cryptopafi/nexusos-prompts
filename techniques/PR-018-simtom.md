---
id: PR-018
title: "SimToM (Simulated Theory of Mind)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-018: SimToM (Simulated Theory of Mind)

## Obiectiv

SimToM abordează întrebări complexe care implică multiple persoane sau obiecte, fiecare cu perspective/cunoștințe diferite. Tehnica cere modelului să simuleze perspectiva fiecărei persoane separat înainte de a răspunde. Esențial pentru scenarii unde "cine știe ce" contează.

## Pași

### Pas 1: Identifică scenarii cu perspective multiple
SimToM e necesar când:
- Diferite persoane au informații diferite
- Întrebarea depinde de ce știe o persoană specifică
- Există asimetrie informațională

### Pas 2: Cere modelului să listeze ce știe fiecare persoană

**Template Step 1 — Perspective mapping:**
```
Read the following scenario carefully.

Scenario: "Alice put a ball in a red box and left the room. While Alice was away, Bob moved the ball from the red box to a blue box. Carol watched Bob move the ball. Then Alice returned."

Before answering any questions, list what each person knows:
- Alice knows: [list]
- Bob knows: [list]
- Carol knows: [list]
```

### Pas 3: Pune întrebarea din perspectiva unei persoane specifice

**Template Step 2 — Question from perspective:**
```
Based on what Alice knows (she put the ball in the red box and was away when it was moved), answer:

Where does Alice think the ball is?
```

### Pas 4: Combinație single-prompt

**Template complet:**
```
Scenario: "{SCENARIO_WITH_MULTIPLE_ACTORS}"

Step 1: List what each person in the scenario knows and doesn't know.
Step 2: Answer the following question from {PERSON}'s perspective ONLY, using ONLY the information that {PERSON} has access to.

Question: {QUESTION}
```

### Pas 5: Aplică la scenarii business
```
Scenario: "The CEO announced a restructuring plan to the board on Monday. The HR director was informed on Tuesday. The employees haven't been told yet. It's Wednesday."

What each party knows:
- CEO: [restructuring plan details + board reaction]
- Board: [restructuring plan details]
- HR Director: [restructuring plan details, informed Tuesday]
- Employees: [nothing about restructuring]

Question: If an employee asks HR about rumors of restructuring, what can HR legitimately discuss?
Answer (from HR's perspective, considering what HR knows AND what employees know):
```

### Pas 6: Verifică consistența perspectivelor
Asigură-te că modelul nu "scapă" informații dintr-o perspectivă în alta (information leakage).

## Verificare

- [ ] Fiecare actor/persoană are perspectiva listată separat
- [ ] Răspunsul folosește DOAR informația accesibilă persoanei întrebate
- [ ] Nu există information leakage între perspective
- [ ] Scenariul are genuină asimetrie informațională
- [ ] Răspunsul e verificat logic din perspectiva corectă

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — gestionează bine perspective multiple |
| GPT-4/5 | Bun, mai ales cu step-by-step explicit |
| Modele mici | Slab — tind să amestece perspectivele |

## Note

- **Când să folosești**: False-belief tasks; scenarii cu informație asimetrică; negotiation analysis; privacy-aware reasoning; game theory.
- **Când NU**: Întrebări cu o singură perspectivă; task-uri factuale simple.
- **Greșeli comune**: Nu listezi perspectivele separat (modelul amestecă); întrebarea nu specifică din a cui perspectivă; scenarii fără asimetrie reală.
- **Relație**: PR-017 (S2A — filtrare informații), PR-041 (Metacognitive Prompting).
