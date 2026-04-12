---
id: PR-047
title: "Meta-Reasoning over Multiple CoTs"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-047: Meta-Reasoning over Multiple CoTs

## Obiectiv

Meta-Reasoning (Yoran et al., 2023) generează multiple chain-uri de raționament (nu neapărat cu răspunsuri finale) pentru o problemă, apoi le prezintă modelului pentru a raționa DESPRE raționamente (meta-reasoning) și a produce un răspuns final. Diferența vs. Self-Consistency: analizează PROCESUL de gândire, nu doar răspunsurile.

## Pași

### Pas 1: Generează N chain-uri de raționament diverse
```python
chains = []
for _ in range(N):
    chain = llm(f"{problem}\nLet's think step by step.", temperature=0.7)
    chains.append(chain)
```

### Pas 2: Prezintă toate chain-urile unui meta-reasoner

**Template Meta-Reasoning:**
```
I need to solve this problem: {PROBLEM}

Here are {N} different reasoning attempts:

Reasoning Chain 1:
{chain_1}

Reasoning Chain 2:
{chain_2}

Reasoning Chain 3:
{chain_3}

Now, analyze ALL these reasoning chains:
1. Which chains have correct reasoning? Which have errors?
2. Where do the chains agree? Where do they diverge?
3. What is the strongest argument across all chains?
4. Based on this analysis, what is the correct answer?

Meta-analysis:
```

### Pas 3: Variantă focusată pe erori
```
Review these {N} reasoning attempts for {PROBLEM}:

{chains}

Identify:
- Any logical errors in each chain
- Any missing steps
- Any incorrect assumptions
- Points of agreement (likely correct)

Corrected reasoning:
[produce a new chain that avoids all identified errors]

Final answer:
```

### Pas 4: Implementare
```python
def meta_reasoning(problem, n_chains=5):
    # Generate chains
    chains = [llm(f"{problem}\nThink step by step.", temp=0.7) for _ in range(n_chains)]

    # Meta-reason
    formatted = "\n\n".join(f"Chain {i+1}:\n{c}" for i, c in enumerate(chains))
    meta = llm(f"""
        Problem: {problem}

        Reasoning chains:
        {formatted}

        Analyze all chains. Identify errors and correct reasoning.
        Produce the best possible answer.
    """, temperature=0)
    return meta
```

### Pas 5: Aplică la probleme cu erori de raționament frecvente
Meta-reasoning e superior Self-Consistency când:
- Modelul face erori sistematice (meta-reasoning le identifică)
- Chain-urile conțin pași parțial corecți (meta combină cele mai bune părți)

### Pas 6: Evaluează calitatea meta-analizei
Verifică: a identificat corect erorile? A combinat corect elementele bune? Răspunsul final e mai bun decât any single chain?

## Verificare

- [ ] N >= 3 chain-uri de raționament generate
- [ ] Meta-reasoner primește TOATE chain-urile
- [ ] Analiza identifică erori/divergențe
- [ ] Răspunsul final e bazat pe meta-analiza, nu pe un singur chain
- [ ] Superior vs. majority vote (Self-Consistency)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent la meta-analiza raționamentelor |
| GPT-4/5 | Excelent |
| Mix: Sonnet (chains) + Opus (meta) | Optim cost/calitate |

## Note

- **Când să folosești**: Probleme complexe de reasoning; când Self-Consistency nu dă consens; debugging raționament.
- **Când NU**: Task-uri simple; când chains concordă deja (overhead inutil).
- **Greșeli comune**: Meta-reasoner favorizeazăprimul chain (position bias); chain-uri prea similare; meta-analiza superficială.
- **Relație**: PR-045 (Self-Consistency), PR-046 (Universal SC), PR-043 (MoRE).
