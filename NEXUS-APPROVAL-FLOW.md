---
id: NEXUS-APPROVAL-FLOW
version: "1.0"
created: "2026-03-09"
author: "Genie (FORGE'd)"
domain: nexus-ops
type: procedure
status: ACTIVE
decision_ref: "D-P2 (2026-03-09)"
---

# NEXUS-APPROVAL-FLOW — Gate Action Approval via Telegram

## Purpose
Defines how Pafi approves or rejects gate actions (Rosu level) from NexusOS agents.

## Scope
All agent actions classified as Rosu (deploy, push, spend, infrastructure changes, external-facing outputs).

## Flow

### 1. Agent Identifies Gate Action
Agent detects task requires Rosu-level approval per its SOUL.md autonomy table.
Agent writes in PROGRESS.md:
```yaml
status: BLOCKED
blocked_reason: "gate_approval_required"
gate_action: "{description of what needs approval}"
gate_level: "rosu"
```

### 2. GENIE Detects Block
GENIE reads PROGRESS.md at next heartbeat (30min max).
GENIE writes approval request to `intel/SENTINEL-NOTIFY.md`:
```yaml
type: approval_request
agent: "{agent_name}"
task_id: "{task-id}"
action: "{what the agent wants to do}"
risk: "{HIGH/CRITICAL}"
timestamp: "YYYY-MM-DD HH:MM"
```

### 3. SENTINEL Sends Telegram
SENTINEL reads SENTINEL-NOTIFY.md, sends via relay bot:
```
[APPROVAL REQUIRED]
Agent: {agent_name}
Task: {task-id}
Action: {description}
Risk: {level}

Reply OK or REJECT
```

### 4. Pafi Responds
- **OK** → relay writes to `intel/APPROVAL-RESPONSE.md`:
  ```yaml
  approved: true
  task_id: "{task-id}"
  timestamp: "YYYY-MM-DD HH:MM"
  ```
- **REJECT** → relay writes to `intel/APPROVAL-RESPONSE.md`:
  ```yaml
  approved: false
  task_id: "{task-id}"
  reason: "{Pafi's reason if provided}"
  timestamp: "YYYY-MM-DD HH:MM"
  ```

### 5. GENIE Unblocks or Cancels
GENIE reads `intel/APPROVAL-RESPONSE.md` at next heartbeat.
- Approved → updates agent PROGRESS.md: `status: IN_PROGRESS`, removes block
- Rejected → updates agent PROGRESS.md: `status: CANCELLED`, `cancel_reason: "rejected by Pafi"`

### 6. Timeout
If no response within 1 hour: task auto-cancelled, GENIE notified via APPROVAL-RESPONSE.md with `approved: false, reason: "timeout"`.

## Constraints
- One-writer rule: SENTINEL writes Telegram, relay writes approval file, GENIE reads
- No agent may bypass gate by reclassifying action as Verde/Galben
- Approval is per-task, not blanket — each Rosu action needs its own approval
- Approval log persisted in `intel/data/YYYY-MM-DD/approvals.jsonl`

## Anti-Patterns
- Agent polling Telegram directly (only SENTINEL sends)
- Batching multiple gate actions in one approval request (one per task)
- Auto-approving after timeout (always cancel, never auto-approve)
