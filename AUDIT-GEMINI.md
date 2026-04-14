---
type: procedure
created: 2026-03-22
status: active
slug: audit-gemini
tags: [procedure, nexus]
---

> **⚠️ DEPRECATED (2026-03-22)** — This procedure has been absorbed into **AUDIT-PRO** (`~/.nexus/audit/AUDIT-PRO.md`). Do not use. Use `/audit` command instead.

# AUDIT-GEMINI v1.0 — FORGE-AUDIT via Gemini CLI

**Status**: DEPRECATED | Absorbed into AUDIT-PRO v1.1 Step 4 + gemini-audit-dispatch.sh (`~/.nexus/audit/AUDIT-PRO.md`) on 2026-03-21. Use `/audit` instead.
**Creat**: 2026-03-11
**Versiune**: 1.0
**Regulă asociată**: QUAL-H-004 (Verify-Before-Assert)
**Scope**: Standalone FORGE-AUDIT execution using Gemini CLI with NPLF scoring. Runs as a discovery layer (findings inform Opus re-audit) or as a fast independent audit when Opus capacity is constrained.
**Parent**: FORGE-AUDIT v1.5, FORGE-AUDIT-CROSS-MODEL v1.2

---

## 1. Problema

### Ce nu funcționează fără un audit Gemini standalone

1. **Opus bottleneck** — All FORGE-AUDITs require Opus subagent, creating a single-model bottleneck during batch audits or high-load sessions.
2. **No async audit** — Codex dispatches GPT-5.4 audits async, but no equivalent for Gemini. Gemini CLI runs locally, is free ($0.00), and returns in 30-90s.
3. **Cross-model gap** — FORGE-AUDIT-CROSS-MODEL exists but requires orchestrating 3 models manually. A standalone Gemini script enables fast "second opinion" without full triangle setup.
4. **Model blind spots** — Gemini catches different categories than Opus/GPT (logic gaps, verification holes, DATE patterns — evidenced in session 2026-03-11 where Gemini found 2 HIGHs both others missed).

### Situații acoperite

- Fast audit of Codex deliveries (before or parallel with Opus)
- Second opinion on CONDITIONAL Opus audits
- Batch audit when Opus context is full
- CI/pre-commit lightweight audit gate
- Discovery layer for FORGE-AUDIT-CROSS-MODEL triangle

---

## 2. Procedura

### Pas 1: RESOLVE TARGET

Script: `~/.nexus/scripts/audit-gemini.sh`

```bash
# Single file
audit-gemini.sh src/dispatch.ts

# Directory
audit-gemini.sh ./src/

# Codex delivery
audit-gemini.sh m4-448

# Multiple files
audit-gemini.sh file1.ts file2.ts file3.sh

# With options
audit-gemini.sh --tier DEEP --model gemini-2.5-pro src/relay.ts
```

Target resolution:
- **File path** → read directly
- **Directory** → find all .ts/.js/.sh/.py/.yaml/.json/.md (exclude node_modules, .git, dist)
- **m4-NNN** → parse `~/.codex/codex-to-genie.md` for delivery section, extract file paths
- **Multiple** → combine all

### Pas 2: SELECT TIER

| Tier | Dimensions | When |
|------|-----------|------|
| LIGHT | D1-D3 | Config change, quick fix, <50 lines |
| STANDARD | D1-D6 | Feature, Codex delivery, integration (default) |
| DEEP | D1-D8 | Architecture, security, production |

Default: STANDARD. Override with `--tier LIGHT|DEEP`.

### Pas 3: SELECT MODEL

**Priority chain**:
1. `--model X` (explicit override) → use X
2. `gemini-3.1-pro-preview` (primary) → try first
3. `gemini-2.5-pro` (fallback) → if primary fails (404, timeout, error)

Both models: $0.00 via Google One CLI OAuth. ~200K context window.

### Pas 4: BUILD NPLF PROMPT

The script automatically:
1. Reads all target files
2. Sanitizes content (redact API_KEY/SECRET/TOKEN/PASSWORD values, replace $HOME with ~)
3. Builds NPLF rubric block with tier-appropriate dimensions
4. Includes file content inline in prompt
5. Specifies exact output format

### Pas 5: DISPATCH + FALLBACK

```
gemini -m {model} -p "{prompt}" → output
```

If primary model fails → automatic fallback to gemini-2.5-pro.
If both fail → exit 1, log error.

### Pas 6: OUTPUT

Report saved to: `~/.nexus/workspace/output/forge/audit-gemini-{YYYYMMDD-HHMMSS}.md`

Contains:
- Header (date, model, tier, file count)
- NPLF dimension scores with justifications
- Findings by severity (CRITICAL/HIGH/MEDIUM/LOW) with file:line references
- File list audited

### Pas 7: INTEGRATION WITH FORGE-AUDIT

The Gemini audit report is a **discovery layer**, not a final verdict:

| Use case | Flow |
|----------|------|
| **Fast independent audit** | Run audit-gemini.sh → review report → apply fixes if clear |
| **Input to Opus re-audit** | Run audit-gemini.sh → feed findings to Opus subagent → Opus produces authoritative NPLF score |
| **Triangle audit** | audit-gemini.sh + Codex GPT-5.4 → merge findings (CROSS-MODEL Pas 4) → Opus re-audit (Pas 5) |
| **Batch pre-filter** | Run Gemini on N deliveries → only escalate CONDITIONAL/FAIL to Opus |

---

## 3. Cortex Logging

```json
{
  "text": "AUDIT-GEMINI: {target} | model: {model} | tier: {TIER} | score: {X}/4.0 | verdict: {VERDICT} | findings: {N}",
  "collection": "procedures",
  "metadata": {
    "type": "audit-report",
    "procedure": "AUDIT-GEMINI",
    "rule_id": "QUAL-H-004",
    "model": "{gemini-3.1-pro-preview|gemini-2.5-pro}",
    "audit_tier": "{LIGHT|STANDARD|DEEP}",
    "audit_subject": "{target}",
    "forge_bypass": true,
    "forge_bypass_reason": "audit-report",
    "tags": ["audit", "gemini", "forge-audit"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — as discovery layer before or parallel with Opus audit
- Manual — `/audit gemini {target}` or direct script invocation
- Automated — integrate in nightly-audit or CI pipeline

### WHEN
- At any Codex delivery (optional fast pre-screen)
- When Opus is busy (context full, batch mode)
- When FORGE-AUDIT-CROSS-MODEL triangle is requested (Pas 3B)
- When Pafi explicitly requests Gemini audit

### HOW (violation detection)
- Script invoked without tier selection → default STANDARD (not a violation)
- Gemini output treated as authoritative final verdict without Opus validation → violation (for HIGH/CRITICAL scope)
- Prompt without NPLF rubric → violation (script enforces automatically)
- Secrets not sanitized → violation (script auto-redacts)

### CONNECT
- **FORGE-AUDIT v1.5** — parent, NPLF definitions and tier rules
- **FORGE-AUDIT-CROSS-MODEL v1.2** — triangle protocol, Pas 3B uses this script
- **audit-gemini.sh** — `~/.nexus/scripts/audit-gemini.sh` (executable)
- **Gemini CLI** — `/opt/homebrew/bin/gemini` (@google/gemini-cli@0.30.0)

### VERIFY
- [ ] Target resolved to >0 files?
- [ ] Tier matches scope (not over/under audit)?
- [ ] Model responded (not empty/error)?
- [ ] NPLF scores present in output for all tier dimensions?
- [ ] Findings have file:line references?
- [ ] Report saved to output/forge/?
- [ ] Secrets sanitized (no API_KEY/TOKEN values in prompt)?

---

## 5. Dependențe

| Componentă | Rol | Path |
|-----------|-----|------|
| Gemini CLI | Model invocation | `/opt/homebrew/bin/gemini` |
| FORGE-AUDIT v1.5 | NPLF definitions, tier rules | `~/.nexus/procedures/FORGE-AUDIT.md` |
| FORGE-AUDIT-CROSS-MODEL v1.2 | Triangle integration | `~/.nexus/procedures/FORGE-AUDIT-CROSS-MODEL.md` |
| Codex delivery file | m4-NNN target resolution | `~/.codex/codex-to-genie.md` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Model success rate | % invocations returning valid NPLF output | >= 95% |
| Fallback trigger rate | % times primary fails, fallback used | <= 10% |
| Finding overlap with Opus | % Gemini findings confirmed by Opus | >= 60% |
| Unique findings | % findings only Gemini catches | >= 15% |
| Execution time | Prompt → report | <= 120s |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există: QUAL-H-004
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] Script executabil: `~/.nexus/scripts/audit-gemini.sh` (chmod +x, bash -n OK)
- [x] Modele testate: gemini-3.1-pro-preview OK, gemini-2.5-pro OK
- [x] Sanitizare secrets: regex strip API_KEY/SECRET/TOKEN/PASSWORD
- [x] Fallback chain: primary → fallback → exit 1
- [x] Output format: NPLF standard, compatible cu CROSS-MODEL merge
