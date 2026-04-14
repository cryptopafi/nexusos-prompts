---
type: procedure
created: 2026-03-17
status: active
slug: nexus-approval-gate
tags: [procedure, nexus]
---

# NEXUS-APPROVAL-GATE — V/G/R Human Approval Flow

## Overview
Three-decision gate for human-in-the-loop control of NexusOS task execution.
Decisions: **Veto** (reject), **Go** (approve), **Redirect** (reroute to different agent/approach).

## When Triggered
- Task `spent_usd >= budget_usd` (100% of assigned budget, per NEXUS-COST-TRACKING.md Step 3b)
- Agent requests explicit human approval (PROGRESS.md: `needs_approval: true`)
- GENIE escalates blocked task after 24h (via SENTINEL-NOTIFY.md)
- Any task classified as `gate_level: "rosu"` per routing-table.yaml approval config

## Flow

```
GENIE detects approval-needed
  → writes SENTINEL-NOTIFY.md (request)
  → SENTINEL health-check.sh reads SENTINEL-NOTIFY.md
  → SENTINEL sends Telegram message to Pafi (via send_telegram)
  → SENTINEL writes APPROVAL-RESPONSE.md: status=PENDING
  → SENTINEL clears SENTINEL-NOTIFY.md

Pafi reads Telegram message
  → replies: "OK", "REJECT [reason]", or "REDIRECT [agent] [reason]"
  → Telegram relay detects approval response pattern
  → relay writes APPROVAL-RESPONSE.md with decision

GENIE heartbeat (Step 7)
  → reads APPROVAL-RESPONSE.md
  → if approved: unblocks agent, sets PROGRESS.md → EXECUTING
  → if rejected: sets PROGRESS.md → FAILED, reason from Pafi
  → if redirected: re-routes task to specified agent
  → clears APPROVAL-RESPONSE.md after processing
```

## File Formats

### SENTINEL-NOTIFY.md (written by GENIE, read by SENTINEL)
```yaml
---
msg_type: approval
from: genie
to: sentinel
correlation_id: "research-001"
timestamp: "2026-03-10T12:00:00Z"
priority: high
---
task_id: "research-001"
agent: iris
action: "Execute high-cost research task"
cost_estimate_usd: 3.50
reason: "Exceeds cost guardrail warn threshold"
```

### APPROVAL-RESPONSE.md (written by SENTINEL/relay, read by GENIE)
```yaml
---
msg_type: approval
from: relay
to: genie
correlation_id: "research-001"
timestamp: "2026-03-10T12:05:00Z"
in_reply_to: "research-001"
status: APPROVED  # PENDING | APPROVED | REJECTED | REDIRECTED
---
task_id: "research-001"
decision: "GO"          # GO | VETO | REDIRECT
responded_by: "pafi"
responded_via: "telegram"
reason: ""              # optional, required for VETO/REDIRECT
redirect_agent: ""      # only for REDIRECT
```

## Response Patterns (Telegram)
Pafi can reply with:
- `OK` or `GO` or `APPROVE` → decision: GO, status: APPROVED
- `REJECT` or `VETO` → decision: VETO, status: REJECTED
- `REJECT too expensive` → decision: VETO, reason: "too expensive"
- `REDIRECT mercury` → decision: REDIRECT, redirect_agent: mercury
- `REDIRECT tech use codex instead` → decision: REDIRECT, redirect_agent: tech, reason: "use codex instead"

## Timeouts
- From routing-table.yaml: `approval.timeout_s: 3600` (1 hour)
- If no response after 1h: GENIE marks task BLOCKED with reason "approval timeout"
- If no response after 24h: GENIE marks task FAILED

## One-Writer Principle
- SENTINEL-NOTIFY.md: written by GENIE, read+cleared by SENTINEL
- APPROVAL-RESPONSE.md: sequential handoff — SENTINEL writes PENDING, then relay overwrites with decision, then GENIE reads+clears. The `status` field acts as a state machine guard:
  - SENTINEL only writes when file is empty or cleared (no status field)
  - Relay only writes when status=PENDING (gated in handleApprovalResponse)
  - GENIE only clears after processing a terminal status (APPROVED/REJECTED/REDIRECTED)
  - Race window: if SENTINEL writes a new PENDING while relay is writing a decision for the previous request, the relay's write wins (last-writer-wins). This is acceptable because approval requests are infrequent (hours apart) and the relay response is near-instant after Pafi replies.
- GENIE as orchestrator may write to other agents' PROGRESS.md for supervisor actions: approval gate transitions (GO→EXECUTING, VETO→FAILED), blocked escalation (BLOCKED→FAILED after 48h). This is an intentional one-writer exception for the orchestrator role.
