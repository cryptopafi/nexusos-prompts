---
type: procedure
created: 2026-03-17
status: active
slug: nexus-messaging-protocol
tags: [procedure, nexus]
---

# NEXUS-MESSAGING-PROTOCOL — Structured Inter-Agent Messaging v1.1

## Overview
Standard message envelope for all inter-agent communication in NexusOS.
Every file in `~/.nexus/workspace/intel/` MUST use this envelope format.

## Envelope Format (YAML Frontmatter)

```yaml
---
msg_type: request         # REQUIRED: request | response | status | alert | approval | task
from: genie               # REQUIRED: genie | iris | mercury | tech | sentinel | relay | pafi
to: iris                  # REQUIRED: target agent or "broadcast"
correlation_id: "task-001" # REQUIRED: task_id or request_id for threading
timestamp: "2026-03-10T12:00:00Z"  # REQUIRED: ISO 8601 UTC
priority: normal           # OPTIONAL: low | normal | high | critical (default: normal)
in_reply_to: ""            # OPTIONAL: correlation_id of parent message
---
```

## Message Types

### request
Agent or orchestrator dispatching work to another agent.
- **Writer**: GENIE (primary), Pafi (direct)
- **Files**: `IRIS-REQUEST.md`, `IRIS-REQUEST-mercury.md`, `SENTINEL-NOTIFY.md`
- **Required body fields**: task description, output_location, complexity

### response
Agent delivering results for a prior request.
- **Writer**: owning agent only (one-writer principle)
- **Files**: `IRIS-OUTPUT.md`, `APPROVAL-RESPONSE.md`
- **Required body fields**: status (DONE/PARTIAL/FAILED), output summary

### status
Periodic heartbeat or status report.
- **Writer**: owning agent only
- **Files**: `GENIE-STATUS.md`, `SENTINEL-HEALTH.md`
- **Required body fields**: systems map, alerts list

### alert
Urgent notification requiring attention.
- **Writer**: SENTINEL (primary), GENIE (escalation)
- **Files**: `SENTINEL-NOTIFY.md` (when used for alerts vs approval requests)
- **Required body fields**: severity (YELLOW/RED/CRITICAL), alert message

### approval
Human-in-the-loop gate request or response.
- **Writer**: SENTINEL (writes PENDING state in `APPROVAL-RESPONSE.md` after notify), relay/pafi (writes APPROVE/REJECT/REDIRECT response)
- **Files**: `SENTINEL-NOTIFY.md`, `APPROVAL-RESPONSE.md`
- **Required body fields**: See NEXUS-APPROVAL-GATE.md

### task
Task dispatch to agent queue.
- **Writer**: GENIE
- **Files**: `DISPATCH.md` in `active/{task_id}/`
- **Required body fields**: task_description, assigned_agent, complexity, budget_usd, max_turns

## File Ownership (One-Writer Principle)

| File | Writer | Readers |
|------|--------|---------|
| IRIS-REQUEST.md | GENIE | IRIS |
| IRIS-REQUEST-mercury.md | MERCURY | IRIS |
| IRIS-OUTPUT.md | IRIS | GENIE, Pafi |
| SENTINEL-NOTIFY.md | GENIE | SENTINEL (read+clear) |
| SENTINEL-HEALTH.md | SENTINEL | GENIE, Pafi |
| APPROVAL-RESPONSE.md | SENTINEL (PENDING) → relay/pafi (decision) | GENIE (read+clear) |
| GENIE-STATUS.md | GENIE | All agents, Pafi |

## Correlation ID Rules
- For task-routed work: use `task_id` from PROGRESS.md (e.g., `research-001`)
- For approval gates: use `task_id` from the triggering task
- For health/status: use `heartbeat-{cycle_number}` or `health-{timestamp}`
- For ad-hoc requests: use `{from}-{YYYYMMDD}-{seq}` (e.g., `pafi-20260310-001`)

## Backward Compatibility
- Parsers MUST accept both old format (no envelope) and new format (with envelope)
- SENTINEL health-check.sh grep patterns work on body content regardless of envelope
- Body fields like `task_id:` remain unchanged; envelope adds `correlation_id:` as a separate threading field (different field name, no grep conflict)
- See also: NEXUS-LOGGING-STANDARD.md for how `correlation_id` links messages to structured logs
- Migration: update files to new format as they are next written (no bulk migration needed)

## Validation Rules
- `timestamp` MUST be ISO 8601 UTC (ending in Z)
- `from` and `to` MUST be registered agent names from `agent-registry.yaml` or "pafi"/"relay"/"broadcast"
- `correlation_id` MUST NOT be empty for request/response/approval types
- `priority: critical` triggers immediate Telegram notification via SENTINEL

## Example: Research Request

```yaml
---
msg_type: request
from: genie
to: iris
correlation_id: "research-042"
timestamp: "2026-03-10T14:30:00Z"
priority: normal
---

## Research Request: DeFi yield aggregator landscape

Investigate current DeFi yield aggregator protocols.
Focus on: TVL trends, security audit history, fee structures.

**Output location**: ~/.nexus/workspace/intel/IRIS-OUTPUT.md
**Complexity**: medium
**Max length**: 1000 words
**Format**: structured markdown with ## headers per finding
```

## Example: Research Response

```yaml
---
msg_type: response
from: iris
to: genie
correlation_id: "research-042"
timestamp: "2026-03-10T15:45:00Z"
in_reply_to: "research-042"
---

## IRIS Research Output: DeFi Yield Aggregator Landscape

**Status**: DONE
**Sources**: 4 (DeFiLlama, Tavily, ArXiv, Cortex)
**Confidence**: 0.78

## Finding 1: TVL Concentration
[Content...]

## Finding 2: Fee Structures
[Content...]
```

## Example: Approval Request

```yaml
---
msg_type: approval
from: genie
to: sentinel
correlation_id: "research-042"
timestamp: "2026-03-10T16:00:00Z"
priority: high
---

task_id: "research-042"
agent: iris
action: "Execute high-cost research task"
cost_estimate_usd: 3.50
reason: "Exceeds budget_usd threshold"
```

## Example: Health Status

```yaml
---
msg_type: status
from: sentinel
to: broadcast
correlation_id: "health-20260310-1605"
timestamp: "2026-03-10T16:05:00Z"
---

systems:
  cortex: GREEN
  memory_sync: GREEN
  echelon: GREEN
  telegram: GREEN
  codex_pipeline: GREEN

alerts:
  - "none"
autonomous_actions_taken:
  - "none"
```

## Version Changelog
- v1.1 (2026-03-10): Added msg_type:task for Phase 2 autonomous dispatch
