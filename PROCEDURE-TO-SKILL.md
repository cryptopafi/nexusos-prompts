---
type: procedure
created: 2026-03-22
status: active
slug: procedure-to-skill
tags: [procedure, nexus]
---

# PROCEDURE-TO-SKILL — Standard Operating Procedure for Converting Procedures to Skills

> **MERGED INTO**: FORGEBUILD.md v1.5 §8 Conversion Annex — this file is archived, see FORGEBUILD.md for the authoritative version.

**Status**: ARCHIVED (merged into FORGEBUILD.md §8)
**Creat**: 2026-03-20
**Versiune**: 1.0
**Regulă asociată**: META-H-002 (enforcement), DEV-H-010 (quality gates)
**Scope**: Standardized process for converting any NexusOS procedure into a Claude Code skill, plugin component, CLI wrapper, or embedded logic — eliminating dead documentation and maximizing automation reuse.

---

## 1. Problema

Without a standardized conversion process:

- Procedures stay as markdown documents that nobody reads or executes — dead knowledge
- No automation, no CLI entry points, no reusable skills — manual-only execution
- Duplicate logic across procedures and skills when conversions happen ad-hoc
- Inconsistent conversion quality — some skills get full SKILL.md with I/O contracts, others get bare-bones wrappers
- Original procedures are not archived, creating confusion about which version is canonical
- Skills are created from scratch when an existing procedure already captures the logic

Situations covered:
- Converting an operational procedure into a SKILL.md for agent consumption
- Wrapping a Python/shell script procedure as a CLI tool with JSON output
- Embedding small procedure logic into an existing skill (no new skill needed)
- Bundling multiple related procedures into a plugin
- Deciding that a procedure is reference-only (policy/rules, no executable steps)

---

## 2. Procedura

### Phase 1: ASSESS — Is the procedure suitable? What type of conversion?

**Step 1.1: Classification decision tree**

```
Procedure → Is it executable (has concrete steps with input/output)?
  │
  NO → REFERENCE ONLY
  │     Add to an existing SKILL.md "References" section
  │     Example: QUAL-H-004 Devil's Advocate → referenced in critic SKILL.md
  │
  YES → How complex is the logic?
    │
    ├─ 1-3 clear steps with defined I/O → SKILL (standalone SKILL.md)
    │   Example: EPR scoring → could be standalone skill
    │
    ├─ Shell/Python script wrapper → CLI (shell script + optional SKILL.md)
    │   Example: research.py → commands/research.md + CLI wrapper
    │
    ├─ Small logic, fits inside existing skill → EMBEDDED
    │   Example: X-EXTRACTION steps → embedded in scout-social SKILL.md
    │
    └─ Multiple related procedures → PLUGIN (bundle of skills + commands)
        Example: all scout procedures → delphi plugin with 13 skills
```

**Step 1.2: Duplication check**

Before converting, search for existing skills:
1. `ls ~/.claude/plugins/` — all installed plugins
2. `cortex_search "skill:<procedure-name>"` — Cortex registry
3. `grep -r "<procedure-name>" ~/.claude/plugins/*/skills/*/SKILL.md` — existing skills referencing it
4. If similar skill exists → EMBEDDED or REFERENCE ONLY (do not create duplicate)

**Step 1.3: Consumer scope check**

- Procedure used by multiple agents/consumers? → Shared skill in `~/.claude/plugins/{plugin}/skills/`
- Procedure used by only one agent? → Embedded in that agent's skill or inline in agent file
- Procedure used as a standalone tool by Pafi? → CLI wrapper with `--help`

### Phase 2: DECOMPOSE — Break procedure into skill components

**Step 2.1: Extract contracts**

For each procedure being converted, identify:

| Component | Question to answer | Maps to |
|---|---|---|
| Input contract | What does it receive? (topic, URL, config, data) | SKILL.md `## Input` JSON schema |
| Output contract | What does it produce? (findings, scores, reports) | SKILL.md `## Output` JSON schema |
| Tools needed | What MCP tools, CLIs, APIs does it need? | SKILL.md `## Execution` tool table |
| Dependencies | Other procedures, data sources, configs it needs | SKILL.md `## Dependencies` or `## References` |
| Error cases | What can go wrong? Rate limits, auth fails, empty data | SKILL.md `## Error Handling` |
| Boundaries | What does it do / what does it NOT do | SKILL.md `## What You Do` / `## What You Do NOT Do` |

**Step 2.2: Map steps to skill format**

For each step in the original procedure:
1. Classify as: execution step, validation step, routing step, or reference
2. Execution steps → become numbered steps in SKILL.md `## Execution`
3. Validation steps → become `## Input Validation` rules
4. Routing steps → become decision logic in orchestrator agent or command
5. Reference steps → become `## References` or inline comments

### Phase 3: CONVERT — Build the skill/CLI/embedded logic

#### Path A: SKILL (standalone SKILL.md)

1. Create `~/.claude/plugins/{plugin}/skills/{skill-name}/SKILL.md`
2. Use this YAML frontmatter format:
   ```yaml
   ---
   name: {skill-name}
   description: "{One sentence: what it does}. Use when {trigger}."
   model: haiku|sonnet|opus
   ---
   ```
3. Required sections (in order):
   - `## What You Do` — positive boundaries (2-5 bullet points)
   - `## What You Do NOT Do` — negative boundaries (3-5 bullet points, reference who DOES handle those)
   - `## Input` — JSON schema with example
   - `## Input Validation` — edge cases, defaults, error returns
   - `## Execution` — numbered steps, each with clear action
   - `## Output` — JSON schema with example (matching handoff contract)
   - `## Error Handling` — fallback chains, graceful degradation
   - `## CLI Usage` — standalone invocation example (if CLI exists)
4. Add query templates if procedure involves search (per-channel patterns)
5. Add deduplication step if procedure aggregates data from multiple sources
6. Reference original procedure: `Source: ~/.nexus/procedures/{original}.md (CONVERTED)`

#### Path B: CLI (shell script wrapper)

1. Create `~/.claude/plugins/{plugin}/skills/{skill-name}/cli/{script-name}.sh`
2. Required patterns:
   - `#!/bin/bash` + `set -euo pipefail`
   - Arg parsing with `while [[ $# -gt 0 ]]` + `case` statement
   - `--help` flag that prints usage
   - Error output as JSON to stderr: `{"status": "error", "error": "message", "agent": "skill/sub"}`
   - Success output as JSON to stdout (structured, matching handoff contract)
   - If Python logic needed: use quoted heredoc pattern (`python3 << 'PYEOF'`)
   - Export ALL bash variables before Python heredoc (prevent shell injection)
   - Exit codes: 0 = success, 1 = error
3. If CLI is the primary interface, also create a minimal SKILL.md that references the CLI

#### Path C: EMBEDDED (add to existing skill)

1. Open existing SKILL.md where logic belongs
2. Add relevant steps to the `## Execution` section (numbered, integrated with existing flow)
3. Add comment referencing original procedure: `<!-- Source: X-EXTRACTION v1.1, embedded 2026-03-20 -->`
4. Do NOT duplicate logic — integrate with what exists
5. Update Input/Output contracts if new fields are needed
6. If embedding adds >50 lines to the skill, reconsider → maybe it should be standalone

#### Path D: PLUGIN (bundle of related skills)

1. Create plugin directory structure:
   ```
   ~/.claude/plugins/{plugin-name}/
   ├── .claude-plugin/plugin.json    ← Manifest (name, version, skills list)
   ├── skills/{skill-1}/SKILL.md     ← Individual skills
   ├── skills/{skill-2}/SKILL.md
   ├── commands/{command}.md          ← User-facing slash commands
   ├── hooks/{lifecycle}.sh           ← Pre/post/error hooks
   ├── agents/{agent}.md              ← Orchestrator agent (if needed)
   ├── procedures/{proc}.md           ← FORGE-format procedures
   └── resources/                     ← Config, templates, state
   ```
2. Each skill follows Path A format
3. Each CLI follows Path B format
4. Write plugin.json manifest listing all skills
5. Write commands that dispatch to agents/skills

### Phase 4: AUDIT — Verify the conversion

| Check | Tool | Minimum score | Applies to |
|---|---|---|---|
| Skill format validation | Skill Creator 3.0 | Score >= 70 | SKILL.md files |
| Prompt quality | PromptForge v3.6 | Score >= 70 | All prompts (SKILL.md, agent.md, commands) |
| Procedure structure | AUDIT-PRO tier STANDARD | Score >= 3.5/4.0 | FORGE-format procedures |
| CLI output | Manual test with real data | Valid JSON output | CLI scripts |
| Contract compatibility | Compare I/O JSON schemas | Exact match with consumers | All types |
| Intent preservation | Manual review | Original procedure goals met | All types |
| No duplication | grep across all skills | Zero duplicate logic blocks | All types |

### Phase 5: INTEGRATE — Wire into the system

1. **Update references**: If new skill added, update agents that should reference it
2. **Update plugin.json**: Add new skill to manifest
3. **Update channel-config.yaml**: If new search channel added (for DELPHI-type plugins)
4. **Integration test**: Run end-to-end with real data through the full pipeline
5. **Archive original**: In original procedure file, add header:
   ```
   **Status**: CONVERTED
   **Converted to**: ~/.claude/plugins/{plugin}/skills/{skill-name}/SKILL.md
   **Conversion date**: YYYY-MM-DD
   **Conversion type**: SKILL | CLI | EMBEDDED | REFERENCE
   ```
   Do NOT delete original — it serves as documentation history.

### Test Cases

1. **Normal flow (SKILL conversion)**: Procedure with 3 steps, clear JSON input/output → Creates SKILL.md with all sections, passes Skill Creator score >= 70, consumers can invoke it
2. **Edge case (EMBEDDED)**: Small procedure (1 step, 10 lines) that overlaps 80% with existing skill → Embeds into existing skill with source comment, no new file created
3. **Failure case**: Procedure is a policy document with no executable steps → Classified as REFERENCE ONLY, added to existing skill's References section, no conversion attempted

---

## 3. Cortex Logging

After each conversion, save a record:

```json
{
  "text": "Converted procedure {PROCEDURE-NAME} to {TYPE} at {PATH}. Original: {ORIGINAL_PATH}. Audit scores: SC={score}, PF={score}, FORGE={score}.",
  "collection": "procedures",
  "metadata": {
    "type": "procedure-conversion",
    "procedure": "PROCEDURE-TO-SKILL",
    "rule_id": "META-H-002",
    "source_procedure": "{ORIGINAL-NAME}",
    "conversion_type": "SKILL|CLI|EMBEDDED|REFERENCE|PLUGIN",
    "target_path": "{output path}",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["conversion", "skill", "automation"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (when any new skill is being created from an existing procedure)
- SOL Phase 2 "Audit" (when SOL reviews procedures for improvement opportunities)
- DELPHI-SOC Faza 2 (monthly review of procedure→skill conversion candidates)

### WHEN
- Every time a procedure is being converted to a skill (pre-conversion gate)
- Monthly: DELPHI-SOC F2 scans for unconverted procedures that should be skills
- On new procedure creation: check if a skill would be better format

### HOW (violation detection)
- Skill created without checking for existing procedure → violation
- Procedure converted without following Phase 1-5 sequence → violation (partial conversion)
- Original procedure not archived after conversion → violation (leads to confusion)
- Skill published without audit scores (Phase 4) → violation
- Runner: DELPHI-SOC nightly scan, manual review at procedure creation time

### CONNECT
- META-H-002 → enforcement structure
- DEV-H-010 → quality gates (audit scores)
- FORGEBUILD.md → template structure this procedure follows
- DELPHI-SOC.md → Faza 2 monthly review references this procedure
- `procedure-health.json` → add entry for PROCEDURE-TO-SKILL

### VERIFY (procedural checkpoint — emitted as VK in session)
- [ ] Procedure was executed completely? (all 5 phases: ASSESS → DECOMPOSE → CONVERT → AUDIT → INTEGRATE)
- [ ] Output satisfies criteria from §1? (dead procedure is now live skill/CLI/embedded)
- [ ] VK emitted in session? (both lines below visible for Pafi)
- [ ] If any = NO → procedure is NOT complete, do not mark DONE

**Two mandatory VKs per FORGE procedure** (per VK-H-001):
1. **Execution VK**:
   `[PROC] PROCEDURE-TO-SKILL | §1 §2 §3 §4 VER | {PROCEDURE-NAME} → {TYPE} at {PATH}`
2. **Save VK**:
   `[CORTEX] "Converted {NAME}" | FORGE | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activity | Model | Reason |
|---|---|---|
| Assess + Decompose (Phase 1-2) | Sonnet 4.6 | Pattern matching, decision tree |
| Convert (Phase 3) | Sonnet 4.6 | Writing SKILL.md, CLI, embeddings |
| Audit (Phase 4) | Opus 4.6 subagent | Quality scoring requires deep reasoning |
| Integrate (Phase 5) | Sonnet 4.6 | File edits, manifest updates |

---

## 5. Dependențe

| Component | Role | Path/Endpoint |
|---|---|---|
| FORGEBUILD.md | Template structure for procedures | `~/.nexus/procedures/FORGEBUILD.md` |
| Skill Creator 3.0 | Validates SKILL.md format and scores | `/skill-creator` skill |
| PromptForge v3.6 | Scores prompt quality | `~/.nexus/procedures/PROMPTING.md` |
| AUDIT-PRO | Structural audit of procedures | `~/.nexus/audit/AUDIT-PRO.md` |
| Cortex MCP | Stores conversion records | `cortex_store` / `cortex_search` |
| DELPHI-SOC | Monthly review triggers | `~/.claude/plugins/delphi/procedures/DELPHI-SOC.md` |

---

## 6. Metrics

| Metric | What it measures | Target |
|---|---|---|
| Conversion rate | % of executable procedures that have been converted | > 80% |
| Audit pass rate | % of converted skills scoring >= 70 on first attempt | > 60% |
| Dead procedure count | Procedures that are executable but not converted | 0 (aspirational) |
| Duplication index | Duplicate logic blocks across skills | 0 |

---

## 7. Real Examples from DELPHI PRO Build

### Example 1: X-EXTRACTION v1.1 → EMBEDDED in scout-social

**Original**: 7-step pipeline procedure for extracting X/Twitter data (query construction, hashtag optimization, engagement filtering, dedup, normalization).

**Assessment**: Steps overlap heavily with scout-social's X/Twitter channel. Small enough to embed (adds ~30 lines). Only one consumer (DELPHI via scout-social).

**Conversion**: Embedded as the "X/Twitter" query template section in `scout-social/SKILL.md`. The 7 steps became: query format rules, engagement sorting, content preview extraction, and T2 tier assignment. Source comment added.

**Result**: `~/.claude/plugins/delphi/skills/scout-social/SKILL.md` — X/Twitter section under Query Templates. Original procedure marked CONVERTED.

### Example 2: EPR modules (epr.py + 3 scoring modules) → EMBEDDED in critic

**Original**: Python module with 3 sub-modules: Evidence quality, Perspective diversity, Reasoning depth. Produced a composite EPR score 0-20.

**Assessment**: EPR scoring is the core of what Critic does. Embedding makes it the backbone of Step 2 in Critic's execution flow. Used only by DELPHI pipeline.

**Conversion**: The 3 EPR dimensions became the 5-dimension evaluation table in `critic/SKILL.md` (expanded from 3 to 5: added Authority + Temporal). EPR composite score became the weighted formula in Step 2. Python calculation logic referenced but not inlined (Critic computes via LLM reasoning, not Python).

**Result**: `~/.claude/plugins/delphi/skills/critic/SKILL.md` — Steps 1-3 are EPR-based evaluation. Original epr.py archived.

### Example 3: research.py CLI → COMMAND + SKILL wrapper

**Original**: Python CLI script that accepted `--topic` and `--depth` args, orchestrated a research pipeline.

**Assessment**: Already a CLI — needs SKILL.md wrapper for agent invocation + command for user-facing `/research` entry. Two outputs needed.

**Conversion**:
1. Created `commands/research.md` — slash command definition with argument parsing (`/research [topic] [--depth D1-D4]`), dispatches to DELPHI agent
2. The original CLI logic was absorbed into the `agents/delphi.md` orchestrator (which replaced the Python script with LLM-native orchestration)
3. CLI wrappers for individual scouts handle the actual shell execution

**Result**: `commands/research.md` + `agents/delphi.md` replace research.py. Original script archived.

---

## 8. Anti-patterns

| Anti-pattern | Why it is harmful | What to do instead |
|---|---|---|
| **Over-atomization** — converting every procedure into a separate skill | Explosion of tiny skills, hard to discover, high overhead | Embed small procedures into existing skills |
| **Context loss** — splitting one procedure across 5+ skills | Original intent gets fragmented, nobody understands the full flow | Keep cohesive logic together; split only at natural boundaries |
| **CLI for everything** — creating shell wrappers for things that should be MCP tools | Unnecessary process spawning, no streaming, poor error handling | Use MCP tools for things that need real-time interaction |
| **Ghost procedures** — not archiving originals after conversion | Two versions exist, nobody knows which is canonical | Always mark original as CONVERTED with pointer to new location |
| **Mega-skills** — embedding too much into one SKILL.md (>200 lines) | Hard to maintain, slow to load, confusing boundaries | Split into standalone skill when embedding exceeds 200 lines |
| **Blind conversion** — converting without checking if similar skill exists | Creates duplicates that drift apart over time | Always run duplication check (Phase 1, Step 1.2) first |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (META-H-002, DEV-H-010)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu cele 3 checks obligatorii
- [x] Procesul din WHERE a fost identificat (WISH Step H, SOL, DELPHI-SOC)
- [ ] Entry adăugat în `procedure-health.json`
- [x] Salvat în Cortex cu metadata (see §3)
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
- [x] Test cases documentate — 3 real examples from DELPHI PRO

---

## Changelog

| Versiune | Data | Modificări |
|---|---|---|
| 1.0 | 2026-03-20 | Initial version. Based on patterns extracted from DELPHI PRO build session. 5-phase conversion pipeline (ASSESS → DECOMPOSE → CONVERT → AUDIT → INTEGRATE). 4 conversion types (SKILL, CLI, EMBEDDED, REFERENCE + PLUGIN). 3 real examples. 6 anti-patterns. |
