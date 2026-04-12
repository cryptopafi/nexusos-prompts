---
id: C-071
title: Context Engineering
domain: ai-prompt-engineering
version: 1.0
forge_audit: PASS
created: 2026-03-06
source: OPT Round 2 Synthesis | elastic.co/search-labs/blog/context-engineering-vs-prompt-engineering | arxiv:2503.02400v2 | DAIR promptingguide.ai
---

## §0 — Objective

Design the **full information environment** for an LLM call — not just the prompt text, but ALL inputs that shape model behavior: memory, retrieved data, tool outputs, conversation history, system instructions, and dynamic context injection.

## §1 — Theory

**Prompt Engineering** = crafting the text input (creative writing, instruction design).
**Context Engineering** = systems design — architecting what information enters the context window and in what form.

DAIR (2025): "Prompt engineering is now being rebranded as context engineering."

**Context Window Components** (all must be intentionally designed):
```
┌─────────────────────────────────────┐
│ SYSTEM PROMPT (persona, rules)      │
├─────────────────────────────────────┤
│ INJECTED MEMORY (Cortex/RAG hits)   │
├─────────────────────────────────────┤
│ TOOL OUTPUTS (search, code, APIs)   │
├─────────────────────────────────────┤
│ CONVERSATION HISTORY (pruned)       │
├─────────────────────────────────────┤
│ USER PROMPT + TASK                  │
└─────────────────────────────────────┘
```

**Key difference from PE**: PE = "write a better question". CE = "ensure the model has ALL relevant information in the right format, at the right position in the window, pruned of noise."

**Computational cost**: CE > PE — dynamic retrieval, tool orchestration, memory management add latency/overhead. Use when PE alone insufficient.

## §2 — When to Apply

**USE for**:
- RAG systems, agentic workflows, multi-turn complex tasks
- Tasks requiring external knowledge not in model weights
- Production systems where consistency and factuality critical
- Claude Code (Genie) sessions on multi-session projects

**SKIP when**:
- Single-turn, self-contained task
- No external knowledge needed
- PE alone produces adequate results

**Reasoning models (Opus extended thinking, o1/o3)**: reduce INJECTED MEMORY la cele mai relevante items — reasoning nativ performează mai bine cu mai puțin context noise.

## §3 — Procedure

### Step 1: Audit context requirements
For the task, identify WHICH context components are needed:
- [ ] Static knowledge (system prompt, procedures)
- [ ] Dynamic knowledge (Cortex search, RAG retrieval)
- [ ] Tool state (recent outputs, search results)
- [ ] Conversation history (prune to relevant turns only)
- [ ] User intent (from C-070 Intention Prompting)

### Step 2: Position design (context window placement)
Optimal positions per component:
- **System instructions** → top of system prompt
- **Examples (few-shot)** → immediately before task
- **Retrieved data** → after task description, before output format
- **Conversation history** → pruned to last N relevant turns + key decisions
- **Tool outputs** → inline at point of use

### Step 3: Noise reduction
Remove from context:
- Redundant conversation turns (keep only turns that changed state)
- Failed attempts and error messages (unless relevant)
- Tangential discussions
- Duplicate information from multiple retrievals

### Step 4: Context injection template
For Genie use, the injection order is:
```xml
<system>[Identity, rules, constraints]</system>

<memory>[Cortex retrievals relevant to task]</memory>

<tools>[Tool outputs if applicable]</tools>

<history>[Pruned conversation — key decisions only]</history>

<task>[C-070 INTENT/CONTEXT/TASK/SUCCESS structure]</task>
```

### Step 5: Prompt caching layer (production)
For repeated calls with stable prefixes:
- Identify stable context prefix (system prompt + memory = cacheable)
- Variable suffix (task + user input = not cached)
- Anthropic: 90% cost reduction on cached prefix
- Place cache boundary after stable content, before dynamic content

### Step 6: Validate context quality
Ask before submitting:
1. Does the model have all information needed to answer without hallucinating?
2. Is there noise (irrelevant context) that could misdirect?
3. Is the most important information positioned prominently (not buried)?
4. Is the total length within model context limit with margin?
5. Contextul oferă modelului suficient grounding pentru a exprima incertitudine în loc să halucineze pe gaps? Dacă nu, injectează permisiunea explicită de incertitudine.

## §4 — Verification Keys

- `VK [C-071]: context audit complete — components: [list]`
- `VK [C-071]: noise pruned — removed: [N] items`
- `VK [C-071]: cache boundary identified: YES/NO`

## §5 — Anti-Patterns

- **Context flooding**: injecting all available Cortex results without filtering → noise overwhelms signal
- **Middle burial**: placing critical instructions in the middle of long context (lost in middle problem)
- **No history pruning**: passing full conversation history → stale/contradictory context
- **Static context for dynamic tasks**: not refreshing retrieved data on state changes
- **PE-only for agent tasks**: using just prompt text for complex agentic workflows where state management is needed

## §6 — Cortex Log

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"CONTEXT-ENG: [task type] | components used: [list] | noise pruned: YES/NO | cache: YES/NO | quality: [rating]\",
       \"collection\":\"procedures\",
       \"metadata\":{\"type\":\"context-engineering\",\"procedure\":\"C-071\"}}'"
```
