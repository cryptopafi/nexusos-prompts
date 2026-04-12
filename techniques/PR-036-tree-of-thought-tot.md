---
id: PR-036
title: "Tree-of-Thought (ToT)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-036: Tree-of-Thought (ToT)

## Obiectiv

Tree-of-Thought (Yao et al., 2023) transformă raționamentul într-o structură arborescentă. La fiecare pas, generează multiple opțiuni (branches), evaluează fiecare, și continuă pe cele mai promițătoare. Permite backtracking — dacă o cale nu funcționează, revine și explorează alta. Superior CoT pe probleme care necesită explorare (puzzle, planning, creative).

## Pași

### Pas 1: Identifică probleme potrivite pentru ToT
ToT e superior CoT pe: puzzles, game playing, creative writing cu constrângeri, planning cu multiple opțiuni, probleme cu dead ends.

### Pas 2: Definește structura arborescentă

**Template ToT:**
```
Solve this problem by exploring multiple approaches. At each step:
1. Generate 2-3 possible next steps
2. Evaluate each option (rate as promising/neutral/dead-end)
3. Continue with the most promising option(s)
4. If stuck, backtrack and try a different path

Problem: {PROBLEM}

Step 1 — Generate options:
Option A: [...]
Option B: [...]
Option C: [...]

Evaluation:
- Option A: [promising/neutral/dead-end] because [reason]
- Option B: [promising/neutral/dead-end] because [reason]
- Option C: [promising/neutral/dead-end] because [reason]

Continuing with Option [best]:

Step 2 — Generate options from chosen path:
[...]
```

### Pas 3: Exemplu complet — Creative Writing
```
Write a short story that includes: a clock, a secret, and rain.

Step 1 — Story premise options:
A) A watchmaker discovers a time-traveling clock during a rainstorm
B) A child finds a secret message inside a clock on a rainy day
C) A detective must solve a mystery before a clock strikes midnight in the rain

Evaluation:
A) Promising — unique angle, good tension
B) Neutral — charming but limited scope
C) Promising — natural narrative arc

Continuing with A:

Step 2 — Opening scene options:
A1) Start with the storm, watchmaker in his shop
A2) Start in media res, clock already discovered
A3) Start with normal day, storm builds gradually

[...continue tree exploration...]
```

### Pas 4: Implementare programmatic (BFS/DFS)
```python
def tree_of_thought(problem, max_depth=3, branching=3):
    root = {"state": problem, "children": []}
    queue = [root]

    while queue:
        node = queue.pop(0)  # BFS
        if depth(node) >= max_depth:
            continue

        # Generate branches
        options = llm(f"Given state: {node['state']}\nGenerate {branching} possible next steps.")
        for option in parse_options(options):
            # Evaluate
            score = llm(f"Rate this path (1-10): {option}")
            child = {"state": option, "score": score, "children": []}
            node["children"].append(child)
            if score >= 7:  # Only continue promising paths
                queue.append(child)

    return find_best_leaf(root)
```

### Pas 5: Simplifică pentru uz manual
Dacă nu vrei implementare programatică:
```
{PROBLEM}

Think about this in a tree-like fashion:
1. Consider 3 different approaches
2. For the most promising approach, consider 3 next steps
3. For the best next step, continue until you reach a solution
4. If you get stuck, go back and try a different branch

Show your exploration tree, then give your final answer.
```

### Pas 6: Evaluează calitatea explorării
Verifică: a generat opțiuni diverse (nu similare)? A evaluat corect? A făcut backtracking când necesar?

## Verificare

- [ ] Multiple opțiuni generate la fiecare pas (minim 2)
- [ ] Fiecare opțiune evaluată explicit
- [ ] Opțiunile dead-end abandonate (backtracking)
- [ ] Arborele explorat suficient de profund
- [ ] Soluția finală provine din cea mai bună cale

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — gestionează arbori complecși |
| GPT-4/5 | Excelent |
| Claude Sonnet | Bun pentru arbori simpli (2-3 niveluri) |

## Note

- **Când să folosești**: Puzzle-uri, planning, creative cu constrângeri, design decisions, game-playing, orice cu multiple căi posibile.
- **Când NU**: Probleme cu o singură cale corectă; task-uri simple; token budget limitat (ToT e costisitor).
- **Greșeli comune**: Opțiuni prea similare (nu explorează diversitate); arbore prea adânc; neefectuarea backtracking-ului; evaluarea incorectă a branch-urilor.
- **Relație**: PR-022 (CoT linear), PR-037 (Recursion-of-Thought), PR-040 (Skeleton-of-Thought).
