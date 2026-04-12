# Apply the Game Play Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Structures an AI interaction as a game with defined rules, goals, scoring, and progression mechanics to elicit higher-quality outputs through structured constraint and engagement.

---

## 1. Problema

Free-form prompting can produce unfocused outputs and misses the value of structured iteration. Gamification imposes beneficial constraints — rules, win conditions, challenge levels — that guide the AI toward more creative, systematic, or rigorously constrained responses while increasing engagement.

Situations covered:
- Brainstorming sessions that need structured diversity (e.g., "generate 10 unique solutions, no idea can reuse a previous mechanism")
- Training and quiz scenarios where the AI acts as quiz master
- Iterative refinement where each round must improve on the last (scored progression)
- Exploration tasks where you need the AI to be methodical rather than jumping to easy answers

---

## 2. Procedura

### Pas 1: Define the Game Structure
Before any gameplay, write out the full game specification:
- **Objective**: What does winning look like?
- **Rules**: What is allowed and forbidden in each turn?
- **Scoring**: How are outputs evaluated? What earns points?
- **Rounds**: How many turns does the game have?
- **Win/fail condition**: When does the game end and what is the outcome?

### Pas 2: Present Game Rules to the AI
Submit the game specification to the AI and ask it to confirm it understands the rules. Ask: "Please restate the rules in your own words before we begin to confirm your understanding." Correct any misinterpretations before the first round.

### Pas 3: Execute Round 1
Begin the game. Make your first move according to the rules. The AI should respond according to the game mechanics you defined. Keep the interaction strictly within the game frame — do not allow the AI to break game structure for convenience.

### Pas 4: Apply Scoring and Enforce Rules
After each round, explicitly score the AI's output against your defined criteria. Announce the score. If a rule was violated, call it out and either penalize or request a redo. This enforcement is what makes the game structure produce better outputs than free-form prompting.

### Pas 5: Increase Challenge with Level Progression
If the game supports progression, increase the difficulty after Round 2 or 3. This could mean: stricter constraints, harder input cases, requiring more original approaches, or raising the minimum score threshold to continue. Progression prevents the AI from settling into a comfortable pattern.

### Pas 6: End Game and Extract Outputs
At game completion, collect the highest-value outputs generated during gameplay. Ask the AI to summarize the "winning" solutions or moments from the game. These are your actual deliverables — the game was a mechanism for generating them.

### Pas 7: Post-Game Debrief
Ask the AI: "What was the most creative or effective approach you took during the game, and why?" This reflection often surfaces meta-insights about the problem space that weren't explicit during gameplay.

---

## 3. Cortex Logging

```json
{
  "text": "Game Play Pattern applied. Game type: [description]. Rounds played: [count]. Scoring criteria: [summary]. Best outputs generated: [key results]. Pattern effectiveness: [assessment].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-047",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "game_play"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Pattern aplicat fără documentare → violation
- Reguli de joc nedocumentate înainte de start → violation
- Jocul continuat după ce scopul pedagogic a fost atins fără evaluare → violation
- Runner: Opus audit periodic + FORGE-AUDIT batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum AI/Prompt Engineering

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Pattern corect aplicat în minim un exemplu real?

**VK-uri obligatorii:**
1. `✅ [PROC] C-047 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Game Play Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | FORGE-AUDIT |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| LLM access (Claude/ChatGPT) | Pattern application and testing |
| Prompt testing environment | Iteration and validation |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Pattern applications | Minim 2 exemple per execuție |
