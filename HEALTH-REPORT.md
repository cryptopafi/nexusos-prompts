---
type: procedure
created: 2026-03-24
status: DEPRECATED
deprecated_at: 2026-03-28
deprecated_reason: "Replaced by Health Pro plugin (~/.claude/plugins/health/) and Health V2 agent (~/.nexus/v2/agents/health/)"
successor: "~/.claude/plugins/health/agents/health.md"
slug: health-report
tags: [procedure, nexus, DEPRECATED]
---

# [DEPRECATED] HEALTH-REPORT — Standard Operating Procedure

> **DEPRECATED 2026-03-28**: This procedure has been absorbed into the Health Pro plugin orchestrator SOUL at `~/.claude/plugins/health/agents/health.md`. Use `/health-report` command instead. This file is kept for reference only.

**Status**: DEPRECATED
**Created**: 2026-03-24
**Version**: 2.1
**Associated Rule**: HEALTH-R-001 (Every health report MUST follow the 4-source cross-reference pipeline + reporter publish)
**Scope**: Blood panel analysis, supplement/peptide protocols, and any health report delivered to Pafi.
**Owner**: Health Agent (Opus 4.6)

---

## Constants

| Variable | Value | Notes |
|----------|-------|-------|
| `HEALTH_VPS_HOST` | `89.116.229.189` | VPS for report hosting |
| `HEALTH_VPS_USER` | `pafi` | SCP user |
| `HEALTH_VPS_PATH` | `/var/www/research/` | Deploy directory |
| `HEALTH_VPS_URL` | `http://${HEALTH_VPS_HOST}/nexus/` | Public URL prefix |
| `NLM_SERVICE_PORT` | `18790` | NotebookLM service port |
| `CORTEX_PORT` | `6400` | Cortex API port |

Update these constants when infrastructure changes. All procedure steps reference these variables, not hardcoded values.

---

## 1. Problem

Health reports were generated using only a single knowledge source (typically Cortex), missing cross-referencing with NotebookLM notebooks, web research, and PromptForge optimization. Reports were not published to shareable URLs via the Reporter skill, resulting in inconsistent formatting and no permanent link.

Types of situations covered:
- Blood panel / lab result analysis
- Peptide protocol design
- Supplement stack recommendations
- Longevity health optimization reports
- Any health-related report delivered to Pafi

---

## 1b. Safety (NEVER-list)

This procedure generates health recommendations including peptide dosages. The following constraints are absolute:

- **NEVER** present recommendations as medical prescriptions or replacements for professional medical advice
- **NEVER** recommend dosages without citing the specific source (video, study, document)
- **NEVER** omit a disclaimer that this is informational content for discussion with a qualified healthcare provider
- **NEVER** recommend controlled substances without explicit source citation and legality context
- **NEVER** ignore contraindications between recommended peptides and the patient's current markers
- **ALWAYS** flag markers suggesting conditions requiring urgent medical attention (e.g., critically low/high values)
- **ALWAYS** include at the top of every report: "This report is for informational purposes only. Consult a qualified healthcare provider before starting any protocol."

---

## 2. Procedure

### Step 1: Extract & Parse Input Data
Parse the blood test PDF or lab results. Extract all markers, values, units, and reference ranges into structured data. Flag markers outside optimal range (not just reference range).

### Step 2: Cortex Search (Source 1)
```
cortex_search("blood markers [specific markers] peptide protocol optimization")
```
- Min 3 queries with different angles (markers, protocols, interactions)
- Search for: specific markers, conditions, peptides, supplements, longevity protocols

### Step 3: NotebookLM Query — ALL Notebooks (Source 2)
```python
from nlm_client import nlm_query, nlm_list, nlm_default_notebook

# Query ALL health-related notebooks, not just default
notebooks = nlm_list()
health_notebooks = [nb for nb in notebooks if nb['topic'] in ['health', 'longevity', 'peptides', 'nutrition']]

# If list fails or empty, fall back to default
if not health_notebooks:
    health_notebooks = [{"id": nlm_default_notebook()}]

for nb in health_notebooks:
    answer = nlm_query(nb['id'], PROMPTFORGED_QUERY)
    # Aggregate answers across all notebooks
```
- Query ALL health-related notebooks (Bachmeyer, David P, Asprey, longevity, future ones)
- As new medical/longevity/nutrition notebooks are added, they auto-join the pipeline
- **MANDATORY**: PromptForge (`/opt`) the query BEFORE sending — use Template A.1

### Step 4: NotebookLM Deep Protocol (Source 3)
```python
detailed = nlm_query(notebook_id, PROMPTFORGED_DEEP_QUERY)
```
- Second NLM query: ask for detailed protocol recommendations
- Include specific markers + ranges in the query
- **MANDATORY**: PromptForge this query too — use Template A.2

### Step 5: Web Research (Source 4)
- Use `tavily_research` (model: "pro") or `brave_web_search` as fallback
- Focus on: recent clinical studies (2024-2026), FDA/EMA updates, interaction warnings, dosage guidelines

### Step 6: Cross-Reference Synthesis
Cross-reference all 4 sources:
- Agreements (high confidence) → present as primary recommendations
- Conflicts → flag explicitly, present both sides with source attribution
- Unique findings (only one source) → present with source attribution + confidence note
- Gaps (no source covers) → flag as "needs further research"

### Step 7: Structure Report
Every blood panel report MUST include these sections:

1. **Patient Overview** — name, date, lab, age context
2. **Results Dashboard** — all markers with values, reference ranges, status (optimal/borderline/critical)
3. **Deep Analysis** — each abnormal marker explained (what it means, root causes, downstream effects)
4. **Cross-Reference Insights** — what Cortex + NLM + web research say about this specific pattern
5. **Peptide Protocol** — specific peptides addressing each deficiency/issue:
   - Peptide name, mechanism, dosage, timing, cycle length
   - Synergies between peptides
   - Contraindications with current markers
6. **Fat Loss Peptides Assessment** — always evaluate applicability of:
   - Retatrutide (triple agonist: GLP-1/GIP/glucagon)
   - 5-amino-1-MQ (NNMT inhibitor, fat cell metabolism)
   - AOD-9604 (hGH fragment 176-191, lipolysis)
   - NAD+ (cellular energy, mitochondrial function)
   - Tesamorelin (visceral fat reduction)
   - Include only if markers support usage (no contraindications)
7. **Supplement Stack** — non-peptide supplements for remaining gaps
8. **Monitoring Plan** — which markers to retest, when, expected improvements
9. **Sources & Confidence** — attribution to Cortex/NLM/web for each recommendation

### Step 8: Publish via Reporter Skill
Invoke the Delphi Pro Reporter skill:
```json
{
  "task": "publish",
  "report_markdown": "<full markdown report from Step 7>",
  "metadata": {
    "topic": "Blood Panel Analysis — [Patient Name]",
    "depth": "D3",
    "source_count": {"cortex": N, "notebooklm": N, "web": N},
    "patient": "[name]",
    "date": "[analysis date]"
  },
  "output_format": "html",
  "tier": 2,
  "deploy_vps": true
}
```
Reporter handles SCP to `${HEALTH_VPS_USER}@${HEALTH_VPS_HOST}:${HEALTH_VPS_PATH}{slug}-{timestamp}.html`
Fallback: if VPS unreachable → save to `~/.nexus/reports/health/` and deliver local path.

### Step 9: Deliver Link
Send Pafi the VPS URL: `${HEALTH_VPS_URL}{slug}-{timestamp}.html`

### Test Cases

1. **Normal flow**: Blood panel PDF with 5 abnormal markers → report with all 9 sections, 4 sources cross-referenced, published to VPS, link delivered
2. **Edge case**: NLM service down → skip Sources 2+3, proceed with Cortex + web only, flag "NLM unavailable" in report
3. **Edge case**: Tavily rate limited → fall back to Brave search, flag "web research limited" in report
4. **Failure case**: No abnormal markers found → produce summary report confirming all markers optimal, skip peptide protocol section
5. **Edge case**: VPS unreachable → save HTML to `~/.nexus/reports/health/`, deliver local path, flag "VPS unavailable" in report
6. **Edge case**: PromptForge `/opt` unavailable → proceed with raw queries built from Templates A.1/A.2 directly (templates are already structured), flag "queries not PromptForged" in Sources & Confidence section

---

## 3. Cortex Logging

After every health report execution, save to Cortex:

```json
{
  "text": "Health Report: [Patient Name] — [date] — [N abnormal markers] — [N peptides recommended] — published to [VPS URL or local path]",
  "collection": "procedures",
  "metadata": {
    "type": "health-report-execution",
    "procedure": "HEALTH-REPORT",
    "rule_id": "HEALTH-R-001",
    "has_enforcement_loop": true,
    "forge_version": "1.9",
    "patient": "[name]",
    "markers_flagged": ["marker1", "marker2"],
    "sources_used": {"cortex": true, "nlm": true, "web": true},
    "nlm_notebooks_queried": ["notebook_id_1", "notebook_id_2"],
    "prompts_optimized": true,
    "published_url": "[VPS URL]",
    "reporter_tier": 2,
    "tags": ["health", "blood-panel", "peptides", "report"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Health agent report generation flow
- Any task matching: "analyze blood", "blood panel", "health report", "peptide protocol", "supplement stack"

### WHEN
- Every time a health report is requested by Pafi
- Every time the Health agent produces any report (blood test, peptide, longevity, supplement)

### HOW (violation detection)
- Check 1: Report contains "Sources & Confidence" section with all 4 sources listed → if missing, violation
- Check 2: NLM queries were PromptForged → if raw query detected in logs, violation
- Check 3: Report published via Reporter skill (VPS URL present) → if local-only, violation (unless VPS was unreachable)
- Check 4: All health NLM notebooks queried (not just default) → if only 1 notebook and multiple exist, violation
- Runner: manual review by Pafi + Opus audit on report quality

### CONNECT
- HEALTH-R-001 → enforces the 4-source pipeline
- PROMPTING.md → referenced for PromptForge execution (§4b)
- Delphi Reporter skill → publishing step
- `procedure-health.json` → add entry: `{"name": "HEALTH-REPORT", "version": "2.0", "rule": "HEALTH-R-001", "last_verified": "2026-03-24"}`

### VERIFY (procedural checkpoint)
At end of procedure execution, executing agent verifies:
- [ ] All 4 sources queried? (Cortex, NLM, NLM-deep, web)
- [ ] All NLM queries PromptForged? (zero raw queries)
- [ ] Report has all 9 mandatory sections?
- [ ] Published via Reporter to VPS? (or fallback documented)
- [ ] VK emitted in session?
- [ ] If any = NO → procedure NOT complete, do NOT mark DONE

**Two mandatory VKs per execution** (per VK-H-001):
1. **Execution VK**:
   `✅ [HEALTH-REPORT] §1✓ §2✓ §3✓ §4✓ VER✓ | sources: Cortex+NLM+Web | published: {URL}`
2. **Save VK**:
   `✅ [CORTEX] "Health Report: {patient}" | FORGE ✓ | rule: HEALTH-R-001 | v2.0`

### MODEL ROUTING

| Activity | Model | Reason |
|----------|-------|--------|
| Blood panel extraction + analysis | Opus 4.6 | Deep medical reasoning, multi-marker pattern recognition |
| NLM query optimization (/opt) | Sonnet 4.6 | PromptForge is Sonnet-optimized (COST-H-001) |
| Cross-reference synthesis | Opus 4.6 | 4-source reconciliation requires deep reasoning |
| Reporter HTML generation | Sonnet 4.6 | Template-based rendering, no deep reasoning needed |
| Periodic audit of report quality | Opus 4.6 subagent | Structural review (DEV-H-010) |

---

## 4b. Notes for Prompt Generation

This procedure generates prompts at two critical boundaries:
- **NLM queries** (Templates A.1 and A.2) → always PromptForged via `/opt` before dispatch
- **Reporter input** (report markdown) → structured per Section 2 Step 7, no optimization needed (data, not prompt)

**Master reference**: `~/.nexus/procedures/PROMPTING.md` is the unified entry point for all prompting tasks. This procedure does NOT reimplement PromptForge — it references PROMPTING.md via `/opt`.

**Promptware Lifecycle (C-073)**: NLM query templates (A.1, A.2) are versioned in this procedure. When templates change: (1) bump procedure version, (2) run one representative test case per modified template — send the PromptForged query to NLM and verify the response contains: structured sections, source citations, and dosage specifics. If response lacks any of these, the template change has regressed quality — revert.

---

## 5. Dependencies

| Component | Role | Path/Endpoint |
|-----------|------|---------------|
| NLM Service | NotebookLM query API | `~/.nexus/services/nlm-service.py` :${NLM_SERVICE_PORT} |
| NLM Client | Python wrapper for NLM service | `~/.nexus/lib/nlm_client.py` |
| Cortex | Knowledge search + storage | localhost:${CORTEX_PORT} |
| Reporter Skill | HTML report generation + VPS deploy | `~/.claude/plugins/delphi/skills/reporter/SKILL.md` |
| PromptForge | Query optimization | `~/.nexus/procedures/PROMPTING.md` via `/opt` |
| VPS | Report hosting | ${HEALTH_VPS_HOST}:22/80 |
| Health Scripts | Video ingest, data processing | `~/.nexus/projects/health/scripts/` |
| Report Output | Local fallback storage | `~/.nexus/reports/health/` |

---

## 6. Metrics

| Metric | What it measures | Target |
|--------|------------------|--------|
| Sources queried | Number of knowledge sources used per report | 4/4 (100%) |
| NLM notebooks covered | Fraction of health notebooks queried | 100% of available |
| Prompts optimized | NLM queries that went through /opt | 100% (zero raw) |
| Report publish rate | Reports successfully published to VPS | > 95% |
| Marker coverage | Abnormal markers with at least 1 protocol recommendation | 100% |
| Cross-reference conflicts flagged | Disagreements between sources explicitly noted | 100% |

---

## Appendix A: NLM Query Templates

### A.1 Initial Query Template (Source 2)

Before sending, run `/opt` on a query built from this template:

```
ROLE: You are a clinical research assistant with access to all available sources including
Dr. Trevor Bachmeyer's peptide research, Dave Asprey's longevity protocols, and
David P IFBB Pro's performance optimization content.

CONTEXT: Patient blood panel shows the following abnormal markers:
{LIST_ABNORMAL_MARKERS_WITH_VALUES_AND_RANGES}

TASK: For each abnormal marker above:
1. Identify which peptides, supplements, or protocols from your sources directly address it
2. Cite the specific source (video title, episode, or document) for each recommendation
3. Flag any contraindications between recommended peptides and the patient's current markers

OUTPUT FORMAT:
- Group by marker (one section per abnormal marker)
- For each: Marker → Root cause hypothesis → Recommended intervention → Source citation
- End with a SYNERGY MAP showing which interventions address multiple markers simultaneously
```

### A.2 Deep Protocol Template (Source 3)

Run `/opt` on a query built from this template:

```
ROLE: You are a peptide protocol designer referencing 269+ health optimization sources.

CONTEXT: Based on initial analysis, the following interventions were identified:
{LIST_INTERVENTIONS_FROM_SOURCE_2_RESPONSE}

TASK: Design a complete 12-week protocol:
1. PHASE 1 (Weeks 1-4): Foundation — list each peptide/supplement with exact dosage,
   timing (AM/PM/pre-meal/post-workout), injection site rotation if applicable
2. PHASE 2 (Weeks 5-8): Optimization — adjustments based on expected marker response
3. PHASE 3 (Weeks 9-12): Maintenance/cycling — what to keep, what to cycle off

For each peptide include: brand-agnostic name, dose in mcg/mg, frequency, reconstitution
if applicable, storage requirements, and expected timeline to see marker improvement.

ALSO EVALUATE fat loss stack applicability:
- Retatrutide (GLP-1/GIP/glucagon triple agonist)
- 5-amino-1-MQ (NNMT inhibitor)
- AOD-9604 (hGH fragment 176-191)
- NAD+ (IV or sublingual, dose comparison)
- Tesamorelin (GHRH analog, visceral fat)

OUTPUT: Structured protocol table + interaction warnings + retest schedule at week 6 and 12.
```

### A.3 Rules

- Initial NLM search queries (Source 2) — use Template A.1
- Deep NLM analysis queries (Source 3) — use Template A.2
- Any follow-up NLM queries during synthesis — build from templates, always `/opt` first
- Any query sent to any LLM for analysis within this pipeline — `/opt` first, no exceptions
- **Future NLM notebooks** (longevity, nutrition, etc.) — same rule: `/opt` before every query

---

## Appendix B: Execution Checklist

Before delivering ANY health report, verify:

- [ ] Cortex searched (min 3 queries, different angles)
- [ ] ALL NLM health notebooks queried (not just default)
- [ ] NLM queries PromptForged (min 2 queries, both via /opt)
- [ ] Web research done (Tavily pro or Brave)
- [ ] All 4 sources cross-referenced in synthesis
- [ ] Conflicts between sources explicitly flagged
- [ ] Report has all 9 mandatory sections (Step 7)
- [ ] Peptide protocol includes dosages + timing + cycles
- [ ] Fat loss peptides evaluated (retatrutide, 5-amino-1-MQ, AOD-9604, NAD+, tesamorelin)
- [ ] Report published via Reporter skill to VPS
- [ ] VPS link delivered to Pafi
- [ ] HTML renders correctly (dark mode, charts, responsive)
- [ ] Execution VK emitted
- [ ] Cortex save VK emitted

---

## 7. Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-03-24 | Initial: 4-source pipeline, PromptForge mandate, Reporter publish, fat loss peptides |
| 1.1 | 2026-03-24 | Added: NLM query templates (A.1, A.2), query ALL notebooks rule, future-proof for new medical notebooks |
| 2.0 | 2026-03-24 | FORGE v1.9 compliance: §1 Problem, §3 Cortex Logging, §4 Enforcement Loop (WHERE/WHEN/HOW/CONNECT/VERIFY), §4b Prompt Generation notes, MODEL ROUTING, VK format, test cases, metrics, dependencies table, appendices reorganized |
| 2.1 | 2026-03-24 | Audit fixes: Constants table (parameterized VPS/ports), Safety NEVER-list, 2 additional test cases (VPS-down, /opt-unavailable), C-073 template regression test gate, version history consistency fix, owner field |
