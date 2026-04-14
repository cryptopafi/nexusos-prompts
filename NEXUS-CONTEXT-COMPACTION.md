---
procedure: "NEXUS-CONTEXT-COMPACTION"
version: "1.0"
created: "2026-03-10"
owner: "all-agents"
trigger: "before /compact or context window >80%"
---

# NEXUS-CONTEXT-COMPACTION — Checkpoint & Recovery

## Purpose
Prevent context loss when `/compact` triggers. Agent writes checkpoint before compaction, reads it after to resume seamlessly.

## Pre-Compaction (BEFORE /compact)

Write `~/.nexus/agents/{agent_id}/workspace/checkpoint.md` with:

```markdown
---
agent: "{agent_id}"
task_id: "{current task ID or null}"
timestamp: "{ISO 8601}"
status: "{IDLE|PLANNING|EXECUTING|REVIEWING|BLOCKED|DONE}"
---

## Active Task
{1-2 sentence summary of what you were doing}

## Key Findings So Far
- {bullet 1}
- {bullet 2}
- {bullet 3}

## Next Action
{Exact next step to take when resuming}

## Files In Scope
- {file path 1 — why it matters}
- {file path 2 — why it matters}

## Decisions Made This Session
- {decision 1}
- {decision 2}
```

## Post-Compaction (AFTER /compact resumes)

1. Read `~/.nexus/agents/{agent_id}/workspace/checkpoint.md`
2. If exists and non-empty: resume from `Next Action`
3. If missing: read PROGRESS.md + HEARTBEAT.md to reconstruct state
4. Delete checkpoint.md after successful resume (one-time use)

## Rules
- Checkpoint is agent-private (one-writer: agent itself)
- Max 500 words (keep it compact — ironic but necessary)
- Do NOT checkpoint SOUL.md content (it's 444-locked and always loadable)
- Do NOT store raw research data (store pointers: file paths + line ranges)
- Overwrite previous checkpoint (no versioning needed — latest only)
