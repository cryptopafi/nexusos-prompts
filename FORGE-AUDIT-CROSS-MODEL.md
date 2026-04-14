---
type: procedure
created: 2026-03-22
status: active
slug: forge-audit-cross-model
tags: [procedure, nexus]
---

> **⚠️ DEPRECATED (2026-03-22)** — This procedure has been absorbed into **AUDIT-PRO** (`~/.nexus/audit/AUDIT-PRO.md`). Do not use. Use `/audit` command instead.

# FORGE-AUDIT-CROSS-MODEL v1.2 — Triangle Audit Protocol

**Status**: DEPRECATED | Absorbed into AUDIT-PRO v1.1 Step 7 Cross-Model Dispatch (`~/.nexus/audit/AUDIT-PRO.md`) on 2026-03-21. Use `/audit` instead.
**Creat**: 2026-03-11 | **Updated**: 2026-03-11
**Versiune**: 1.2
**Regula asociata**: QUAL-H-004 (Verify-Before-Assert)
**Scope**: Standardized protocol for dispatching FORGE-AUDIT methodology to external models (GPT-5.4, Gemini 3.1 Pro Preview) and merging findings with Opus re-audit.
**Parent**: FORGE-AUDIT v1.5

> **Override notice**: This procedure is an authorized extension of FORGE-AUDIT v1.5. The parent's "Opus EXCLUSIVE" rule (line 532) applies to the *authoritative verdict*. External models (GPT-5.4, Gemini) serve as **discovery layers** that surface findings Opus might miss. Opus remains the sole authority for final scoring (Pas 5).

---

## 1. Problema

### Ce nu functioneaza fara un protocol cross-model standardizat

1. **Audit disparity** — GPT-5.4 and Gemini apply their own heuristics, not FORGE NPLF. Findings are incomparable.
2. **Missing categories** — Each model has blind spots. GPT-5.4 catches injection vectors, Gemini catches logic gaps, Opus catches architectural issues. Without all three, coverage is incomplete.
3. **Manual translation** — Genie manually maps ad-hoc findings to FORGE categories, introducing interpretation errors.
4. **No rubric enforcement** — External models produce free-form reviews unless given explicit scoring rubric.
5. **Inconsistent severity** — What GPT calls "HIGH" may be Gemini's "MEDIUM" without a shared scale.

### Evidence (2026-03-11 session)

- GPT-5.4 found 3 HIGHs that Gemini missed (XML injection, log leaking)
- Gemini 3.1 Pro found 2 HIGHs that GPT missed (DATE_CLAIM_PATTERN gate, verification logic)
- Opus found 2 HIGHs that both missed (topic detection false positives, maintenance session leak)
- Combined: 7 unique HIGH findings vs max 3 from any single model

---

## 2. Procedura

### Pas 1: CLASSIFY TIER + SELECT DISPATCH MODE

**Step 1a — Tier classification (inherited from parent FORGE-AUDIT v1.5 Pas 1):**

Classify the audit tier using the PARENT procedure's rules exactly:
- **LIGHT**: Fix simplu, config change, quick review → D1-D3
- **STANDARD**: Procedure noua, feature, Codex delivery → D1-D6
- **DEEP**: Architecture, production audit, security → D1-D8 + Pas 5 Research

The tier determines WHICH dimensions are scored. This step is mandatory and follows parent rules without modification.

**Step 1b — Dispatch mode (NEW — orthogonal to tier):**

The dispatch mode determines HOW MANY models participate. It is independent of tier:

| Condition | Mode | Rationale |
|-----------|------|-----------|
| <=200 lines, single file, config/fix | **Single (Opus only)** | Overkill to dispatch 3 models |
| 200-1000 lines, 2-5 files, feature/delivery | **Dual (Opus + 1 external)** | Good coverage, reasonable cost |
| >1000 lines, >5 files, architecture/security | **Triangle (Opus + GPT-5.4 + Gemini)** | Maximum coverage for high-stakes |
| Production deployment, financial logic, security | **Triangle (mandatory)** | Risk too high for single-model |

**Important**: A DEEP tier audit can run in any dispatch mode (single/dual/triangle). A LIGHT tier audit on many files can be triangle. Tier = dimensions. Mode = models.

**Step 1c — DSE detection (inherited from parent):**

Check if the subject qualifies for DSE-RESEARCH or DSE-WORKFLOW per parent Pas 1 rules. If DSE applies, include DSE dimensions (R1-R4 / W1-W4) in the rubric block for ALL dispatched models.

Emit VK: `🔍 [AUDIT-XMODEL] tier: {LIGHT|STANDARD|DEEP} | dse: {DSE-NAME|none} | mode: {single|dual|triangle} | files: {N} | lines: {N}`

### Pas 2: PREPARE NPLF RUBRIC BLOCK

This block is included verbatim in ALL external model prompts to enforce FORGE-AUDIT methodology:

```
## AUDIT METHODOLOGY: FORGE-AUDIT NPLF

Score each dimension on the NPLF scale (ISO 15504):
- F (Fully achieved, 86-100%): Complete, no gaps
- L (Largely achieved, 51-85%): Functional with minor gaps
- P (Partially achieved, 16-50%): Significant gaps
- N (Not achieved, 0-15%): Absent or fundamentally broken

### Dimensions to evaluate:

**Select dimensions based on audit tier (inherited from parent FORGE-AUDIT v1.5):**
- LIGHT (D1-D3): Quick review, config changes, minor fixes
- STANDARD (D1-D6): Features, deliveries, procedures
- DEEP (D1-D8): Architecture, security, production systems

| # | Dimension | Key question (verbatim from parent FORGE-AUDIT v1.5) | Tier |
|---|-----------|-------------------------------------------------------|------|
| D1 | Completeness | Lipsesc pasi, sectiuni, sau cazuri? | LIGHT+ |
| D2 | Accuracy | E tehnic corect? Informatia e valida? | LIGHT+ |
| D3 | Verifiability | Poate fi verificat obiectiv ca functioneaza? | LIGHT+ |
| D4 | Clarity | E inteles de utilizatorul tinta? Limbaj simplu? | STANDARD+ |
| D5 | Consistency | Intra in conflict cu alte documente/proceduri/reguli? | STANDARD+ |
| D6 | Safety | Sunt definite actiunile interzise? Limitele sunt clare? | STANDARD+ |
| D7 | Adequacy | E potrivit pentru scopul declarat? Rezolva problema reala? | DEEP only |
| D8 | Maintainability | Poate fi actualizat usor? Version control? Ownership clar? | DEEP only |

**Rules for rubric generation:**
- Include ONLY the dimensions matching the current tier (D1-D3 for LIGHT, D1-D6 for STANDARD, D1-D8 for DEEP)
- For code audits, add domain-specific probes beneath each dimension (e.g., under D6 Safety: "injection vectors, data leaks, missing validation at system boundaries") — but keep the canonical dimension name and key question unchanged
- DSE extensions (R1-R4, W1-W4) from parent procedure apply when relevant — include them in the rubric if the audit subject qualifies per Pas 1c

### Scoring rules:
- Score each dimension F/L/P/N with a 1-line justification
- Convert: F=4, L=3, P=2, N=1
- Total = arithmetic mean of all dimensions, /4.0
- Verdict: >=3.5 PASS | 2.5-3.4 CONDITIONAL | <2.5 FAIL
- Any single N dimension → max CONDITIONAL regardless of total

### Findings format:
Classify each finding by severity:
- CRITICAL: Data loss, security breach, system crash
- HIGH: Logic error, missing validation, injection vector
- MEDIUM: Inconsistency, suboptimal pattern, missing edge case
- LOW: Style, naming, minor improvement

For each finding, provide:
- Severity: CRITICAL/HIGH/MEDIUM/LOW
- File: exact path
- Line: line number(s)
- Description: what is wrong
- Fix: concrete fix (code or description)
```

### Pas 2b: SANITIZE PAYLOAD

Before dispatching ANY code to external models, sanitize the payload:

1. **Exclude sensitive files**: Never include `.env`, `credentials.json`, `tokens/`, `*.pem`, `*.key` in audit scope
2. **Regex strip**: Scan included files for patterns matching `API_KEY|SECRET|TOKEN|PASSWORD|PRIVATE_KEY` — if found in actual values (not variable names), redact the value: `API_KEY=sk-abc123...` → `API_KEY=[REDACTED]`
3. **Path sanitization**: Replace absolute home paths with `~/` to avoid leaking machine-specific info
4. **Verification**: Log excluded files count — `Sanitized: {N} files excluded, {N} values redacted`

If a file in scope fails sanitization (contains unredactable secrets), exclude it from external dispatch and note it for Opus-only review in Pas 5.

### Pas 3A: DISPATCH TO GPT-5.4 (via Codex)

Append to `~/.codex/genie-to-codex.md`:

```markdown
## Task m4-{NNN}: FORGE-AUDIT Cross-Model — {target}

**Status**: PENDING
**Prioritate**: HIGH
**Model**: gpt-5.4
**Estimare**: {15-45 min based on scope}

### Prompt pentru Codex

**Goal**: Complete FORGE-AUDIT with NPLF scoring on {target}, producing standardized findings.

**Context**:
{Architecture summary, what the code does, recent changes}

{NPLF RUBRIC BLOCK from Pas 2 — include verbatim}

**Files to audit**:
{List every file with full path — Codex reads them directly}

**Steps**:
1. Read all files in scope
   - Checkpoint: all {N} files loaded
2. Score each NPLF dimension (D1-D6) per the rubric above
   - Checkpoint: 6 scores assigned with justification
3. Identify all findings by severity (CRITICAL/HIGH/MEDIUM/LOW)
   - Checkpoint: each finding has file:line reference
4. Calculate combined score and verdict
   - Checkpoint: score /4.0 matches dimension scores

**Output** — scrie in `~/.codex/codex-to-genie.md`:
```
## m4-{NNN}: FORGE-AUDIT Cross-Model {target}

### NPLF Score: {X}/4.0 — {PASS/CONDITIONAL/FAIL}
| Dimension | Score | Justification |
|-----------|-------|---------------|
| D1 Completeness | {F/L/P/N} | {1 line} |
| D2 Accuracy | {F/L/P/N} | {1 line} |
| D3 Verifiability | {F/L/P/N} | {1 line} |
| D4 Clarity | {F/L/P/N} | {1 line} |
| D5 Consistency | {F/L/P/N} | {1 line} |
| D6 Safety | {F/L/P/N} | {1 line} |

### Findings
#### CRITICAL
- {finding with file:line + fix}
#### HIGH
- {finding with file:line + fix}
#### MEDIUM
- {finding with file:line + fix}
#### LOW
- {finding with file:line + fix}
```

**Success Criteria**:
- [ ] All {N} files reviewed
- [ ] 6 NPLF dimensions scored with justification
- [ ] All findings have file:line references
- [ ] Verdict matches scoring rules

**Constraints**:
- Do NOT modify any files — read-only audit
- Do NOT run scripts — static analysis only
- Score MUST use the NPLF scale exactly as defined above
- macOS bash 3.2 compatibility checks on shell scripts
```

### Pas 3B: DISPATCH TO GEMINI 3.1 PRO PREVIEW (via CLI)

Run via `gemini` CLI with the NPLF rubric prepended:

```bash
cat <<'PROMPT' | gemini -m gemini-3.1-pro-preview
You are a senior code auditor performing a FORGE-AUDIT.

{NPLF RUBRIC BLOCK from Pas 2 — include verbatim}

## Target files:

{For each file, include the FULL content inline:}
### File: {path} ({lines} lines)
```{lang}
{file content}
```

## Task:
1. Score all NPLF dimensions for the selected tier (D1-D3 for LIGHT, D1-D6 for STANDARD, D1-D8 for DEEP)
2. List ALL findings by severity with file:line references
3. Calculate total score /4.0 and verdict
4. Focus especially on: injection vectors, logic errors, race conditions, missing error handling

Output your response in the exact NPLF format specified above.
PROMPT
```

**Practical notes**:
- Gemini CLI has ~200K context — can handle up to ~3000 lines of code inline
- For larger scopes, split into multiple calls and merge findings
- Model ID: `gemini-3.1-pro-preview` (NOT `gemini-3.1-pro`)
- Cost: $0.00 via Google One CLI OAuth

### Pas 4: MERGE FINDINGS

After all external audits return, merge into a consolidated finding set:

#### 4a. Deduplication
- Same file + same line + same issue type = duplicate → keep the more detailed description
- Same file + different line + same pattern = keep both (pattern may repeat)
- Different file + same issue type = keep both (systemic issue → flag as pattern)

#### 4b. Severity reconciliation
When models disagree on severity (GPT-5.4 vs Gemini — Opus acts in Pas 5, not here):

| GPT-5.4 | Gemini | Final |
|---------|--------|-------|
| CRITICAL | CRITICAL | CRITICAL |
| CRITICAL | HIGH | CRITICAL |
| CRITICAL | MEDIUM | CRITICAL |
| HIGH | HIGH | HIGH |
| HIGH | MEDIUM | HIGH (conservative) |
| MEDIUM | HIGH | HIGH (conservative) |
| HIGH | — | HIGH (single source, trust) |
| — | HIGH | HIGH (single source, trust) |
| MEDIUM | MEDIUM | MEDIUM |
| MEDIUM | LOW | MEDIUM |
| MEDIUM | — | MEDIUM |
| — | MEDIUM | MEDIUM |
| LOW | LOW | LOW |
| LOW | — | LOW |
| — | LOW | LOW |

Rule: **always take the higher severity** when models disagree.

#### 4b-fallback. Partial model failure
If one external model fails to return (timeout, error, empty output):
- Proceed with available results — do NOT block on missing model
- Note in report: `Model {X} failed: {reason}. Findings from {Y} only.`
- Opus re-audit (Pas 5) compensates by providing the missing perspective
- If BOTH external models fail, fall back to single-model Opus audit (standard FORGE-AUDIT)

#### 4c. Consolidated findings table

```
## Cross-Model Findings Summary

| # | Finding | Severity | File:Line | Found by | Status |
|---|---------|----------|-----------|----------|--------|
| 1 | {desc} | CRITICAL | {path:N} | GPT+Gemini | OPEN |
| 2 | {desc} | HIGH | {path:N} | Gemini only | OPEN |
| 3 | {desc} | HIGH | {path:N} | GPT only | OPEN |
| 4 | {desc} | MEDIUM | {path:N} | Both | OPEN |

Unique findings: GPT-only: {N} | Gemini-only: {N} | Both: {N} | Opus-only: {N}
```

### Pas 5: OPUS RE-AUDIT (authoritative)

After applying fixes from merged findings, Opus executes the **full parent FORGE-AUDIT** for the classified tier:

1. Read all files in their FINAL state (post-fix)
2. Verify each fix was correctly applied (checklist)
3. **Execute parent Pas 1-6** for the classified tier:
   - LIGHT: Pas 1→2→6
   - STANDARD: Pas 1→2→3→4→6
   - DEEP: Pas 1→2→3→4→**5 (Research Validation)**→6
4. Score ALL dimensions for the tier (D1-D3/D6/D8) + DSE if applicable
5. Check for regressions introduced by fixes
6. Produce final verdict per parent scoring rules

**DEEP tier gate**: If tier=DEEP, Opus MUST execute parent Pas 5 (Research Validation) — Cortex search + external sources. External models do NOT satisfy this gate. VK required: `🔍 [AUDIT-RESEARCH]` or explicit `⚠️ skip: {reason}`.

This is the authoritative score — external models inform, Opus decides.

### Pas 6: FINAL REPORT

```
FORGE-AUDIT CROSS-MODEL | Target: {target}
Mode: {single|dual|triangle} | Date: YYYY-MM-DD

## Audit Sources
| Model | Score | Verdict | Unique Findings |
|-------|-------|---------|-----------------|
| GPT-5.4 | {X}/4.0 | {V} | {N} |
| Gemini 3.1 Pro | {X}/4.0 | {V} | {N} |
| Opus (final) | {X}/4.0 | {V} | {N} |

## Final NPLF Score: {X}/4.0 — {PASS/CONDITIONAL/FAIL}
(Opus re-audit score is authoritative)

## All Findings ({total})
### Fixed this session ({N})
| # | Finding | Severity | Source | Fix applied |
|---|---------|----------|--------|-------------|

### Remaining by design ({N})
| # | Finding | Severity | Rationale |
|---|---------|----------|-----------|

### Deferred ({N})
| # | Finding | Severity | Deferred to |
|---|---------|----------|-------------|

## Cross-Model Coverage Analysis
- GPT-5.4 unique: {N} findings ({list categories})
- Gemini unique: {N} findings ({list categories})
- Opus unique: {N} findings ({list categories})
- Overlap: {N} findings found by 2+ models
- Total unique: {N}
- Coverage improvement vs single-model: +{X}%
```

Emit VK: `✅ [AUDIT-XMODEL] target: "{target}" | mode: {mode} | gpt: {X}/4.0 | gemini: {X}/4.0 | opus: {X}/4.0 | combined-findings: {N} | verdict: {VERDICT}`

---

## 3. Cortex Logging

```json
{
  "text": "CROSS-MODEL AUDIT: {target} | mode: {mode} | models: {list} | opus-final: {X}/4.0 | verdict: {VERDICT} | total-findings: {N} | unique-per-model: GPT={N} Gemini={N} Opus={N}",
  "collection": "procedures",
  "metadata": {
    "type": "audit-report",
    "procedure": "FORGE-AUDIT-CROSS-MODEL",
    "rule_id": "QUAL-H-004",
    "audit_mode": "{single|dual|triangle}",
    "models_used": ["{list}"],
    "scores": {"gpt": "{X}", "gemini": "{X}", "opus": "{X}"},
    "total_findings": "{N}",
    "coverage_improvement": "{X}%",
    "forge_bypass": true,
    "forge_bypass_reason": "audit-report",
    "tags": ["audit", "cross-model", "forge-audit", "triangle"]
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- WISH Step H — any scope qualifying for dual/triangle mode
- Manual — Pafi requests cross-model audit
- Automatic — files >1000 lines or >5 files triggers triangle suggestion

### WHEN
- At every qualifying Codex delivery (>1000 lines)
- At architecture/security reviews (mandatory triangle)
- At explicit cross-model audit request

### HOW (violation detection)
- Triangle mode without all 3 models dispatched → violation
- External model prompt without NPLF rubric block → violation
- Findings merged without severity reconciliation → violation
- Final verdict without Opus re-audit on post-fix state → violation
- Gemini invoked with wrong model ID (not `gemini-3.1-pro-preview`) → violation

### CONNECT
- **FORGE-AUDIT v1.5** — parent procedure, all NPLF rules inherited
- **CODEX-BRIEF-PREWRITE-CHECKLIST** — Codex dispatch format
- **Gemini Integration Proposal** — model routing for gemini-3.1-pro-preview

### VERIFY
- [ ] Mode correctly selected (single/dual/triangle)?
- [ ] NPLF rubric included verbatim in all external prompts?
- [ ] All dispatched models returned findings?
- [ ] Findings deduplicated and severity reconciled?
- [ ] Opus re-audit performed on FINAL state?
- [ ] Coverage improvement calculated?
- [ ] Cortex saved?

### Known Failure Modes

| # | Anti-pattern | Fix |
|---|-------------|-----|
| FM-1 | External model ignores NPLF rubric, produces free-form review | Include rubric as first section of prompt, explicitly say "use this format exactly" |
| FM-2 | Gemini CLI model ID wrong (404 error) | Always use `gemini-3.1-pro-preview`, never `gemini-3.1-pro` |
| FM-3 | GPT-5.4 delivery too large to read in session | Limit Codex audit scope to <2000 lines per brief |
| FM-4 | Opus re-audit on pre-fix state instead of post-fix | Always read files AFTER fixes applied, verify with git diff |

---

## 5. Dependente

| Component | Role | Path |
|-----------|------|------|
| FORGE-AUDIT v1.5 | Parent procedure, NPLF definitions | `~/.nexus/procedures/FORGE-AUDIT.md` |
| Codex daemon | GPT-5.4 dispatch | `~/.codex/genie-to-codex.md` |
| Gemini CLI | Gemini 3.1 Pro dispatch | `gemini` binary |
| Cortex API | Audit report storage | `localhost:6400/api/store` |

---

## 6. Metrics

| Metric | Measures | Target |
|--------|----------|--------|
| Coverage improvement | `(total_unique_findings - max_single_model_findings) / max_single_model_findings * 100`. Baseline = model with most findings. Deduplicated findings only. Skip metric if <2 models returned valid output. | >= 30% (provisional — recalibrate after 5 triangle audits) |
| Cross-model agreement | % findings found by 2+ models | >= 40% |
| Unique-per-model ratio | Distribution of unique findings | No model < 15% |
| Time overhead | Triangle vs single-model time | <= 2x single |
| False positive rate | Findings dismissed as "by design" | <= 20% |
