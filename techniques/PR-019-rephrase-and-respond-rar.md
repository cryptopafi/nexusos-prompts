---
id: PR-019
title: "Rephrase and Respond (RaR)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-019: Rephrase and Respond (RaR)

## Obiectiv

RaR instruiește modelul să reformuleze și să expandeze întrebarea înainte de a genera răspunsul final. Reformularea forțează modelul să proceseze mai profund cerința, să identifice ambiguități și să clarifice ce se cere de fapt. Simplu de implementat, eficient pe task-uri complexe.

## Pași

### Pas 1: Adaugă instrucțiunea de reformulare la prompt

**Template simplu:**
```
{ORIGINAL_QUESTION}

Before answering, rephrase this question in your own words to make sure you understand it correctly. Then provide your answer.
```

### Pas 2: Variantă cu reformulare expandată
```
{ORIGINAL_QUESTION}

First, rephrase and expand this question to clarify what is being asked. Break it down into sub-questions if needed. Then answer the expanded version thoroughly.
```

### Pas 3: Variantă one-line (minimalistă)
Adaugă o singură instrucțiune la finalul oricărui prompt:

```
Explain what the population of the largest city in the state where the Declaration of Independence was signed is.

Rephrase the question, then answer it.
```

Modelul va produce:
```
Rephrased: "What is the population of Philadelphia, which is the largest city in Pennsylvania, the state where the Declaration of Independence was signed?"

Answer: Philadelphia's population is approximately 1.6 million.
```

### Pas 4: Variantă cu validare explicită
```
{COMPLEX_QUESTION}

Step 1: Restate this question in simpler, clearer terms.
Step 2: Identify any assumptions or ambiguities in the original question.
Step 3: Answer the clarified version.
```

### Pas 5: Aplică la instrucțiuni vagi
RaR e deosebit de puternic pe prompt-uri vagi:
```
Original: "Make the code better"

Rephrase and expand: What specific improvements are being requested? Consider:
- Performance optimization?
- Code readability?
- Error handling?
- Test coverage?

Then implement the most likely requested improvements.
```

### Pas 6: Evaluează dacă reformularea e corectă
Citește reformularea modelului. Dacă a înțeles greșit, prompt-ul original era ambiguu — reformularea te ajută să diagnosticezi problema.

## Verificare

- [ ] Instrucțiunea de reformulare e inclusă în prompt
- [ ] Modelul produce reformularea vizibil (nu o sare)
- [ ] Reformularea e corectă (nu schimbă sensul)
- [ ] Răspunsul final e mai bun decât fără RaR
- [ ] Pentru prompt-uri simple: verificat dacă RaR adaugă valoare (poate nu)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Toate modelele | Universală — funcționează pe orice LLM |
| Claude | Reformulări naturale și precise |
| GPT-4 | Reformulări detaliate, uneori excesiv |

## Note

- **Când să folosești**: Întrebări complexe cu multiple componente; prompt-uri ambigue; reasoning tasks; când vrei să verifici dacă modelul a înțeles cerința.
- **Când NU**: Task-uri simple/directe; când viteza e prioritară (RaR adaugă tokens de output).
- **Greșeli comune**: Folosirea pe prompt-uri deja clare (overhead inutil); necitierea reformulării (poți rata o neînțelegere); reformularea schimbă sensul.
- **Relație**: PR-020 (RE2 — re-reading), PR-017 (S2A — rescrie prompt-ul), C-067 (RaR detailed).
