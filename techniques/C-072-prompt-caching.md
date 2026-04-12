---
id: C-072
title: Prompt Caching for Production
domain: ai-prompt-engineering
version: 1.0
forge_audit: PASS
created: 2026-03-06
source: OPT Round 2 Synthesis | thomas-wiegold.com/blog/prompt-engineering-best-practices-2026 | Anthropic docs | lakera.ai/blog/prompt-engineering-guide
---

## §0 — Objective

Dramatically reduce cost and latency for LLM production systems by caching stable prompt prefixes. Applies whenever the same system prompt or context is sent repeatedly across multiple API calls.

## §1 — Theory

**Prompt Caching** = API feature that caches the computation of a prefix portion of the prompt. On cache hit, the model skips reprocessing the cached portion.

**Impact**:
- **Anthropic**: up to **90% cost reduction** on cached tokens, **85% latency reduction**
- **OpenAI**: automatic caching (no explicit setup), **50-90% discount** depending on model

**How it works**:
```
Request 1: [SYSTEM PROMPT (500 tokens) + TASK (50 tokens)]
           → cache created for system prompt portion
           → billed: 550 tokens full price

Request 2: [SYSTEM PROMPT (500 tokens, CACHED) + TASK (50 tokens)]
           → cache hit for 500 tokens
           → billed: 50 tokens full + 500 tokens at 10% price
           → savings: 90% on 500 tokens
```

**Cache invalidation**: cache expires if prefix changes. Even 1 token change = cache miss.

## §2 — When to Apply

**USE when**:
- Same system prompt/instructions used across multiple calls (production systems)
- RAG: large knowledge base injected repeatedly across sessions
- Genie sessions with long CLAUDE.md + procedures loaded repeatedly
- Batch processing with fixed template

**SKIP when**:
- One-off single calls
- Prefix changes frequently (dynamic system prompts)
- Cost not a concern (dev/test)

## §3 — Procedure

### Step 1: Identify stable vs dynamic content

Analyze prompt for cacheability:
```
STABLE (cache-able):
- System instructions / persona definition
- Fixed few-shot examples
- Static knowledge base / procedures
- Genie: CLAUDE.md + core procedures

DYNAMIC (not cached):
- Current task
- User input
- Real-time tool outputs
- Session-specific context
```

### Step 2: Restructure prompt — stable content first

**Cache boundary rule**: ALL stable content must come BEFORE any dynamic content.

Optimal structure:
```
[STABLE BLOCK — cache this]
- System instructions
- Persona/role definition
- Fixed few-shot examples
- Static procedures/knowledge

[CACHE BOUNDARY] ←── place cache marker here (Anthropic: cache_control parameter)

[DYNAMIC BLOCK — never cached]
- Current user message
- Retrieved data (if changes per query)
- Tool outputs
- Task-specific context
```

### Step 3: Anthropic implementation
Use `cache_control` parameter on the message:
```python
messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": STABLE_SYSTEM_CONTENT,
                "cache_control": {"type": "ephemeral"}  # marks cache boundary
            },
            {
                "type": "text",
                "text": dynamic_user_message  # not cached
            }
        ]
    }
]
```

### Step 4: Measure cache performance
Track in production:
- Cache hit rate (target: >70% for steady-state workflows)
- Cost per call with vs without caching
- Latency reduction

### Step 5: Promptfoo integration for eval
Add prompt regression testing alongside caching:
```yaml
# promptfoo.yaml
prompts:
  - file://prompts/v1.txt
  - file://prompts/v2.txt
tests:
  - vars:
      task: "summarize this document"
    assert:
      - type: contains
        value: "key finding"
```
Run: `npx promptfoo eval` — A/B test prompt variants before shipping.

## §4 — Verification Keys

- `VK [C-072]: stable/dynamic split identified — stable tokens: [N]`
- `VK [C-072]: cache_control applied: YES/NO`
- `VK [C-072]: expected savings: ~[X]% on cached prefix`

## §5 — Anti-Patterns

- **Dynamic content in stable block**: injecting session-specific data into the cached portion → constant cache invalidation
- **Ignoring cache in production**: running 10K+ daily calls without caching = 10x unnecessary spend
- **No eval pipeline**: changing prompts without Promptfoo A/B testing → silent quality regression
- **Over-caching**: caching content that should be fresh (real-time data, user preferences) → stale outputs

## §6 — Cortex Log

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"PROMPT-CACHING: [system] | stable tokens: [N] | cache provider: [anthropic/openai] | expected savings: [X]%\",
       \"collection\":\"procedures\",
       \"metadata\":{\"type\":\"prompt-caching\",\"procedure\":\"C-072\"}}'"
```
