---
type: procedure
created: 2026-03-22
status: active
slug: c-065-mementos-long-sessions
tags: [procedure, nexus]
---

# Use Mementos for Long AI Work Sessions — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Maintain contextual coherence across multi-session AI projects using structured state artifacts (mementos) with Python tooling for automatic generation, validation, and restoration.

---

## 1. Problema

Long or fragmented AI sessions lose critical context — the AI forgets, the user restarts from scratch, or progress is unrecoverable. Mementos are structured state snapshots that enable fluid continuation of any complex AI project across sessions, days, or team members.

**When to use**:
- Multi-day analysis or writing projects with AI
- Workflows with multiple stages executed in separate sessions
- Team handoffs where another person continues an AI-assisted project
- Any session exceeding 1 hour or 20 conversational turns

**When NOT to use**:
- Single-session tasks that complete in <30 minutes
- Stateless queries (lookups, translations, simple Q&A)
- When using a system with built-in persistent memory (e.g., Claude Projects with full history)
- Real-time collaborative editing where state is managed by the tool (Google Docs, Notion)

---

## 2. Procedura

### Pas 1: Define memento template at project start

```python
import json
from datetime import datetime

MEMENTO_TEMPLATE = {
    "project_id": "",
    "project_title": "",
    "objective": "",
    "current_stage": "",
    "progress_pct": 0,
    "decisions_made": [],      # {"decision": str, "rationale": str, "date": str}
    "active_files": [],        # file paths currently in use
    "next_steps": [],          # ordered list of immediate next actions
    "blockers": [],            # open questions or dependencies
    "key_context": "",         # 2-3 sentences of critical context
    "session_count": 0,
    "last_updated": "",
}

def create_memento(project_id, title, objective):
    m = MEMENTO_TEMPLATE.copy()
    m["project_id"] = project_id
    m["project_title"] = title
    m["objective"] = objective
    m["last_updated"] = datetime.now().isoformat()
    return m
```

### Pas 2: Save memento at every session pause

```python
def save_memento(memento, session_summary, decisions=None, next_steps=None):
    """Call at end of every session or major milestone."""
    memento["session_count"] += 1
    memento["last_updated"] = datetime.now().isoformat()
    memento["key_context"] = session_summary

    if decisions:
        for d in decisions:
            memento["decisions_made"].append({
                "decision": d["what"],
                "rationale": d["why"],
                "date": datetime.now().strftime("%Y-%m-%d"),
            })

    if next_steps:
        memento["next_steps"] = next_steps

    path = f"~/.nexus/projects/{memento['project_id']}/MEMENTO.json"
    with open(os.path.expanduser(path), "w") as f:
        json.dump(memento, f, indent=2)

    return memento
```

### Pas 3: Load memento at session start

```python
def load_memento(project_id):
    """Load and format memento for AI context injection."""
    path = f"~/.nexus/projects/{project_id}/MEMENTO.json"
    with open(os.path.expanduser(path)) as f:
        m = json.load(f)

    prompt = f"""Continue project: {m['project_title']}
Objective: {m['objective']}
Current stage: {m['current_stage']} ({m['progress_pct']}% complete)
Sessions so far: {m['session_count']}

Key context: {m['key_context']}

Decisions made:
{chr(10).join(f"- {d['decision']} (reason: {d['rationale']})" for d in m['decisions_made'][-5:])}

Next steps:
{chr(10).join(f"- {s}" for s in m['next_steps'])}

Blockers: {', '.join(m['blockers']) if m['blockers'] else 'None'}

First action: {m['next_steps'][0] if m['next_steps'] else 'TBD'}"""

    return prompt, m
```

### Pas 4: Verify AI context comprehension
After loading memento, ask AI to paraphrase: objective, current stage, and first next step. If paraphrase is incorrect, correct before proceeding. Never trust that the AI retained previous sessions.

### Pas 5: Update memento during session

```python
def checkpoint_memento(memento, stage_update=None, new_decision=None, blocker_resolved=None):
    """Call every 30 min or at each milestone during active session."""
    if stage_update:
        memento["current_stage"] = stage_update["stage"]
        memento["progress_pct"] = stage_update["pct"]
    if new_decision:
        memento["decisions_made"].append(new_decision)
    if blocker_resolved:
        memento["blockers"] = [b for b in memento["blockers"] if b != blocker_resolved]
    memento["last_updated"] = datetime.now().isoformat()
    return memento
```

Minimum update frequency: every 30 minutes or at every milestone.

### Pas 6: Archive mementos at project completion

```python
def archive_project(memento):
    """Archive memento chain and extract learnings."""
    archive = {
        "project": memento["project_title"],
        "total_sessions": memento["session_count"],
        "duration_days": None,  # calculate from first/last timestamps
        "total_decisions": len(memento["decisions_made"]),
        "key_learnings": [],  # extract from decisions
        "final_output": memento["current_stage"],
        "context_loss_incidents": 0,  # track manually
    }
    return archive
```

### Pas 7: Version mementos for team handoffs

```python
def handoff_memento(memento, new_owner):
    """Prepare memento for team handoff with explicit context."""
    handoff = memento.copy()
    handoff["handoff_to"] = new_owner
    handoff["handoff_notes"] = f"""
    Critical context for {new_owner}:
    1. Project is at {memento['progress_pct']}% — {memento['current_stage']}
    2. Key decisions that constrain future work: {len(memento['decisions_made'])}
    3. Immediate next step: {memento['next_steps'][0] if memento['next_steps'] else 'TBD'}
    4. Known blockers: {len(memento['blockers'])}
    """
    return handoff
```

---

## 3. Cortex Logging

```json
{
  "text": "Mementos used for [project]. Sessions: [X]. Context loss incidents: [0/N]. Decisions tracked: [N]. Final completion: [%].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-065",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["mementos", "long-sessions", "context-management", "state-persistence"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
Any AI session >1 hour or continuing across days

### HOW (violation detection)
- Session resumed without loading memento → violation
- Memento created without required template fields → violation
- >30 min without checkpoint update in active session → soft violation
- No archive at project completion → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis

### VERIFY
- [ ] Memento template defined at project start?
- [ ] Memento saved at every session end?
- [ ] Loaded at every session start with AI verification?
- [ ] Updated every 30 min during session?
- [ ] Archived at completion?

**VK-uri:**
1. `✅ [PROC] C-065 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Use Mementos for Long AI Work Sessions" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Justification |
|-----------|-------|---------------|
| Memento generation | Sonnet | Structured summarization |
| Context verification | Sonnet | Quick comprehension check |
| Handoff preparation | Opus | Comprehensive context transfer |
| Audit | Opus | Quality assurance |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| JSON memento file | State persistence |
| Project folder structure | Storage location |
| Cortex | Logging and archival |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Mementos created per session | Min. 1 (at end) |
| Context loss incidents | 0 |
| Checkpoint frequency | Every 30 min minimum |
| Handoff success rate | 100% (new owner productive in <10 min) |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Initial version |
| **2.0** | **2026-03-20** | Opus v2: 7 steps with Python code, memento CRUD functions, handoff protocol, versioning, checkpoint automation, when to use/NOT use |
