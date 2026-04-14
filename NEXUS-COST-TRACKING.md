---
type: procedure
created: 2026-03-17
status: active
slug: nexus-cost-tracking
tags: [procedure, nexus]
---

# NEXUS-COST-TRACKING — Per-Task Budget Tracking

## Overview
Every NexusOS task tracks estimated cost in PROGRESS.md. GENIE monitors cost guardrails
from routing-table.yaml and escalates when thresholds are exceeded.

## PROGRESS.md Cost Fields
Each agent adds these fields when starting a task:

```yaml
# Cost tracking (written by owning agent, read by GENIE)
budget_usd: 2.00          # from routing-table cost_guardrails.warn_usd (default)
spent_usd: 0.00           # cumulative estimate updated after each LLM call
model_calls:              # log of LLM invocations for this task
  - model: "claude-haiku-4-5-20251001"
    tokens_in: 1200
    tokens_out: 450
    est_usd: 0.002
```

## Cost Estimation Formula
Agents estimate cost after each Claude CLI call using token counts from output:
- Haiku: $0.80/M input + $4.00/M output
- Sonnet: $3.00/M input + $15.00/M output
- Opus: $15.00/M input + $75.00/M output

Agents write `spent_usd` as running total. Precision: 3 decimal places.

## Budget Assignment
When GENIE routes a task:
1. Read `cost_guardrails` from routing-table.yaml
2. Set `budget_usd` = `warn_usd` (default $2.00) for standard tasks
3. Set `budget_usd` = `halt_usd` ($5.00) for high-complexity tasks
4. Set `budget_usd` = `session_cap_usd` ($20.00) for orchestration-level tasks

## GENIE Guardrail Checks (Heartbeat Step 3b)
At each heartbeat, for each EXECUTING task:
- `spent_usd >= budget_usd * 0.8` → add warning to GENIE-STATUS.md
- `spent_usd >= budget_usd` → trigger approval gate (write SENTINEL-NOTIFY.md)
- `spent_usd >= session_cap_usd` → BLOCKED immediately, escalate to Pafi

## Agent Responsibilities
- **Owning agent**: Updates `spent_usd` and `model_calls` after each LLM invocation
- **GENIE**: Reads cost fields, enforces guardrails, assigns budget
- **SENTINEL**: No cost role (pure bash, no LLM costs)

## One-Writer Principle
Each agent writes ONLY its own PROGRESS.md cost fields during normal execution.
GENIE reads all agents' costs and writes warnings to GENIE-STATUS.md and approval
requests to SENTINEL-NOTIFY.md.

**Exception**: GENIE as orchestrator may write to another agent's PROGRESS.md for
supervisor actions only: BLOCKED→FAILED escalation (>48h), approval gate state
transitions (GO→EXECUTING, VETO→FAILED, REDIRECT→re-route). These are documented
in GENIE HEARTBEAT.md Steps 2, 7 and are the only cross-agent write paths.
