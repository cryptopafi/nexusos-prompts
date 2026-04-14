---
type: procedure
created: 2026-03-17
status: active
slug: opus-insight
tags: [procedure, nexus]
---

# OPUS-INSIGHT — Research Quality Gate & Decision Node

**Status**: ACTIVE
**Version**: 1.0
**Created**: 2026-03-09
**Rule**: NEXUS-H-001 (every research pipeline run passes through Opus Insight)
**Script**: `~/.nexus/echelon/nexus-opus-insight.py`
**Trigger**: Automatic — called by `nexus-run.sh` after Faza 1-3 complete

---

## §1 PURPOSE

Opus Insight is the decision node in the Nexus Research pipeline. It evaluates collected evidence (SuperDelphi + Cortex + ECHELON) and decides:
- **DELIVER**: Evidence is sufficient — synthesize and send to user
- **DEEPEN**: Evidence has gaps — generate a refined sub-topic and re-run the pipeline

Without this gate, the pipeline is a one-shot linear flow. With it, the pipeline becomes an iterative feedback loop that self-improves.

---

## §2 INPUTS

| Input | Source | Required | Description |
|-------|--------|----------|-------------|
| `--topic` | nexus-run.sh | YES | Original search topic |
| `--superdelphi-file` | nexus-superdelphi-bridge.py output | YES | JSON with `info`, `epr_score`, `links`, `self_grade` |
| `--search-file` | nexus-critic-judge.py output | YES | Scored Cortex search results |
| `--mode` | nexus-run.sh | NO (default: manual) | `manual` = Opus model, `auto` = model routing by EPR |
| `--iteration` | nexus-extend.sh | NO (default: 0) | Current loop iteration (0-indexed) |
| `--cortex-url` | env/arg | NO | Cortex base URL |

---

## §3 DECISION LOGIC

### 3.1 Model Selection
| Mode | EPR Score | Model |
|------|-----------|-------|
| manual | any | Opus 4.6 |
| auto | ≥ 14 | Sonnet 4.6 |
| auto | < 14 | Gemini 2.5 Flash |

### 3.2 Decision Rules
```
IF confidence ≥ 0.65 → DELIVER
IF iteration ≥ 3     → FORCE DELIVER (max cycles reached)
IF confidence < 0.65 AND iteration < 3 → DEEPEN
```

### 3.3 DEEPEN Output
When deepening, Opus Insight generates:
- `new_topic`: A specific, narrower search query (max 80 chars)
- The new topic targets the highest-value gap identified in the evidence

### 3.4 DELIVER Output
When delivering, Opus Insight generates:
- `summary`: Actionable insight for the user (max 400 chars)
- `confidence`: Float 0.0-1.0 reflecting evidence quality
- `epr_score`: Integer 1-20 reflecting overall research quality

---

## §4 OUTPUTS

```json
{
  "ok": true,
  "decision": "deliver|deepen",
  "summary": "Actionable insight text (max 400 chars)",
  "new_topic": "Refined search query (max 80 chars, null if deliver)",
  "iteration": 0,
  "epr_score": 14,
  "model_used": "anthropic/claude-opus-4-6",
  "confidence": 0.85
}
```

---

## §5 INTEGRATION POINTS

### 5.1 Upstream (feeds into Opus Insight)
- `nexus-superdelphi-bridge.py` → research summary + EPR + links
- `nexus-critic-judge.py` → scored & filtered Cortex search results
- `nexus-nlm-bridge.py` → NotebookLM pre-analysis (optional)

### 5.2 Downstream (consumes Opus Insight output)
- `nexus-extend.sh` → reads `decision` field, loops if `deepen`
- `nexus-synthesize.py` → generates report when `deliver`
- `nexus-run.sh` → final JSON assembly, VPS publish, Notion/Obsidian sync

### 5.3 Side Effects
- Writes insight vault note to `~/.nexus/research/insights/insight-{timestamp}-{slug}.md`
- Deduplication: skips write if existing insight for same topic within 5 minutes

---

## §6 ERROR HANDLING

| Failure | Recovery |
|---------|----------|
| OpenRouter API down | Fallback to Gemini CLI |
| Both APIs down | Return DELIVER with confidence=0.5, summary="Research complet" |
| Malformed SuperDelphi JSON | Use empty info, EPR=0 |
| Malformed search JSON | Use empty results array |
| Iteration overflow | Force DELIVER at iteration ≥ 3 |

---

## §7 ENFORCEMENT

### VERIFY
- [ ] Every nexus-run.sh execution calls nexus-opus-insight.py
- [ ] Decision field is always "deliver" or "deepen" (never empty/null)
- [ ] new_topic is non-empty when decision="deepen"
- [ ] EPR score is integer 1-20
- [ ] Confidence is float 0.0-1.0
- [ ] Vault note is written on every successful run

### CONNECT
- NEXUS-H-001 → this procedure
- nexus-run.sh line ~545 → calls this script
- nexus-extend.sh → reads decision output
- Cortex collection: `nexus-research` → stores insight metadata
