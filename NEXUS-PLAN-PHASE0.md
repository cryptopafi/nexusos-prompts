---
id: NEXUS-PLAN-PHASE0
version: "1.0"
created: "2026-03-09"
author: "Genie (FORGE'd)"
domain: nexus-orchestration
type: procedure
status: ACTIVE
---

# NEXUS-PLAN-PHASE0 — Mandatory Planning Before Multi-Agent Tasks

## Purpose
Every multi-agent task MUST have a Phase 0 planning step before any agent receives work.
Prevents: scope creep, wrong agent routing, missing dependencies, wasted tokens.

## When to Apply
- Task involves 2+ agents
- Task has dependencies between agents (e.g., IRIS research needed before MERCURY brief)
- Task estimated >1h total effort
- Task modifies shared infrastructure (routing, registry, templates)

## Phase 0 Template

### 1. Task Classification
```yaml
task_id: "{id}"
description: "{1-2 sentences}"
domain: "{research | marketing | development | ops | cross-domain}"
complexity: "{low | medium | high}"
```

### 2. Agent Assignment
```yaml
agents_involved:
  - agent: "{name}"
    role: "{what they do for this task}"
    autonomy: "{verde | galben | rosu}"
    depends_on: ["{other agent or null}"]
```

### 3. Dependency Graph
```
{Agent A} → {Agent B} → {Agent C}
         ↘ {Agent D} (parallel)
```
Mark which steps can run in parallel vs sequential.

### 4. Output Contract
```yaml
expected_outputs:
  - agent: "{name}"
    output: "{description}"
    format: "{md | yaml | json}"
    location: "workspace/output/{agent}/{file}"
```

### 5. Risk Assessment
```yaml
risks:
  - risk: "{description}"
    mitigation: "{action}"
    level: "{LOW | MEDIUM | HIGH}"
```

### 6. Go/No-Go
- [ ] All agents assigned exist and are READY in registry
- [ ] Dependencies are acyclic (no circular waits)
- [ ] Output locations do not conflict (one-writer rule)
- [ ] Cost estimate within guardrails
- [ ] No Rosu actions without approval flow ready

If any checkbox fails → STOP, resolve before proceeding.

## Execution
After Phase 0 complete:
1. GENIE creates task files in `tasks/active/` (or TaskCreate via Tasks API)
2. GENIE copies input to each agent's `workspace/current-task/`
3. Agents begin work per dependency order
4. GENIE monitors via heartbeat + PROGRESS.md reads

## Constraints
- Phase 0 is NOT optional for multi-agent tasks
- Single-agent tasks skip Phase 0 (GENIE routes directly)
- Phase 0 output saved as `tasks/active/{task-id}-plan.yaml`
- Plan is immutable once agents start — changes require new Phase 0
