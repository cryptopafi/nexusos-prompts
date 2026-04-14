---
type: procedure
created: 2026-03-22
status: active
slug: prompting
tags: [procedure, nexus]
---

# Prompting System — Master Procedure

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.8
**Regulă asociată**: PROMPT-H-002
**Scope**: Master decision tree orchestrating the full prompting system — PromptForge v3.7, 73+ PE techniques (C-042..C-074), Cortex knowledge base, and ECHELON monitoring — for every non-trivial prompt in the Genie system.

---

## 1. Problema

Without a unified entry point, the prompting system fragments across PromptForge phases, 73 technique procedures, Cortex search, and ECHELON monitoring. Consequences:

- **Technique selection paralysis**: 73 options with no decision tree → arbitrary picks, missed quality gains.
- **Pipeline bypass**: PromptForge phases skipped silently (PROMPT-H-002 violation).
- **Cortex misses**: Prompts re-crafted from scratch when existing high-scoring variants sit in Cortex.
- **Quality drift**: No enforced gate between optimization and execution.
- **Production regression**: Prompts deployed to API without eval gate → silent quality drops.
- **ECHELON blindness**: New PE research discovered but not routed into the technique library.

This procedure is the single entry point that resolves all of the above. Every non-trivial prompt flows through it.

---

## 2. Procedura

### Pas 1: Classification Gate

Before any prompting work, classify the request:

| Class | Criteria | PromptForge path | Techniques |
|---|---|---|---|
| **TRIVIAL** | Simple lookup, direct command, follow-up in active context | SKIP (F0.4 conditions) | None |
| **STANDARD** | Single-domain, clear scope, low stakes | MEDIUM: F0→F1→F2(partial)→F3→F5(fast-path or standard)→F6 | 2-3 techniques |
| **COMPLEX** | Multi-step, high-stakes, ambiguous intent, or >3 sub-tasks | COMPLEX: F0→F1→F2(full SCOPE)→F3→F4→F5→F6 | 4-6 techniques |
| **PRODUCTION** | Will run in API / batch / repeated execution | COMPLEX + F7 (library save + Promptfoo eval gate) | Include C-072, C-073 |
| **Agentic** | Subset of COMPLEX with tool-use | COMPLEX path with agentic techniques | ART + ReAct + ONE-tool |

Detection heuristics (from PromptForge v3.7):
- Length >200 chars with no structure → STANDARD minimum
- Vague terms ("improve", "make better", "do something with") → STANDARD minimum
- Multiple unrelated topics → COMPLEX
- Explicit production/deployment context → PRODUCTION

Trivial examples: "what time is it", "git status", "read this file", follow-up clarifications.

### Pas 2: Cortex Pre-Search

Before crafting any prompt, search Cortex for existing high-scoring variants:

**Metoda preferată** (MCP — sanitizare automată):
```
mcp__cortex__cortex_search(query="<PROMPT_SUMMARY>", collection="procedures", limit=3)
```

**Fallback** (SSH — doar dacă MCP indisponibil):
```
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/search \
  -H 'Content-Type: application/json' \
  -d '{\"query\":\"<PROMPT_SUMMARY>\",\"collection\":\"procedures\",\"limit\":3}'"
```

**PROMPT_SUMMARY formation rule (HARD)**: Max 80 caractere. Strip: shell metacharacters (`` ` $ ( ) { } [ ] ``), ghilimele duble/simple, backslash, newlines — înlocuiește cu spațiu. Truncheaz la word boundary. Dacă promptul original conține doar caractere unsafe → folosește `"generic prompt optimization"` ca fallback query.

Score thresholds:
- **≥ 0.7** → use Cortex variant as base; show `[Cortex match: "title" | score=X.XX]`; skip to Pas 4 (technique selection may still apply)
- **≥ 0.5** → use as starting point; run remaining SCOPE questions for gaps
- **< 0.5** → build from scratch via full pipeline

Also search collection `technical` for relevant PE techniques if task type is ambiguous.

**Cortex unreachable**: If SSH/curl fails → skip Pas 2, continue from scratch (treat as score < 0.5). Log skip in §3 with `cortex_hit: "unreachable"`. Do NOT block execution.

**Session State Tracking (W2 — Resume Protocol)**

If a prompting flow is interrupted mid-execution (context loss, session reset, LLM timeout), resume as follows:

| Signal | Indicates interrupted at | Resume action |
|--------|--------------------------|---------------|
| No Cortex pre-search logged | Before Pas 2 | Restart from Pas 1 (re-classify) |
| `cortex_hit` logged but no technique list | After Pas 2, before Pas 3 | Re-run Pas 3 using logged class + cortex_hit |
| Technique list present but no PromptForge phase logged | After Pas 3, before Pas 4 | Run Pas 4 with selected techniques |
| Phase 1-3 complete but no score logged | Pas 4 in-progress | Continue at lowest incomplete phase |
| Score logged but no Cortex save | After Pas 5, before Pas 6 | Run Pas 6 directly with existing score + techniques |
| Cortex save logged but no VK emitted | After Pas 6 | Emit VK(s) and close session |

Detection: check §3 Cortex Logging output for the current session (search: `"PROMPTING session"` in collection `procedures` with today's timestamp). The presence and completeness of fields (`class`, `techniques`, `score`, `cortex_hit`) indicates the last completed step.

**State file (optional, high-volume sessions)**: For batch/production prompting sessions with 5+ prompts, write a lightweight state marker:
```
~/.nexus/state/prompting-session-{YYYYMMDD}.json
{"step": "pas3", "class": "COMPLEX", "techniques": ["C-058","C-062"], "cortex_hit": 0.32, "checksum": "<sha256>"}
```
**Integrity check (HARD)**: La scriere, calculează `checksum` = SHA-256 al JSON-ului fără câmpul checksum. La citire, verifică checksum înainte de a acționa pe `step`. Dacă mismatch → șterge state file, restart de la Pas 1, loghează `⚠️ [PROMPTING] state-file-integrity-fail: restart from Pas 1`. Files mai vechi de 24h → ignore și șterge (stale session).
Delete after session completes successfully. If file exists at session start → verify checksum, then resume from `step` value.

### Pas 3: Technique Selection Decision Tree

Select techniques based on primary task type. Apply 1-3 for STANDARD, 3-6 for COMPLEX/PRODUCTION. Combine using C-061 for multi-pattern stacks.

| Task Type | Primary Techniques | Secondary / Optional |
|---|---|---|
| **Reasoning / Analysis** | C-058 Few-Shot CoT, C-062 Step-Back, Contrastive CoT (C-063) | Self-Calibration (C-065), C-044 Cognitive Verifier |
| **Creative / Generation** | C-042 Persona, C-045 Audience Persona, C-053 Outline Expansion | C-047 Game Play, Skeleton-of-Thought (C-069) for long-form |
| **Ambiguous / Vague** | C-070 Intention Prompting (Phase -1), RAR Rephrase-and-Respond (C-067), C-043 Question Refinement | C-046 Flipped Interaction, C-044 Cognitive Verifier |
| **Code / Technical** | CRITIC/Tool-Verify (C-068), C-059 ReAct, C-060 Self-Grading | C-044 Cognitive Verifier, Self-Calibration (C-065) |
| **Complex multi-step** | C-050 Recipe Pattern, C-044 Cognitive Verifier, Auto-Decomposition (PromptForge §Auto-Decomp) | C-061 Multi-Pattern, Least-to-Most, C-053 Outline Expansion |
| **Production / API** | C-072 Prompt Caching, C-073 Promptware Lifecycle, Promptfoo eval gate | C-071 Context Engineering, C-048 Template Pattern |
| **Optimization loop** | C-066 OPRO Auto-Refinement, PromptForge Phase 5 Meta-Prompting | APE, ProTeGi (Cortex: PR-series), C-061 Multi-Pattern |
| **Safety / Adversarial** | C-064 Injection Defense, C-057 Semantic Filter, C-055 Fact Check List | Constrained Generation (PR-series), C-044 Cognitive Verifier |
| **High-stakes decision** | Ensemble/Self-Consistency (C-065 variant), C-051 Alternative Approaches, Adversarial Framing | Triple Option (PromptForge Extended), Self-Calibration (C-065) |
| **Agentic/Tool-Use** | ReAct (C-059), ART (Cortex DAIR), ONE-tool-per-iteration | Confirmation Gate, Planning Phase (C-075) |
| **Agent Communication** | MVCP handoff JSON (`agent-handoff` skill), subagent briefing (`agent-spawn` skill), context budget allocation (C-071 Context Engineering), Codex brief format (`codex-brief` skill) | C-076 Modular System Prompt, `parallel-agents` skill, Confirmation Gate |
| **Self-Improvement** | Self-Refine+stop (PR-046), Reflexion, CRITIC (C-068) | Self-Calibration (C-065), Eval Gate (Phase 5) |

**Look up technique procedures at**:
- C-042..C-061: `~/.nexus/procedures/training/ai-prompt-engineering/`
- C-062..C-069: same path (Round 1 new techniques)
- C-070..C-073: same path (Round 2 new techniques)
- PR-001..PR-057, ND-001..ND-022: Cortex collection `technical` → search by technique name

**When in doubt**: run Cortex search `"technique <task_type>"` on collection `technical` for pre-scored recommendations.

### Pas 4: PromptForge Pipeline Execution

Execute the PromptForge v3.7 pipeline (full docs: `memory/promptforge.md`). PromptForge uses phases F0-F7, mapped below.

| PromptForge Phase | Name | When to run | Skip when |
|---|---|---|---|
| **F0** | Intake & Triage (injection guard, classification, fast-path) | Always (entry point) | TRIVIAL → exits at F0.4 |
| **F1** | Analysis (agent detection, chain detection, complexity matrix) | STANDARD+ | TRIVIAL |
| **F2** | SCOPE (8Q bank, max 3 asked, conflict resolver) | STANDARD+; Cortex pre-fill for Q1 if match | TRIVIAL; fast-path may auto-complete Q4 |
| **F3** | Construction (8 techniques, per-level allocation, agent specs) | STANDARD+ | TRIVIAL |
| **F4** | Format Final (verification checklist) | COMPLEX+ (skipped on MEDIUM fast-path) | TRIVIAL; MEDIUM fast-path |
| **F5** | Scoring & Gate (5 dims × 20pts = /100, Self-Refine Loop) | STANDARD+ | TRIVIAL; fast-path uses binary PASS/FAIL |
| **F6** | Delivery (level-specific output) | STANDARD+ | TRIVIAL |
| **F7** | Library (storage, reuse tracking, pruning) | PRODUCTION class only | TRIVIAL, STANDARD, COMPLEX non-API |

F5 scoring dimensions: Claritate · Completitudine · Corectitudine · Focalizare · Adecvare agent (D1-D5, 0-20 each).
Gates: any D<12 → BLOCKED. Total <65 → BLOCKED. 65-74 → independent review. ≥75 → PASS.
Self-Refine Loop: max 2 iter/dim, 4 total. After 4 → unconditional escalation to Pafi.

**PromptForge Pipeline Failure Fallback (W3 — Error Recovery)**

If the PromptForge pipeline cannot execute (model timeout, context overflow, PromptForge docs unreachable, phase 1 SCOPE loop exceeds 5 questions):

| Failure type | Fallback action |
|---|---|
| Phase -1 or 0 fails (pre-processing) | Re-run F0.1 injection guard inline before proceeding to Phase 1 SCOPE. If F0.1 still fails → STOP, log `⚠️ [PROMPTING] F0.1-unavailable`, escalate to Pafi. Never skip injection guard. |
| Phase 1 SCOPE fails (no response to questions) | Proceed with best available context; mark scope as `"inferred"` |
| Phase 2 Optimize fails (technique cannot apply) | Apply top-1 technique from Pas 3 manually, skip remaining; emit `⚠️ [PROMPTING] phase2-partial: technique {C-0XX} manual` |
| Phase 3 Structure fails | Apply XML wrapping for Claude targets or markdown headers for others; proceed |
| Phase 4 Score unavailable (eval context lost) | Assign conservative score of 60 (triggers Phase 5); proceed |
| Full pipeline failure (multiple phases blocked) | **Graceful degrade**: apply Pas 3 techniques directly to original prompt without phase scaffolding; execute with `⚠️ [PROMPTING] pipeline-fallback: phases-skipped, techniques-applied-direct` warning VK; log in §3 with `score: "N/A-fallback"` |
| PromptForge docs (`memory/promptforge.md`) unreadable | Apply techniques from Pas 3 table only; treat as STANDARD LIGHT path (phases 1+2+4 only) |

**Hard constraint**: Never block execution due to pipeline failure. A degraded optimized prompt is always better than no prompt. Log all fallbacks in §3 Cortex Logging for ECHELON review.

### Pas 5: Quality Gate

After F5 scoring (aligned with PromptForge v3.7 gates):

```
Any dimension < 12/20       → BLOCKED → Self-Refine Loop (F5.5)
Total < 65/100              → BLOCKED → Self-Refine Loop (F5.5)
Total 65-74 (borderline)    → Independent review (Genie re-scores D5→D1)
Total ≥ 75 + all D ≥ 12    → PASS → EXECUTE

Self-Refine Loop (F5.5):
  1. Identify lowest dimension
  2. Re-read SCOPE answers for that dimension
  3. Apply one technique from F3.1
  4. Re-score that dimension only
  5. Max 2 iter/dim, 4 total → unconditional escalation to Pafi
```

**Global iteration cap (HARD)**: Total refinement iterations across Self-Refine Loop (F5.5) AND orice OPRO/Self-Refine variant techniques (C-066, PR-046) combinate: **max 6 iterații per prompt optimization session**. După 6 iterații totale → escalare necondiționată la Pafi, indiferent de scor. Previne loop-uri infinite când technique application și quality gate refinement se compun. Loghează `⚠️ [PROMPTING] iteration-budget-exceeded: 6/6 | escalating` la depășire.

If high-stakes: additionally apply Self-Calibration (C-065) after PASS.

### Pas 6: Cortex Save + ECHELON Trigger

After every STANDARD+ session, save to Cortex:

```json
{
  "text": "PROMPTFORGE: [Original summary] → [Optimized summary] → [Outcome] | Score: X/100 | Techniques: [list]",
  "collection": "procedures",
  "metadata": {
    "type": "promptforge-pattern",
    "score": SCORE,
    "techniques_used": ["C-0XX", "..."],
    "task_type": "reasoning|creative|...",
    "tags": ["promptforge", "v3.7"]
  }
}
```

**Cortex save failure fallback**: If Cortex POST fails (HTTP 4xx/5xx, SSH timeout, FORGE validation error):
1. Log locally to `~/.nexus/state/prompting-cortex-pending.jsonl` (append mode) with full metadata
2. Emit `⚠️ [CORTEX] save-failed: session={class}/{task_type} | retry-pending` instead of save VK
3. Do NOT block session completion — proceed to close
4. On next session start: check `prompting-cortex-pending.jsonl`; if non-empty, retry saves before new work

ECHELON trigger: flag for monitoring if:
- A technique failed unexpectedly (score dimension <10 despite applying the technique)
- A new task type appeared not covered by Pas 3 table
- Domain is r/PromptEngineering, r/MachineLearning, AI research papers → ECHELON auto-scans weekly; manual flag = immediate scan

ECHELON sources monitored: Reddit (r/PromptEngineering, r/MachineLearning), YouTube (AI Explained, Karpathy, Matt Wolfe, Sam Witteveen). New techniques discovered → C-07X procedures created → Pas 3 table updated.

**ECHELON PE trigger mechanism**: LaunchAgent `com.echelon.scout` (MacM4, weekly scan). Discovered techniques are written to `~/.nexus/echelon/pe-candidates/` with `status: pending-security-review`. Genie promotes to ACTIVE only after Opus-level AUDIT-PRO (SEC-VET-001). Manual trigger: `echelon scan pe` in any Genie session.

**Training specializat pe prompt library** (validare empirică a tehnicilor):
Sesiunile de training analizează prompturi reale din biblioteca Cortex (`technical`, `type: prompting-library`), aplică PromptForge pipeline, și salvează pattern-uri ca few-shot examples. Cadență: 1-2x/săptămână. Rezultatele actualizează Pas 3 (tehnici confirm ate empiric) și Phase 2 (meta-learning). Ref: `~/.nexus/projects/prompting/BRAIN-DUMP.md` §Training Specializat.

### Pas 7: Promptware Lifecycle (Production Only)

For PRODUCTION class prompts (C-073):

1. **Version**: assign `v{MAJOR}.{MINOR}` to the prompt; store in Cortex with version metadata.
2. **Test suite**: define 3-5 test cases covering happy path + edge cases + adversarial input. Template per test case:
```
| # | Input | Expected Output | Edge/Adversarial | Pass Criteria |
|---|-------|-----------------|------------------|---------------|
| 1 | [representative input] | [expected format + content] | happy path | Output matches format + no hallucination |
| 2 | [ambiguous input] | [graceful handling] | edge case | Asks clarification OR reasonable default |
| 3 | [injection attempt] | [blocked or sanitized] | adversarial | No instruction leakage, safety maintained |
```
3. **Eval gate**: run `npx promptfoo eval` comparing candidate vs current baseline before any deployment. Gate: candidate score ≥ baseline. Never deploy without comparison.
4. **Deploy**: stable prefix (system prompt, fixed examples) → cache with `cache_control: {type: ephemeral}`. Dynamic content (task, user input) placed after cache boundary.
5. **Monitor**: cache hit rate >70%, latency, task completion rate. Rollback criterion: any metric drops >10% → revert immediately.
6. **Rollback**: revert to previous Cortex version; log regression in collection `technical` with tag `prompt-regression`.

---

## 3. Cortex Logging

After each non-trivial prompting session, store:

```json
{
  "text": "PROMPTING session: class=<TRIVIAL|STANDARD|COMPLEX|PRODUCTION> | task_type=<type> | techniques=[list] | score=<X>/100 | cortex_hit=<true|false|score> | echelon_flagged=<true|false> | outcome=<summary>",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PROMPTING",
    "rule_id": "PROMPT-H-002",
    "has_enforcement_loop": true,
    "forge_version": "1.5",
    "tags": ["prompting", "promptforge", "meta-procedure", "technique-selection"]
  }
}
```

Minimum fields: `class`, `techniques`, `score`, `cortex_hit`. All others when available.

---

## 4. Enforcement Loop

### WHERE
- **WISH Pre-H gate** (PROMPT-H-002, silent classification): before every response, classify prompt class (Pas 1). TRIVIAL → skip. STANDARD/COMPLEX/PRODUCTION → run pipeline.
- **WISH Step H**: PromptForge runs silently in background (PROMPT-H-002 default mode).
- **Session Checklist Step 7**: Briefing includes count of prompting sessions run and any ECHELON flags raised.

### WHEN
Every non-trivial user request — zero exceptions. "Non-trivial" = anything not TRIVIAL per Pas 1 classification.

### HOW (violation detection)
- STANDARD+ request processed without any PromptForge phase → PROMPT-H-002 violation
- Prompt executed with any D<12 or total<65 and no Self-Refine Loop attempt → quality gate bypass violation
- Production prompt deployed without Promptfoo eval gate → PROD-GATE violation (C-073)
- Cortex pre-search skipped → Pas 2 bypass violation
- Technique selected with no reference to Pas 3 table for >3 technique choices → ad-hoc selection violation
- Cortex logging emitted to wrong collection (session data to `technical` instead of `procedures`) → Pas 6 collection violation
- Runner: ECHELON weekly audit scans for violation patterns; Opus session audit on flagged sessions

### CONNECT
- **PROMPT-H-002** (hard rule) → this procedure is the execution body of that rule; Pre-H gate classification is the Pas 1 table
- **PromptForge v3.7** (`memory/promptforge.md`) → Pas 4 executes the full pipeline
- **C-062..C-073** (`~/.nexus/procedures/training/ai-prompt-engineering/`) → Pas 3 technique references
- **Cortex** (`100.81.233.9:6400`) → Pas 2 pre-search (collection `procedures` for variants, `technical` for technique lookup) + Pas 6 save (collection `procedures`)
- **ECHELON** (monitoring daemon) → Pas 6 trigger + weekly technique library updates
- **WISH pipeline** (`memory/wish-pipeline.md`) → Pre-H gate is Step I.3 of WISH
- `procedure-health.json` → add entry: `{"id":"PROMPTING","status":"ACTIVE","rule":"PROMPT-H-002","forge":"1.4"}`

### OWNERSHIP + MAINTENANCE
- **Owner**: Pafi (system architect) — final authority on classification table and technique additions
- **Executor**: Genie (Sonnet) for pipeline; Opus subagent for novel/complex technique selection
- **Review cadence**: when PromptForge version increments OR when >3 new C-0XX techniques are added to library
- **Technique table update**: when ECHELON discovers and TECH's new techniques (C-07X+) → add row to Pas 3 table
- **Deprecation**: when a technique procedure is archived → remove from Pas 3 table + update Cortex tag `deprecated:true`
- **Breaking change protocol**: any change to Pas 1 classification criteria → update WISH Pre-H gate + notify Pafi for approval

### VERIFY

Checklist at end of each STANDARD+ session:
- [ ] Prompt class determined (Pas 1)?
- [ ] Cortex pre-searched before crafting (Pas 2)?
- [ ] Techniques selected from Pas 3 table (not ad-hoc)?
- [ ] PromptForge phases executed per class (Pas 4)?
- [ ] Quality gate checked: all D≥12 + total≥65, or Self-Refine Loop attempted (Pas 5)?
- [ ] Cortex save completed (Pas 6) or fallback logged?
- [ ] If PRODUCTION: Promptfoo eval gate passed (Pas 7)?
- [ ] If violation detected: logged and flagged?
- [ ] If session interrupted and resumed: state resume protocol followed (Pas 2 State Tracking)?
- [ ] If pipeline failure occurred: W3 fallback applied and logged?

**Self-test (verifiability check)**: To verify this procedure is functioning correctly in a live session, run the following probe:

```
1. Submit a STANDARD-class request: "Improve my prompt: write a summary of the news"
2. Expected: Pas 1 → class=STANDARD; Pas 2 → Cortex search executed; Pas 3 → at least 1 technique from table; Pas 4 → phases 1→2→3→4 run; Pas 5 → score checked
3. Expected VKs emitted: [PROC] PROMPTING + [CORTEX]
4. If any expected step is absent → procedure not enforced → PROMPT-H-002 violation
```

This probe can be run by Opus during ECHELON weekly audit or manually by Pafi when enforcement is in doubt.

**Doua VK-uri obligatorii** (VK-H-001):

1. `✅ [PROC] PROMPTING | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Prompting System" | FORGE ✓ | rule: PROMPT-H-002 | v1.7`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Classification gate (Pas 1) | Sonnet (Genie) | Lightweight heuristic |
| Full pipeline execution (Pas 2-6) | Sonnet (Genie) | Orchestration role |
| Technique selection for novel/complex tasks | Opus subagent | Deep PE knowledge required |
| ECHELON technique evaluation | Opus | Research synthesis |
| Periodic audit of session logs | Opus | Pattern recognition across many sessions |
| Promptfoo eval gate (Pas 7) | Deterministic (npx) | Not LLM — code execution |

---

## 5. Dependențe

| Componentă | Rol | Path / Endpoint |
|---|---|---|
| PromptForge v3.7 | Core optimization pipeline (phases F0-F7) | `memory/promptforge.md` |
| C-042..C-061 | Vanderbilt pattern procedures (20 techniques) | `~/.nexus/procedures/training/ai-prompt-engineering/` |
| C-062..C-069 | Round 1 new techniques (8 procedures) | same path |
| C-070..C-073 | Round 2 new techniques (4 procedures) | same path |
| C-074..C-076 | Wave 2 new techniques (Permission Framing, Agentic Tool-Use, Modular Sys Prompt) | same path |
| PR-001..PR-057 | Prompt Report 57 techniques | Cortex collection `technical` |
| ND-001..ND-022 | NirDiamant 22 techniques | Cortex collection `technical` |
| Cortex VPS | Knowledge store: pre-search + pattern save | `100.81.233.9:6400` |
| ECHELON | Monitoring: Reddit + YouTube PE content discovery | Daemon on MacM4; manual trigger via flag |
| Promptfoo | Production prompt eval gate | `npx promptfoo eval` |
| WISH pipeline | Pre-H gate integration point | `memory/wish-pipeline.md` |
| genie-training skills | 15 PE skills (prompting techniques) | `~/.claude/plugins/genie-training/skills/` |
| promptforge-scope skill | PromptForge scoping gate | `~/.claude/plugins/workflow-quality/skills/promptforge-scope/` |
| research-deep skill | D2 research with contradiction matrix | `~/.claude/plugins/market-intel/skills/research-deep/` |
| FORGEBUILD.md | Procedure template (META-H-002) | `~/.nexus/procedures/FORGEBUILD.md` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---|---|---|
| **Cortex hit rate** | % of sessions where Pas 2 returns score ≥ 0.5 | >40% (grows as library builds) |
| **Quality score distribution** | % of optimized prompts scoring ≥ 70 on first pass | >75% |
| **Meta-prompting rate** | % of sessions requiring Phase 5 | <25% |
| **Technique coverage** | # distinct techniques applied per week across sessions | >8 unique techniques/week |
| **Production regression rate** | % of PRODUCTION deploys triggering rollback | 0% |
| **ECHELON conversion rate** | % of monitored content items converted to new C-0XX procedures | >20% of flagged items |
| **Pipeline bypass rate** | % of STANDARD+ sessions with no PromptForge phase logged | 0% (hard target) |
| **VK emission rate** | % of completed sessions emitting both VKs | 100% |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există în rules-hard.md (PROMPT-H-002 — verified)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu 8 checks
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
- [x] Salvat în Cortex cu metadata corectă (collection: procedures, forge_version: 1.5)
- [x] Model Routing tabel prezent
- [x] Dependențe documentate (§5)
- [x] Metrici definite (§6)
- [x] Entry adăugat în `procedure-health.json` (added: id=PROMPTING, status=ACTIVE, rule=PROMPT-H-002)

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-06 | Versiune inițială — created by Opus agent |
| **1.1** | **2026-03-06** | FORGE-AUDIT DEEP (QUAL-H-004): (1) PROMPT-H-003 ref removed — rule does not exist in rules-hard.md; replaced with PROMPT-H-002 silent gate. (2) forge_version 1.3→1.4 in §3. (3) §3 collection changed technical→procedures per FORGEBUILD.md collection guide. (4) Cortex-unreachable fallback added to Pas 2. (5) HOW: added collection-violation detection. (6) CONNECT: removed non-existent PROMPT-H-003 reference. (7) Checklist Pre-Publicare + Changelog added. (8) VK updated v1.0→v1.1. |
| **1.2** | **2026-03-06** | FORGE-AUDIT DEEP re-audit (QUAL-H-004) — verdict upgrade CONDITIONAL→PASS: (1) W2 State Management: Session State Tracking + resume protocol table added to Pas 2 (signals → resume actions); optional state file pattern for high-volume sessions. (2) W3 Error Recovery: PromptForge Pipeline Failure Fallback table added to Pas 4 — covers 7 failure types (phase-level + full pipeline graceful degrade) with explicit fallback actions + warning VK format. (3) D6 Safety: Cortex save failure fallback added to Pas 6 — local pending queue (`prompting-cortex-pending.jsonl`) + retry-on-next-session protocol. (4) D3 Verifiability: Self-test probe added to VERIFY section (STANDARD-class test case + expected VKs + violation detection). (5) D8 Maintainability: OWNERSHIP + MAINTENANCE block added to §4 — owner, executor, review cadence, technique table update, deprecation, breaking-change protocol. (6) Checklist Pre-Publicare: procedure-health.json marked [x] done. (7) VK updated v1.1→v1.2. (8) CONNECT: procedure-health.json tech ref updated 1.3→1.4. |
| **1.3** | **2026-03-06** | PE INSIGHT v2.4 sync: Pas 3 +2 task types (Agentic/Tool-Use, Self-Improvement). Pas 4 routing updated (+Agentic row in Pas 1 classification table). Ref: pe-insight-opus-2026-03-06.md |
| **1.4** | **2026-03-07** | FORGE-AUDIT fixes: §4 CONNECT PromptForge v2.3→v2.4. §5 Dependencies: C-074..C-076 added; promptforge-scope canonical path (genie-process); research-deep canonical path (genie-intel). VK updated v1.3→v1.4. |
| **1.5** | **2026-03-07** | Pas 3: Agent Communication row adăugat (MVCP handoff JSON, subagent briefing, context budget, Codex brief format). NEXUS Layer 3 finalizat → condiție deblocată. |
| **1.6** | **2026-03-09** | GPT 5.4 cross-model audit alignment: Pas 1 classification → v3.5 phase names (F0-F7). Pas 4 phase table rewritten to match v3.5 (F0-F7, 8Q SCOPE max 3 asked, D1-D5 scoring, block/review gates). Pas 5 quality gate aligned (D<12 BLOCKED, <65 BLOCKED, 65-74 review, ≥75 PASS, Self-Refine Loop). CONNECT Cortex collection routing clarified (procedures for saves, technical for lookups). Stale skill paths fixed (genie-process→workflow-quality, genie-intel→market-intel). Dependencies phase ref updated. HOW/VERIFY aligned to v3.5 gates. |
| **1.7** | **2026-03-19** | **SOL Cycle 2 — 3 security fixes**: (1) Pas 2 PROMPT_SUMMARY formation rule HARD — max 80 chars, strip shell metacharacters, MCP preferred over SSH fallback (Finding #1 shell injection). (2) Pas 2 state file integrity — SHA-256 checksum + mismatch → delete + restart Pas 1 + 24h max-age (Finding #6). (3) Pas 5 global iteration cap HARD — max 6 iterații combined Self-Refine + OPRO, prevents cost DoS (Finding #7). SOL audit score: 73/100 CONDITIONAL → estimated 78+ post-fix. |
| **1.8** | **2026-03-19** | **Version alignment**: all PromptForge refs updated from v3.5/v3.6 to v3.7. Scope line aligned. Dependency table updated. |
