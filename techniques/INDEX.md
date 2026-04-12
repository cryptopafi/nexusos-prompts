# AI Prompt Engineering Procedures — Index

**Domain**: AI Prompt Engineering
**Rule**: META-H-002
**Total procedures**: 20
**Last updated**: 2026-03-04
**Tech version**: 1.3

---

## Procedures by ID

| ID | File | Title | Complexity | Pattern Tag Status |
|----|------|-------|------------|---------------------|
| C-042 | [C-042-persona-pattern.md](C-042-persona-pattern.md) | Apply the Persona Pattern | BASIC | Persona FORGED |
| C-043 | [C-043-question-refinement-pattern.md](C-043-question-refinement-pattern.md) | Apply the Question Refinement Pattern | BASIC | Question Refinement FORGED |
| C-044 | [C-044-cognitive-verifier-pattern.md](C-044-cognitive-verifier-pattern.md) | Apply the Cognitive Verifier Pattern | BASIC | Cognitive Verifier FORGED |
| C-045 | [C-045-audience-persona-pattern.md](C-045-audience-persona-pattern.md) | Apply the Audience Persona Pattern | BASIC | Audience Persona FORGED |
| C-046 | [C-046-flipped-interaction-pattern.md](C-046-flipped-interaction-pattern.md) | Apply the Flipped Interaction Pattern | INTERMEDIATE | Flipped Interaction FORGED |
| C-047 | [C-047-game-play-pattern.md](C-047-game-play-pattern.md) | Apply the Game Play Pattern | INTERMEDIATE | Game Play FORGED |
| C-048 | [C-048-template-pattern.md](C-048-template-pattern.md) | Apply the Template Pattern | INTERMEDIATE | Template FORGED |
| C-049 | [C-049-meta-language-creation-pattern.md](C-049-meta-language-creation-pattern.md) | Apply the Meta-Language Creation Pattern | ADVANCED | Meta-Language FORGED |
| C-050 | [C-050-recipe-pattern.md](C-050-recipe-pattern.md) | Apply the Recipe Pattern | INTERMEDIATE | Recipe FORGED |
| C-051 | [C-051-alternative-approaches-pattern.md](C-051-alternative-approaches-pattern.md) | Apply the Alternative Approaches Pattern | INTERMEDIATE | Alternative Approaches FORGED |
| C-052 | [C-052-ask-for-input-pattern.md](C-052-ask-for-input-pattern.md) | Apply the Ask-for-Input Pattern | BASIC | Ask-for-Input FORGED |
| C-053 | [C-053-outline-expansion-pattern.md](C-053-outline-expansion-pattern.md) | Apply the Outline Expansion Pattern | INTERMEDIATE | Outline Expansion FORGED |
| C-054 | [C-054-menu-actions-pattern.md](C-054-menu-actions-pattern.md) | Apply the Menu Actions Pattern | INTERMEDIATE | Menu Actions FORGED |
| C-055 | [C-055-fact-check-list-pattern.md](C-055-fact-check-list-pattern.md) | Apply the Fact Check List Pattern | BASIC | Fact Check List FORGED |
| C-056 | [C-056-tail-generation-pattern.md](C-056-tail-generation-pattern.md) | Apply the Tail Generation Pattern | BASIC | Tail Generation FORGED |
| C-057 | [C-057-semantic-filter-pattern.md](C-057-semantic-filter-pattern.md) | Apply the Semantic Filter Pattern | BASIC | Semantic Filter FORGED |
| C-058 | [C-058-few-shot-chain-of-thought.md](C-058-few-shot-chain-of-thought.md) | Apply Few-Shot Examples with Chain-of-Thought | INTERMEDIATE | Few-Shot + CoT FORGED |
| C-059 | [C-059-react-prompting-pattern.md](C-059-react-prompting-pattern.md) | Apply the ReAct Prompting Pattern | ADVANCED | ReAct FORGED |
| C-060 | [C-060-llm-self-grading.md](C-060-llm-self-grading.md) | Use LLM Self-Grading for Quality Assurance | INTERMEDIATE | Self-Grading FORGED |
| C-061 | [C-061-combine-prompt-patterns.md](C-061-combine-prompt-patterns.md) | Combine Multiple Prompt Patterns | ADVANCED | Multi-Pattern FORGED |

---

## Procedures by Complexity

### BASIC (6 procedures)
- C-042 Persona Pattern
- C-043 Question Refinement Pattern
- C-044 Cognitive Verifier Pattern
- C-045 Audience Persona Pattern
- C-052 Ask-for-Input Pattern
- C-055 Fact Check List Pattern
- C-056 Tail Generation Pattern
- C-057 Semantic Filter Pattern

### INTERMEDIATE (9 procedures)
- C-046 Flipped Interaction Pattern
- C-047 Game Play Pattern
- C-048 Template Pattern
- C-050 Recipe Pattern
- C-051 Alternative Approaches Pattern
- C-053 Outline Expansion Pattern
- C-054 Menu Actions Pattern
- C-058 Few-Shot + Chain-of-Thought
- C-060 LLM Self-Grading

### ADVANCED (3 procedures)
- C-049 Meta-Language Creation Pattern
- C-059 ReAct Prompting Pattern
- C-061 Combine Multiple Prompt Patterns

---

## Procedures by Use Case

### Input / Context Gathering
- C-043 Question Refinement — surface what the AI needs before answering
- C-046 Flipped Interaction — AI interviews you to gather all inputs
- C-052 Ask-for-Input — AI blocks until it has required inputs

### Reasoning Enhancement
- C-044 Cognitive Verifier — decompose complex questions into sub-questions
- C-058 Few-Shot + Chain-of-Thought — demonstrate reasoning patterns via examples
- C-059 ReAct — interleave reasoning and action with observation grounding

### Output Structure and Format
- C-048 Template Pattern — lock output structure with explicit placeholders
- C-053 Outline Expansion — build long-form content from outline progressively
- C-050 Recipe Pattern — get complete ordered plan from goal to output

### Role and Audience
- C-042 Persona Pattern — AI adopts a specific expert identity
- C-045 Audience Persona Pattern — calibrate output for a defined reader

### Exploration and Navigation
- C-047 Game Play Pattern — gamify interaction for structured diversity
- C-051 Alternative Approaches — get multiple distinct solutions
- C-054 Menu Actions Pattern — navigate interaction through menus
- C-056 Tail Generation — AI suggests high-value follow-up directions

### Quality and Verification
- C-055 Fact Check List — AI generates verifiable claim list with content
- C-057 Semantic Filter — apply meaning-based criteria as output filter
- C-060 LLM Self-Grading — structured rubric-based quality assessment loop

### Advanced Composition
- C-049 Meta-Language Creation — design domain-specific AI shorthand
- C-061 Combine Multiple Patterns — stack patterns for complex tasks

---

## Integration with Training Curriculum

These procedures are part of the AI/Prompt Engineering training domain. Suggested learning sequence:

**Week 1 — Foundations (BASIC)**
C-042 → C-043 → C-044 → C-045 → C-052 → C-055 → C-056 → C-057

**Week 2 — Structured Workflows (INTERMEDIATE)**
C-048 → C-053 → C-050 → C-046 → C-051 → C-054 → C-058 → C-047 → C-060

**Week 3 — Advanced Patterns**
C-049 → C-059 → C-061

---

## Cortex Collection

All procedures log to collection: `training`
Domain tag: `ai_prompt_engineering`
Rule: `META-H-002`
