---
id: C-070
title: Intention Prompting
domain: ai-prompt-engineering
version: 1.0
forge_audit: PASS
created: 2026-03-06
source: OPT Round 2 Synthesis | arxiv:2507.22134v1 | Springer taxonomy (link.springer.com/article/10.1007/s11704-025-50058-z)
---

## §0 — Objective

Reduce the **intent-prompt gap** by explicitly encoding user intent — the WHY behind a task — into the prompt structure before specifying the WHAT. Results in outputs aligned to real goals, not surface instructions.

## §1 — Theory

Standard prompts encode TASK but omit INTENT. The **intent-prompt gap** (SD-A research, 2026) = systematic divergence between:
- User's latent communicative goal (implicit constraints, background assumptions, success criteria)
- What the prompt actually specifies (usually just the surface request)

Intention Prompting closes this gap with an explicit 4-part structure:

```
INTENT:   [WHY you need this — the real goal]
CONTEXT:  [system/project/constraints relevant to intent]
TASK:     [specific deliverable]
SUCCESS:  [what "done well" looks like — criteria + format]
```

**Evidence**: IntentFlow (arxiv:2507.22134v1) shows separating high-level goals from low-level intent reduces misalignment. CoPrompter (ACM IUI 2025) generates explicit criteria per prompt for alignment scoring.

## §2 — When to Apply

**USE when**:
- Complex multi-step task where model might optimize for wrong sub-goal
- Task where implicit constraints matter (architecture decisions, content strategy)
- High-stakes outputs (production code, client deliverables)
- User has previously received "technically correct but wrong" answers

**SKIP when**:
- Simple lookups ("what is X")
- Direct commands with clear scope ("git status")
- Follow-up in active conversation where intent is established

**Pentru reasoning models**: folosește INTENT + TASK doar (omite CONTEXT dacă repetă task-ul; omite SUCCESS dacă e auto-evident).

## §3 — Procedure

### Step 1: Intent Extraction (before writing prompt)
Ask yourself or the user:
1. "What decision will this output inform?"
2. "What would make this output WRONG even if technically correct?"
3. "Who is the audience and what action do they take after reading?"
4. "What are the implicit constraints not stated in the task?"

### Step 2: Build INTENT block
```
INTENT: [1-2 sentences: real goal, decision being made, outcome needed]
```

### Step 3: Build CONTEXT block
Include: project state, constraints, prior decisions, audience profile. Keep ≤ 5 bullet points.

### Step 4: Build TASK block
Specific, measurable deliverable. Use numbered steps if multi-part.

### Step 5: Build SUCCESS block
SUCCESS CRITERIA: include cerințe pozitive de calitate ('output-ul trebuie să X', 'asigură Y'). Convertește criterii negative ('evită Z') în echivalente pozitive ('focusează-te pe A în locul lui Z').
- Format (length, structure, sections)
- Quality threshold ("no hedging", "include code", "cite sources")

### Step 5.5: Permission framing (opțional)
Dacă output-ul va conține afirmații factuale sau recomandări, adaugă: "Dacă ești nesigur, exprimă nivelul de încredere în loc să proiectezi certitudine."

### Step 6: Assemble + validate
Full template:
```
INTENT:
[WHY — real goal, not surface request]

CONTEXT:
- [constraint/background 1]
- [constraint/background 2]
- [relevant prior state]

TASK:
[specific deliverable — what to produce]

SUCCESS CRITERIA:
- [format requirement]
- [quality requirement]
- [criteriu pozitiv de calitate — nu negativ]
```

## §4 — Verification Keys

- `VK [C-070]: intent extracted — gap identified: YES/NO`
- `VK [C-070]: INTENT block present — real goal stated beyond surface task`
- `VK [C-070]: SUCCESS CRITERIA include cel puțin 1 criteriu pozitiv de calitate`

## §5 — Anti-Patterns

- Restating the task in the INTENT block ("INTENT: to write a function" = useless — that's the TASK)
- Empty CONTEXT block — always include at least system/project state
- Missing SUCCESS CRITERIA — most common gap in standard prompts
- Over-specifying SUCCESS CRITERIA to the point of constraining the model's approach

## §6 — Cortex Log

After applying, save pattern:
```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"INTENTION-PROMPTING: [task] | intent: [stated] | gap closed: [description] | quality delta: [better/same]\",
       \"collection\":\"procedures\",
       \"metadata\":{\"type\":\"intention-prompting\",\"procedure\":\"C-070\"}}'"
```
