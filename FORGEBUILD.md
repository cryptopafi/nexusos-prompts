---
type: procedure
created: 2026-03-22
status: active
slug: forgebuild
tags: [procedure, nexus]
---

# FORGEBUILD — Standard Procedure Template

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Structura obligatorie pentru orice procedură nouă creată în sistem.

**Activare**:
- **AUTOMATĂ** — Genie aplică FORGE silent la FIECARE procedură nouă (META-H-002). Nu trebuie cerut.
- **MANUALĂ** — Pafi zice `tech X` = creează procedură nouă pentru X. Pafi zice `aplică FORGE pe Y` = standardizează procedura Y existentă.

---

## Meta: Enforcement pe acest template

> **Notă**: Această secțiune se aplică procedurii FORGE în sine (auto-referențial). §0-§4 de mai jos sunt template-ul pentru alte proceduri. Meta enforcement = cum se verifică că FORGE e respectat la crearea altor proceduri.

### WHERE
- WISH Step H (orice moment în care Genie creează o procedură nouă)
- WISH Post-H Skill extraction (când salvezi o lecție ca procedură)

### WHEN
- La FIECARE creare de procedură nouă — zero excepții

### HOW (violation detection)
- Procedură nouă fără secțiunile obligatorii (§1-§4) → violation
- Procedură fără enforcement loop (§4) → violation (META-H-002)
- Procedură fără regulă asociată → violation
- Procedură fără VERIFY checkpoint (§4.1) → violation
- Runner: `test-all-procedures.js` (daily 02:00), Opus audit la review, checklist pre-publicare (§7 mai jos)
- API gate: `forge.ts` middleware rejectă procedures fără FORGE metadata (HTTP 422)

### CONNECT
- META-H-002 (hard rule) → Step 0 referențiază acest template
- PROC-H-001 → enforcement check la Cortex search verifică structura
- `test-all-procedures.js` → scanează toate procedurile pentru structura standard
- `procedure-health.json` → entry pentru acest template

---

## Decision Gate

| Ce ai? | Path | Secțiune |
|:---:|:---:|:---:|
| Vreau procedură nouă | **CREATE** | §0-§7 (template standard) |
| Am procedură veche, transform în skill/plugin | **CONVERT** | §8.1-§8.9 |
| Fac plugin complet (skills + commands + hooks) | **ASSEMBLE** | §8.10 (E.1-E.6) |

---

## §0 SKILL-SEARCH Gate (obligatoriu când output-ul e un skill)

**REGULA**: Înainte de a construi orice skill de la zero, Genie caută ÎNTÂI în toate sursele conectate.
Construcție de la zero = ULTIMĂ OPȚIUNE, nu prima.

```
Output-ul sarcinii este un skill nou?
  │
  YES → SKILL-SEARCH obligatoriu (ÎNAINTE de orice cod/design)
  │     ├─ Tier 0 — Surse INTERNE (caută PRIMUL):
  │     │   ├─ ~/.claude/plugins/          (toate plugin-urile instalate — ls pentru listă)
  │     │   ├─ ~/.claude/plugins/genie-training/skills/  (31 skills PE/training)
  │     │   ├─ ~/.nexus/skills/registry.json  (registry local — dacă există)
  │     │   └─ Cortex: cortex_search "skill:<name>" (collection: procedures)
  │     │
  │     ├─ Tier 1-2 — Surse externe (dacă nu găsești intern):
  │     │   ├─ ClawHub: clawhub search "<keywords>"
  │     │   ├─ anthropics/claude-plugins-official
  │     │   ├─ awesome-claude-code / awesome-claude-skills
  │     │   └─ PulseMCP / Official MCP Registry
  │     │
  │     ├─ GĂSIT → prezintă Pafi opțiunile: INSTALL / COPY-REBUILD
  │     └─ NEGĂSIT → construiești de la zero (documentează că ai căutat)
  │
  NO → continui cu template-ul standard de mai jos
```

**VK obligatoriu dacă e skill**:
`🔍 [SKILL-SEARCH] căutat în: {surse verificate} | rezultat: FOUND({name}) / NOT_FOUND → building from scratch`

### §0.5 COMPONENT-REUSE Scan (obligatoriu când §0 = NOT_FOUND)

**REGULA**: Chiar dacă skill-ul/plugin-ul NU există ca duplicat, componentele altor plugin-uri pot fi refolosite ca building blocks. Scanează ÎNAINTE de a construi de la zero.

```
§0 SKILL-SEARCH → NOT_FOUND (nu există duplicat)
  │
  ▼
§0.5 COMPONENT-REUSE SCAN
  │
  ├─ Scan 1: Plugin skill inventories
  │   Glob: ~/.claude/plugins/*/skills/*/SKILL.md
  │   Extrage: name + description (prima propoziție) din fiecare
  │   Filtrează: care se potrivesc ca dependency/building block pentru task-ul curent
  │   Limită: top 10 cele mai relevante. Dacă >10, notează total count în REUSE TABLE
  │
  ├─ Scan 2: Cortex component search
  │   cortex_search("reusable components for: {task description}", collection="procedures", limit=5)
  │   Filtrează: score ≥ 0.5
  │   Cortex unreachable? → skip Scan 2, note "[Cortex DOWN]" in VK, proceed with Scan 1+3 only
  │
  ├─ Scan 3: Agent capabilities
  │   Glob: ~/.claude/plugins/*/agents/*.md
  │   Extrage: name + description (prima propoziție) din fiecare
  │   Filtrează: agenți care pot fi orchestrați sau apelați de noul skill/plugin
  │   Skip: fișiere cu prefix "REFERENCE" sau fără frontmatter agent (name + description)
  │
  └─ Output: REUSE TABLE (prezintă Pafi)
      | Component | Source Plugin | Reuse? | How |
      |-----------|-------------|--------|-----|
      | {name} | {plugin} | YES/PARTIAL/NO | Import / Fork+Adapt / Reference |

  How definitions:
  - **Import** = call as dependency at runtime (no code duplication)
  - **Fork+Adapt** = copy source and modify for your needs (when original is 70%+ fit)
  - **Reference** = document as related, no code dependency (for documentation/context only)

  Decision:
  ├─ ≥1 YES/PARTIAL → "Reuse {N} components, build only {M} new"
  │   → Adaugă componentele refolosite în §5 Dependențe ale procedurii noi
  │   → NU duplica logica — importă/referențiază componenta existentă
  │
  └─ 0 reusable → "No reusable components found → full build from scratch"
```

**Exemple de componente refolosibile (canonical)**:
- `delphi/skills/scout-social` — căutare pe social media (Instagram, TikTok, X)
- `delphi/skills/scout-web` — căutare web cu surse multiple
- `delphi/skills/scout-video` — căutare video (YouTube, Vimeo)
- `delphi/skills/store-cortex` — salvare rezultate în Cortex
- `delphi/skills/store-notion` — salvare rezultate în Notion
- `delphi/skills/critic` — evaluare calitativă cu scoring multi-dimensional
- `delphi/skills/reporter` — generare rapoarte HTML
- `~/.claude/skills/audit` — audit universal NPLF cu 9 tipuri (AUDIT-PRO, core procedure la `~/.nexus/audit/`)
- `~/.claude/skills/audit-codex` — dispatch audit la GPT-5.4 via codex exec

**VK obligatoriu**:
`🔍 [COMPONENT-REUSE] scanned {N} plugins, {M} skills, {K} agents | reusable: {list sau "none"} | new-build: {list}`

**Anti-pattern**: NU duplica un skill existent doar pentru a-l avea "în plugin-ul tău". Dacă `scout-social` face ce ai nevoie, importă-l ca dependență — nu rescrie logica.

---

### SKILL.md Format Standard (când construiești de la zero)

Orice skill nou TREBUIE să respecte formatul plugin standard (`~/.claude/plugins/{plugin}/skills/{name}/SKILL.md`):

```yaml
---
name: {skill-name}           # kebab-case, fără prefix "skill-"
description: |
  {O propoziție — CE face skill-ul}. Use when {trigger condition}.
  ANTI-PATTERN: Do NOT use for {when NOT to invoke}.
version: 1.0.0
---

## Procedure Reference
**Source**: `~/.nexus/procedures/training/{domain}/{procedure-file}.md`
**Read the full procedure before executing.**

## Quick Summary
{2-4 propoziții care acoperă esența procedurii — ce verifici, ce produci, cum validezi.}

## Key Steps
1. **{Pas 1}** — {ce faci concret}
2. **{Pas 2}** — {ce faci concret}
3. **{Pas N}** — {ce faci concret}

## When to Use vs Alternatives
- **{acest skill}** (this): {când e alegerea corectă — trigger specific}
- **{skill alternativ}**: {când e mai potrivit decât acesta}
- **{alt skill}**: {când e mai potrivit decât acesta}
```

**Reguli SKILL.md**:
- `description` trebuie să conțină: trigger pozitiv + ANTI-PATTERN (când NU se folosește)

**Why description matters**: The description is the ONLY thing the orchestrator/agent sees when deciding which skill to load. It is surfaced in the system prompt alongside all other installed skills. The agent reads these descriptions and picks the relevant skill based on the user's request. If the description is vague, the skill will never trigger.

- `Procedure Reference` → path absolut la procedura sursă din `~/.nexus/procedures/`
- `When to Use vs Alternatives` → minimum 2 alternative cu discriminator clar
- Plugin path: `~/.claude/plugins/{genie-domain}/skills/{skill-name}/SKILL.md`
- Plugin nou → adaugă în `~/.claude/plugins/{plugin}/.claude-plugin/plugin.json`

---

## Template — Copiază de mai jos pentru proceduri noi

---

# {TITLU} — Procedură Standard de Operare

**Status**: DRAFT | ACTIVE | DEPRECATED
**Creat**: {YYYY-MM-DD}
**Versiune**: {X.Y}
**Regulă asociată**: {RULE-ID} (din rules-hard.md sau rules-standard.md)
**Scope**: {O propoziție — ce acoperă procedura}

---

## 1. Problema

{Ce problemă rezolvă? De ce există? Fără procedura asta, ce se întâmplă?}

{Tipuri de situații acoperite — bullet list.}

---

## 2. Procedura

{Pașii concreți. Numerotați. Fiecare pas = o acțiune clară.}

### Pas 1: {NUME_PAS}
{Ce faci, cum verifici, ce output.}

### Pas 2: {NUME_PAS}
{Idem.}

### Pas N: {NUME_PAS}
{Idem.}

### Test Cases (opțional, recomandat pentru skills)
Situații concrete în care procedura/skill-ul trebuie să funcționeze:
1. **Normal flow**: {descriere situație tipică → output așteptat}
2. **Edge case**: {situație limită → comportament așteptat}
3. **Failure case**: {când procedura NU se aplică → ce se face în schimb}

Dacă skill-ul e creat cu Skill Creator, aceste test cases devin `evals.json` entries.

{NOTĂ: descrie CE, nu CUM. Lasă implementatorului libertatea de a alege soluția optimă.
Anti-pattern: cod inline complet. OK: pseudocod scurt, referințe la fișiere.}

---

## 3. Cortex Logging

{Ce se salvează în Cortex după execuție.}

```json
{
  "text": "{ce se stochează}",
  "collection": "{collection_name}",
  "metadata": {
    "type": "{type}",
    "procedure": "{PROCEDURE-NAME}",
    "rule_id": "{RULE-ID}",
    "has_enforcement_loop": true,
    "forge_version": "2.0",
    "tags": ["{tag1}", "{tag2}"]
  }
}
```

**Ghid colecție Cortex** (când nu e evident):
- `"procedures"` → proceduri operaționale, audit reports, promptforge patterns, handoff, session summaries
- `"technical"` → tehnici PE (PR-001..PR-057, ND-001..ND-022, C-042..C-073), skills tehnice, findings de research, prompt regressions

---

## 4. Enforcement Loop (META-H-002)

### WHERE
{În ce proces/gate este verificată? (WISH step, Post-H, Session Checklist, Health Check, Daemon, etc.)}

### WHEN
{Ce event o declanșează? (la fiecare task, la fix, zilnic, la Codex brief, etc.)}

### HOW (violation detection)
{Cum se detectează că procedura NU a fost urmată?}
- {Check 1}
- {Check 2}
- Runner: {cine execută verificările — script, audit, manual}

### CONNECT
{La ce se leagă procedura? Editează procesele din WHERE să o referențieze.}
- {Regula X} → {cum se leagă}
- {Procedura Y} → {cum se leagă}
- {Script Z} → {cum se leagă}
- `procedure-health.json` → {adaugă entry}

### VERIFY (procedural checkpoint — emis ca VK în sesiune)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii din §2)
- [ ] Output-ul satisface criteriile din §1? (problema rezolvată)
- [ ] VK emis în sesiune? (ambele linii de mai jos vizibile pentru Pafi)
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per procedură FORGE** (per VK-H-001):
1. **Execution VK** (confirmă pașii urmați):
   `✅ [PROC] FORGE | §1✓ §2✓ §3✓ §4✓ §4c✓ VER✓ | complete`
2. **Save VK** (confirmă salvarea în Cortex):
   `✅ [CORTEX] "{titlu}" | FORGE ✓ | rule: {RULE-ID} | v{X.Y}`

**§4c emite un VK SEPARAT** (al treilea, doar când §4c se aplică):
3. **Lobster VK** (din §4c, doar pentru skills/agents V2):
   `✅ [LOBSTER] L1:{N gates} L2:{N checkpoints} L3:{N contracts} | or SKIP (reason)`
   Dacă §4c nu se aplică (procedură fără acțiuni riscante, fără >3 pași, fără JSON inter-agent) → Lobster VK = SKIP cu motiv.

### MODEL ROUTING (cine execută ce pe această procedură)
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Creare procedură nouă | Sonnet 4.6 (Genie orchestrator) | FORGE template + Post-H gate asigură structura |
| Remediere backlog (enforcement loops lipsă) | Opus 4.6 subagent | Adăugare enforcement loops cere reasoning profund + context multi-fișier |
| Audit periodic (AUDIT-PRO) | Opus 4.6 subagent | Review structural 100+ items — Sonnet ratează gap-uri critice (DEV-H-010) |
| Validare API | Deterministic (forge.ts) | Nu e LLM — e cod |

**Referință**: COST-H-001 + `memory/rules/model-routing-table.md` — model IDs: `claude-sonnet-5` / `claude-opus-4-8`

---

## 4b. Note pentru Proceduri care Generează Prompturi

**Când procedura TA produce prompturi sau rulează în producție** (ex: proceduri PE, agentic pipelines, batch processing):

- **Context Engineering (C-071)**: Dacă procedura gestionează context window (system prompt + memory + tools + history) → referențiază C-071. Proiectează granița cache stabil/dinamic.
- **Promptware Lifecycle (C-073)**: Dacă prompturile produse de procedură vor fi deployate în producție (API calls repetate, batch processing) → referențiază C-073: versioning, test suite (3-5 cazuri), Promptfoo eval gate, rollback criterion.
- **Master decision point**: Orice task de prompting → **PROMPTING.md** este entry point-ul unificat (`~/.nexus/procedures/PROMPTING.md`). Procedura ta nu trebuie să reimplementeze PromptForge — referențiează PROMPTING.md.

**Când NU e necesar**: proceduri operaționale standard (sync, deploy, audit, onboarding) care nu produc prompturi → ignoră această secțiune.

---

## 4c. Lobster Pattern Integration (obligatoriu pentru skills/agents V2)

> **V2** = orice skill/agent care folosește infrastructura shared din `~/.nexus/v2/` (shared-skills, contracts, heartbeat). Dacă skill-ul tău nu referențiază `~/.nexus/v2/`, evaluează totuși decision tree-ul de mai jos. Patterns-urile L1-L3 sunt utile independent de V2.

**Când procedura/skill-ul TA execută acțiuni cu risc** (cost, modificare externă, restart servicii, dispatch D4, deploy, tranzacții) **sau are mai mult de 3 pași secvențiali**:

### L1: Structural Approval Gates (`approval-gate.sh`)

**Obligatoriu când**:
- Acțiune cu cost > $5 sau buget ce depășește pragul agentului
- Modificare externă (deploy VPS, write Notion/Sheets, send Telegram in bulk, publish)
- Restart/kill process cu PID activ
- Orice acțiune ireversibilă listată în iron-laws.md al agentului

**Cum integrezi**:
1. Identifică acțiunile din §2 care sunt ireversibile sau costisitoare
2. Adaugă `approval-gate.sh <task_id> <step_id> "<reason>"` ÎNAINTE de acțiune
3. Documentează gated actions în iron-laws.md (format IL-N: Structural Gates)
4. Testează: gate blochează? Telegram notifică? Approve deblochează? Deny oprește?

**Exemple concrete**:

GENIE (IL-8):
```
Gated: dispatch-task (budget >$5) → approval-gate.sh halts dispatch
Gated: approve-delivery (score <80) → approval-gate.sh halts delivery
```

SENTINEL (IL-8):
```
Gated: restart-service (PID alive) → approval-gate.sh halts restart until confirmed
Gated: clear-stale-locks (suspicious pattern) → approval-gate.sh halts cleanup
```

DELPHI:
```
Gated: D4 dispatch (expensive research) → approval-gate.sh halts research launch
Gated: VPS deploy (write to production) → approval-gate.sh halts deploy
```

**Nu e necesar** pentru: acțiuni locale (file write, git commit local), read-only operations, taskuri sub $1.

### L2: Step Checkpointing (`checkpoint.sh`)

**Obligatoriu când**:
- Skill-ul/procedura are >3 pași secvențiali executați de un agent
- Orice pipeline care poate crasha mid-execution (API calls, multi-file builds)
- Heartbeat scripts (fiecare check = checkpoint)

**Cum integrezi**:
1. La începutul fiecărui pas: `checkpoint.sh write PROGRESS.md "<step_name>"`
2. Opțional: `--output '{"key":"val"}'` pentru a salva output intermediar
3. La resume: `checkpoint.sh done PROGRESS.md "<step_name>"` decide dacă skip
4. La final: checkpoint.sh clear (opțional, dacă vrei cleanup)

**Exemplu write** (din GENIE heartbeat-v2.sh):
```bash
checkpoint.sh write "$PROGRESS" "validate-config"
# ... execute step ...
checkpoint.sh write "$PROGRESS" "check-agents" --output '{"alive":8,"dead":0}'
```

**Exemplu resume** (la re-run after crash):
```bash
# checkpoint.sh done returnează "SKIP" dacă step-ul e deja complet
STATUS=$(checkpoint.sh done "$PROGRESS" "validate-config")
if [ "$STATUS" = "SKIP" ]; then
  echo "Step already done, skipping" >&2
else
  # ... execute step ...
fi
```

**Nu e necesar** pentru: skills cu 1-2 pași atomici, read-only queries, one-shot operations.

### L3: Typed JSON Envelopes (`validate-contract.sh`)

**Obligatoriu când**:
- Skill-ul produce output structurat consumat de alt agent/skill
- Skill-ul primește input de la alt agent (dispatch, progress, delivery)
- Orice schimb inter-agent cu mai mult de 2 câmpuri

**Cum integrezi**:
1. Definește contractul (required fields + enums + cross-field rules) în `validate-contract.sh` sau într-un fișier `contracts.md` local
2. Validează input la intrare: `validate-contract.sh <contract> "$INPUT_JSON"`
3. Validează output la ieșire: `validate-contract.sh <contract> "$OUTPUT_JSON"`
4. Testează: invalid input respins? Missing fields detectate? Enum invalid raportat?

**Contracte existente** (refolosește, nu duplica):
`router-output`, `dispatch`, `progress`, `delivery`, `subagent-routing`, `sentinel-notify`, `checkpoint`, `approval-gate`, `sentinel-health`, `sentinel-triage`, `delphi-scout`, `delphi-critic`, `delphi-synthesis`

**Unde sunt definite**: Contractele sunt hardcoded ca case-uri in `~/.nexus/v2/shared-skills/validate-contract.sh`. Fiecare contract are: required fields, enum validations, cross-field rules. Pentru schema detaliată a unui contract: `validate-contract.sh --show <contract_name>` sau citește sursa direct. Plugin-specific contracts pot fi definite in `contracts.md` local in directorul pluginului.

**Nu e necesar** pentru: skills care produc doar text/markdown, operații fără inter-agent handoff.

### Lobster Decision Tree (include in §2 al procedurii noi)

```
Procedura/skill-ul tău:
  │
  ├─ Are acțiuni ireversibile sau costisitoare?
  │   YES → Adaugă L1 (approval-gate.sh) pentru fiecare acțiune riscantă
  │   NO  → Skip L1
  │
  ├─ Are >3 pași secvențiali?
  │   YES → Adaugă L2 (checkpoint.sh) la fiecare pas
  │   NO  → Skip L2
  │
  └─ Produce/consumă JSON structurat inter-agent?
      YES → Adaugă L3 (validate-contract.sh) la boundaries
      NO  → Skip L3

  IMPORTANT: Evaluează TOATE cele 3 ramuri independent.
  Dacă procedura are și acțiuni costisitoare ȘI >3 pași ȘI JSON inter-agent → aplică L1+L2+L3.
  Nu te opri la prima potrivire.
```

**VK obligatoriu** (al treilea VK, separat de Execution VK din §4):
`✅ [LOBSTER] L1:{N gates} L2:{N checkpoints} L3:{N contracts} | or SKIP (reason)`

**Referință**: `~/.nexus/v2/shared-skills/` (approval-gate.sh, checkpoint.sh, validate-contract.sh)

---

## 5. Dependențe

**FORGEBUILD's own dependencies** (practice what we preach):

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `test-all-procedures.js` | Daily FORGE compliance scanner | `~/.nexus/scripts/test-all-procedures.js` |
| `forge.ts` | API gate — rejects procedures without FORGE metadata (HTTP 422) | `~/.nexus/api/forge.ts` |
| `procedure-health.json` | Registry of all procedures with health status | `~/.nexus/procedures/procedure-health.json` |
| `rules-hard.md` | META-H-002 rule definition (FORGE enforcement) | `~/.claude/projects/-Users-pafi/memory/rules/rules-hard.md` |
| `PROMPTING.md` | Master entry point for prompt optimization | `~/.nexus/procedures/PROMPTING.md` |
| Cortex API | Logging audit results + procedure metadata | `localhost:6400/api/store` |
| AUDIT-PRO | Audit plugin for procedure quality scoring | `~/.nexus/audit/AUDIT-PRO.md` |
| `approval-gate.sh` | L1: Structural approval gates for risky actions (§4c) | `~/.nexus/v2/shared-skills/approval-gate.sh` |
| `checkpoint.sh` | L2: Step checkpointing for multi-step pipelines (§4c) | `~/.nexus/v2/shared-skills/checkpoint.sh` |
| `validate-contract.sh` | L3: Typed JSON envelope validation (§4c) | `~/.nexus/v2/shared-skills/validate-contract.sh` |

**Template for new procedures** (copy this table, replace entries):

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| {Componentă 1} | {Ce face} | {Path sau URL} |
| {Componentă 2} | {Ce face} | {Path sau URL} |

---

## 6. Metrics (opțional)

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| {Metrică 1} | {Descriere} | {Valoare țintă} |
| {Metrică 2} | {Descriere} | {Valoare țintă} |

---

## §8 Conversion Annex — Procedure to Skill/Plugin

> **Note**: §8 appears after §7 in the file but was merged as a self-contained annex. Reading order for NEW procedures: §0 > §0.5 > §1-§4c > §5-§7 (template). §8 is a reference annex consulted only when converting procedures to skills/plugins.

> **Source**: Extracted from `PROCEDURE-TO-SKILL.md` v1.0 (2026-03-20). This annex covers the standardized process for converting any procedure into a skill, CLI wrapper, embedded logic, or plugin component.

### 8.1 Decision Tree — What conversion type?

```
Procedure → Is it executable (has concrete steps with input/output)?
  │
  NO → REFERENCE ONLY
  │     Add to an existing SKILL.md "References" section.
  │
  YES → How complex is the logic?
    │
    ├─ 1-3 clear steps, defined I/O        → SKILL (standalone SKILL.md)
    ├─ Shell/Python script wrapper           → CLI (script + optional SKILL.md)
    ├─ Small logic, fits existing skill      → EMBEDDED (integrate, don't duplicate)
    └─ Multiple related procedures           → PLUGIN (bundle of skills + commands)
```

Before converting, always check:
1. `ls ~/.claude/plugins/` — existing plugins
2. `cortex_search "skill:<name>"` — Cortex registry
3. `grep -r "<name>" ~/.claude/plugins/*/skills/*/SKILL.md` — existing references
4. If similar exists → EMBEDDED or REFERENCE ONLY (no duplicates)

### 8.2 Phase 1: ASSESS

Classify via decision tree above. Check consumer scope:
- Multiple agents → shared skill in `~/.claude/plugins/{plugin}/skills/`
- Single agent → embed in that agent's skill
- Standalone tool for Pafi → CLI wrapper with `--help`

### 8.3 Phase 2: DECOMPOSE

Extract contracts from the procedure:

| Component | Question | Maps to SKILL.md section |
|---|---|---|
| Input | What does it receive? | `## Input` JSON schema |
| Output | What does it produce? | `## Output` JSON schema |
| Tools | What MCP tools/APIs needed? | `## Execution` tool table |
| Errors | What can go wrong? | `## Error Handling` |
| Boundaries | What it does / does NOT do | `## What You Do` / `## What You Do NOT Do` |

Classify each step as: execution → `## Execution`, validation → `## Input Validation`, routing → orchestrator logic, reference → `## References`.

### 8.4 Phase 3: CONVERT

**SKILL path**: Create `~/.claude/plugins/{plugin}/skills/{name}/SKILL.md` with YAML frontmatter + required sections (What You Do, What You Do NOT Do, Input, Input Validation, Execution, Output, Error Handling). Reference original: `Source: ~/.nexus/procedures/{original}.md (CONVERTED)`.

**CLI path**: Script with `set -euo pipefail`, arg parsing, `--help`, JSON output on stdout, JSON errors on stderr. Exit 0/1.

**When to bundle scripts** (vs letting the agent generate code):
- Operation is deterministic (validation, formatting, API calls with fixed structure)
- Same code would be generated repeatedly across sessions
- Errors need explicit handling that the agent might skip
Scripts save tokens and improve reliability vs agent-generated code.

**EMBEDDED path**: Add steps to existing SKILL.md `## Execution`. Add source comment `<!-- Source: {proc} vX.Y, embedded YYYY-MM-DD -->`. If embedding adds >50 lines → reconsider standalone.

**PLUGIN path**: Full directory structure (`skills/`, `commands/`, `agents/`, `hooks/`, `resources/`). Each skill follows SKILL path, each CLI follows CLI path. Write `plugin.json` manifest.

### 8.5 Phase 4: AUDIT

| Check | Tool | Min Score | Applies to |
|---|---|---|---|
| Skill format | Skill Creator 3.0 | >= 70 | SKILL.md |
| Prompt quality | PromptForge v3.6 | >= 70 | All prompts |
| Procedure structure | AUDIT-PRO STANDARD | >= 3.5/4.0 | FORGE procedures |
| CLI output | Manual test, real data | Valid JSON | CLI scripts |
| Contract compat | Compare I/O schemas | Exact match | All types |
| Intent preserved | Manual review | Goals met | All types |
| No duplication | grep across skills | Zero duplicates | All types |

### 8.6 Phase 5: INTEGRATE

1. Update agents that should reference the new skill
2. Update `plugin.json` manifest
3. Update `channel-config.yaml` if new search channel (DELPHI-type)
4. Run end-to-end integration test with real data
5. Archive original procedure with header: `Status: CONVERTED | Converted to: {path} | Date: YYYY-MM-DD | Type: SKILL|CLI|EMBEDDED|REFERENCE`

### 8.7 Quality Gates (summary)

| Gate | When | Blocker? |
|---|---|---|
| Duplication check passed | Phase 1 | Yes — no conversion if duplicate exists |
| I/O contracts extracted | Phase 2 | Yes — no conversion without contracts |
| Skill Creator score >= 70 | Phase 4 | Yes — rework until passing |
| PromptForge score >= 70 | Phase 4 | Yes — rework prompts |
| Integration test green | Phase 5 | Yes — do not archive original until passing |
| Original archived | Phase 5 | Yes — prevents ghost procedures |

### 8.8 Examples from DELPHI PRO

1. **X-EXTRACTION v1.1 → EMBEDDED**: 7-step Twitter pipeline overlapped with scout-social. Embedded as query template section (~30 lines). Only one consumer, no standalone skill needed.
2. **EPR modules → EMBEDDED in critic**: Python scoring (Evidence/Perspective/Reasoning) became the 5-dimension evaluation table in critic SKILL.md. LLM reasoning replaced Python calculation. Original epr.py archived.
3. **research.py → COMMAND + SKILL**: CLI script became `commands/research.md` (user-facing) + `agents/delphi.md` (LLM-native orchestration replacing Python script). CLI wrappers for scouts handle shell execution.

### 8.9 Anti-patterns

1. **Over-atomization** — Every procedure as a separate skill. Explosion of tiny, hard-to-discover skills. → Embed small procedures.
2. **Context loss** — Splitting one procedure across 5+ skills. Intent gets fragmented. → Keep cohesive logic together.
3. **CLI for everything** — Shell wrappers for things that should be MCP tools. → Use MCP for real-time interaction.
4. **Ghost procedures** — Not archiving originals after conversion. Two versions, no canonical. → Always mark CONVERTED.
5. **Mega-skills** — Embedding too much into one SKILL.md (>200 lines). → Split at 200-line threshold.

**When splitting IS appropriate** (positive counterpart to 8.9.5):
- SKILL.md exceeds 250 lines
- Content has distinct domains that are independently useful (e.g., query templates vs execution logic)
- Advanced/rare features bloat the main file
Extract to `REFERENCE.md` in the same skill directory. Add explicit pointer in SKILL.md: `See [REFERENCE.md](REFERENCE.md) for detailed channel-specific query templates.`

6. **Blind conversion** — Converting without duplication check. Creates drifting duplicates. → Always run Phase 1.2 first.

### 8.10 Path E: Plugin Bundle — Complete Guide

> Plugins are the highest-level conversion unit. A plugin bundles multiple skills, agents, commands, hooks, hookify rules, procedures, and resources into a self-contained, portable package. Use when 3+ related procedures form a coherent domain.

#### E.1 Plugin Manifest (`plugin.json`)

Every plugin MUST have a manifest at `~/.claude/plugins/{plugin}/.claude-plugin/plugin.json`.

**Required fields**: `name`, `version`, `description`, `skills[]`, `agents[]`, `commands[]`, `hooks{}`, `procedures[]`, `resources{}`

**Optional fields**: `author`, `command_names[]`, `openclaw_compatible`, `exportable`

**Version**: Semantic versioning — `MAJOR.MINOR.PATCH` (1.0.0 for first release).

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "...",
  "author": "NexusOS / Pafi",
  "skills": ["skills/skill-a", "skills/skill-b"],
  "agents": ["agents/agent-name.md"],
  "commands": ["commands/cmd.md"],
  "command_names": ["/cmd"],
  "hooks": {
    "pre-action": "hooks/pre-action.sh",
    "post-action": "hooks/post-action.sh",
    "on-error": "hooks/on-error.sh"
  },
  "procedures": ["procedures/PROC.md"],
  "resources": {
    "state": "resources/state.json",
    "config": "resources/config.yaml"
  },
  "exportable": true,
  "openclaw_compatible": true
}
```

**Rules**:
- All paths in the manifest are relative to plugin root
- `exportable: true` means the plugin can be copied to another machine and still work
- Empty arrays are valid (e.g., `"agents": []` if plugin has no agents)
- `hooks` object uses named lifecycle keys (e.g., `"pre-research"`, `"on-error"`) — each maps to a single script path string
- `resources` is an object with named keys mapping to relative paths (not an array)
- `command_names[]` lists the user-facing slash commands (e.g., `["/research-pro"]`)

#### E.2 Command Creation Checklist

Commands are thin user-facing dispatchers. They parse input and route to agents/skills.

**File**: `commands/{name}.md` with YAML frontmatter.

```yaml
---
name: command-name
description: One-sentence description
user-invocable: true
---
```

**Requirements**:
- [ ] Trigger phrase defined — `/command-name` format, lowercase, no spaces, hyphen-separated
- [ ] Dispatches to a specific agent or skill (documented in body)
- [ ] Input parsing defined — what arguments, what formats accepted
- [ ] Output format defined — what the user sees on success
- [ ] Error handling covers: agent unavailable, timeout, zero results, invalid input
- [ ] Contains NO business logic — commands are dispatchers only
- [ ] Cross-reference: listed in `plugin.json` `commands[]`

**Anti-pattern**: Commands that contain scoring logic, API calls, or data transformation. Move that to a skill.

#### E.3 Hook Creation Checklist

Hooks are executable shell scripts that fire at lifecycle events.

**Types**:
- `pre-action` — validation, quota check, rate limiting (runs BEFORE the main action)
- `post-action` — save results, notify, update state (runs AFTER the main action)
- `error` — alert, fallback logic, cleanup (runs when main action FAILS)

**File**: `hooks/{event}-{purpose}.sh` (must be executable: `chmod +x`)

**Requirements**:
- [ ] Bash script with `#!/usr/bin/env bash` and `set -euo pipefail`
- [ ] Uses `resolve_key()` from `lib/resolve-key.sh` for ALL API keys — zero hardcoded keys
- [ ] Uses quoted heredocs for Python blocks (`<<'PYEOF'`) — security requirement
- [ ] Outputs to stderr (`>&2`) — NOT stdout — to avoid polluting tool output
- [ ] Exits 0 on success, non-zero on failure
- [ ] Is idempotent — safe to run twice with same input, same result
- [ ] Cross-reference: listed in `plugin.json` `hooks` object under the appropriate named key

**Template**:
```bash
#!/usr/bin/env bash
set -euo pipefail

# Source shared utilities
source "$(dirname "$0")/../lib/resolve-key.sh"

# All diagnostic output to stderr (use >&2 per line)
# Do NOT use `exec 2>&1 1>/dev/null` — it suppresses all stdout

# Hook logic here
API_KEY=$(resolve_key "SERVICE_NAME")

# Exit 0 = success, non-zero = failure (blocks pre-hooks, alerts on error-hooks)
exit 0
```

#### E.4 Hookify Rule Creation Checklist

Hookify rules are Claude Code hook configurations that enforce quality gates and iron laws.

**File**: `.claude/hookify.{rule-name}.local.md` with YAML frontmatter.

```yaml
---
name: rule-name
enabled: true
event: bash        # bash | file | stop | prompt
action: warn       # warn | block
---
```

**Events**:
- `bash` — fires on shell command execution (check what command runs)
- `file` — fires on file write/edit (check what gets written)
- `stop` — fires on session end (check if cleanup happened)
- `prompt` — fires on user input (check what was asked)

**Requirements**:
- [ ] Pattern is a valid regex, tested for false positives AND false negatives
- [ ] Each rule enforces a clear IRON LAW or quality gate (documented in body)
- [ ] Tested with minimum 2 positive matches (rule fires correctly)
- [ ] Tested with minimum 2 negative matches (rule does NOT fire incorrectly)
- [ ] Placement decided: global (`~/.claude/`) for cross-project, project (`.claude/`) for project-specific
- [ ] Does NOT conflict with existing hookify rules (check `ls ~/.claude/hookify.*.md .claude/hookify.*.md`)
- [ ] Cross-reference: counted in plugin documentation, enforces specific rule ID

**Anti-pattern**: Overly broad regex that fires on legitimate actions. Always test negative cases.

#### E.5 Plugin Assembly Checklist

Final assembly verification before a plugin is considered complete.

**Component Quality**:
- [ ] All skills pass Skill Creator 3.0 (score >= 70)
- [ ] All prompts pass PromptForge (score >= 70)
- [ ] All components pass AUDIT-PRO (score >= 3.5/4.0)

**Wiring**:
- [ ] Commands dispatch correctly to agents/skills (manual test each)
- [ ] Hooks fire at correct lifecycle events (test pre/post/error flows)
- [ ] Hookify rules don't conflict with existing hooks (cross-check)
- [ ] `plugin.json` references ALL components correctly (no orphans, no missing refs)

**Portability**:
- [ ] `.env.example` lists all required API keys with descriptions
- [ ] No hardcoded absolute paths — use `$HOME`, `~`, or relative paths
- [ ] `lib/resolve-key.sh` used for all API key resolution
- [ ] `state.json` initialized with correct schema (empty but valid)

**Testing**:
- [ ] Integration test D1: single skill invocation — correct output
- [ ] Integration test D2: full pipeline — command → agent → skills → output
- [ ] Integration test D3: error flow — hook fires, fallback works
- [ ] Export test: copy plugin directory to fresh location, verify it still works

#### E.6 Real Example: DELPHI PRO Plugin

The DELPHI PRO plugin is the canonical reference implementation.

**Component inventory**:
| Category | Count | Details |
|---|---|---|
| Scout skills | 9 | video, social, visual, web, knowledge, deep, finance, brand, domain |
| Infrastructure skills | 3 | critic, synthesizer, reporter |
| Store skills | 3 | store-cortex, store-notion, store-vault |
| Orchestrator agent | 1 | `agents/delphi.md` |
| Commands | 2 | `/research-pro`, `/research-pro-deep` |
| Hooks (lifecycle) | 3 | `pre-research.sh`, `post-research.sh`, `on-error.sh` |
| Hooks (quota) | 1 | `pre-research-quota.sh` |
| Hookify rules | 8 | critic-mandatory, dangerous-bash, delphi-html-report, forge-compliance, protect-credentials, quota-warning, sol-research-reminder, memory-protection |
| Procedures | 1 | `DELPHI-SOC.md` |
| Resources | 5+ | `state.json`, `channel-config.yaml`, `human-program.md`, `optimization-buffer.md`, `templates/` |

**Path**: `~/.claude/plugins/delphi/`

**Structure**:
```
~/.claude/plugins/delphi/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── scout-video/SKILL.md
│   ├── scout-social/SKILL.md
│   ├── ... (9 scouts + 3 infra + 3 store)
│   └── critic/SKILL.md
├── agents/
│   └── delphi.md
├── commands/
│   ├── research.md
│   └── research-deep.md
├── hooks/
│   ├── pre-research.sh
│   ├── pre-research-quota.sh
│   ├── post-research.sh
│   └── on-error.sh
├── lib/
│   └── resolve-key.sh
├── procedures/
│   └── DELPHI-SOC.md
├── resources/
│   ├── state.json
│   ├── channel-config.yaml
│   ├── human-program.md
│   ├── optimization-buffer.md
│   └── templates/
└── .env.example
```

Use this as the reference when building new plugins. Each component follows its respective checklist (E.1-E.5 above).

### E.7 OpenClaw Compatibility (opțional, când `openclaw_compatible: true`)

Când un plugin trebuie să ruleze și pe OpenClaw (MacIntel cu Richard Opus 4.6 + GPT-5.2), urmează acești pași:

**E.7.1 `openclaw.plugin.json`** — manifest separat la rădăcina pluginului.

Câmpuri obligatorii:
```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "...",
  "author": "NexusOS",
  "license": "MIT",
  "engines": { "openclaw": ">=0.9.0" },
  "configSchema": {
    "type": "object",
    "properties": {
      "api_key_name": { "type": "string", "description": "..." }
    },
    "required": ["..."]
  },
  "capabilities": {
    "skills": true,
    "commands": true,
    "hooks": false,
    "tools": false,
    "services": false
  },
  "bundle": { "type": "claude", "path": "." }
}
```

Reguli:
- `configSchema` definește TOATE API key-urile și configurările necesare (JSON Schema)
- `capabilities.hooks` = `false` (OpenClaw nu are lifecycle hooks)
- `engines.openclaw` = versiunea minimă OpenClaw necesară

**E.7.2 `openclaw-wrapper.md`** — procedură adapter.

Secțiuni obligatorii:
1. **Translation table** — fiecare component Claude Code → echivalent OpenClaw:
   - Agenți paraleli → Richard execuție secvențială
   - Stop hooks → loop manual (Richard auditează, GPT-5.2 fixează, Richard re-auditează)
   - Agenți Sonnet → GPT-5.2
   - Auto-triggers (LaunchAgent) → trigger manual only
   - `~/.nexus/` paths → `${PLUGIN_BASE_DIR}` (env var, cu default `~/.openclaw/{plugin}/`)
2. **Setup** — cum se copiază resursele de pe MacM4 → MacIntel (`scp` commands)
3. **Sanitize** — payload sanitization obligatorie ÎNAINTE de orice dispatch la GPT-5.2
4. **Limitations table** — diferențe permanente vs versiunea Claude Code + workarounds
5. **Model routing** — Richard = primary (verdict/logic), GPT-5.2 = scoring/fixes

Template location: `~/.nexus/audit/openclaw-wrapper.md` (Audit Pro = referință canonică).

**E.7.3 Portability rules** — obligatorii când `openclaw_compatible: true`:
- Toate path-urile folosesc `$HOME` sau `${PLUGIN_BASE_DIR}`, niciodată absolute
- API keys prin `resolve_key()` sau `configSchema`, niciodată hardcoded
- Zero dependență de `~/.nexus/` (fișierele necesare se copiază în plugin)
- Zero dependență de LaunchAgents (document fallback manual)
- Zero dependență de Claude Code hooks (document alternativă manuală)

**E.7.4 Quality gates suplimentare**:
- [ ] `openclaw.plugin.json` validează conform JSON Schema
- [ ] Translation table acoperă TOATE componentele pluginului (zero gaps)
- [ ] Wrapper testat pe MacIntel cu Richard (smoke test manual)
- [ ] `${PLUGIN_BASE_DIR}` funcționează cu valoarea default

Referințe canonice:
- `~/.claude/plugins/delphi/openclaw.plugin.json` — manifest exemplu
- `~/.nexus/audit/openclaw-wrapper.md` — wrapper exemplu (Audit Pro)

---

## §7 Checklist Pre-Publicare (verifică ÎNAINTE de a marca ACTIVE)

- [ ] Regulă asociată există în rules-hard.md sau rules-standard.md
- [ ] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [ ] VERIFY checkpoint prezent cu cele 3 checks obligatorii
- [ ] Procesul din WHERE a fost editat să referențieze această procedură
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata: `rule_id`, `type: procedure`, `has_enforcement_loop: true`, `forge_version: "1.9"` (versiunea curentă FORGE)
- [ ] Dacă `openclaw_compatible: true` → `openclaw.plugin.json` + `openclaw-wrapper.md` prezente și verificate (E.7)
- [ ] Descrie CE nu CUM (zero cod inline >10 linii)
- [ ] VK format specificat (per VK-H-001)
- [ ] Test cases documentate — minim 1 normal flow (opțional dar recomandat, obligatoriu pentru skills)
- [ ] No time-sensitive information (dates, versions, "current" references that will become stale)

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-02-27 | Versiune inițială |
| 1.1 | 2026-02-27 | Secțiunea §4 Enforcement Loop adăugată (WHERE/WHEN/HOW/CONNECT/VERIFY). Checklist Pre-Publicare inițial. |
| 1.2 | 2026-02-27 | §0 SKILL-SEARCH Gate adăugat (căutare Tier 0-2 înainte de construcție, VK obligatoriu). |
| 1.3 | 2026-02-27 | MODEL ROUTING table adăugat. §5 Dependențe + §6 Metrics adăugate în template. |
| **1.4** | **2026-03-06** | FORGE-AUDIT DEEP (QUAL-H-004): §0 SKILL-SEARCH paths actualizate (openclaw→plugins/genie-training), SKILL.md format standard adăugat complet (YAML frontmatter + 4 secțiuni obligatorii), MODEL ROUTING specificat cu versiuni exacte (Sonnet 4.6/Opus 4.6), §3 Cortex collection guidance adăugat (procedures vs technical), §4b Context Engineering/Promptware note adăugat, referință PROMPTING.md ca master entry point, `forge_version` actualizat la 1.4 în checklist. |
| 1.5 | 2026-03-20 | §8 Conversion Annex merged from PROCEDURE-TO-SKILL.md v1.0: decision tree (SKILL/CLI/EMBEDDED/REFERENCE/PLUGIN), 5-phase pipeline (ASSESS→DECOMPOSE→CONVERT→AUDIT→INTEGRATE), quality gates table, 3 DELPHI PRO examples, 6 anti-patterns. `forge_version` updated to 1.5. |
| **1.6** | **2026-03-20** | §8 Plugin Bundle complete: E.1 plugin.json manifest spec, E.2 command creation checklist, E.3 hook creation checklist (resolve_key, idempotent, stderr), E.4 hookify rule creation checklist (events, regex testing, iron law), E.5 plugin assembly checklist (quality gates + portability + D1/D2/D3 tests), E.6 DELPHI PRO canonical reference (full component inventory + directory structure). `forge_version` updated to 1.6. |
| **1.7** | **2026-03-20** | 4 targeted additions: (1) "Why description matters" guidance in SKILL.md Format Standard — description is the only thing orchestrator sees for skill selection. (2) "When to bundle scripts" criteria in §8.4 CONVERT — deterministic ops, repeated code, explicit error handling. (3) "When splitting IS appropriate" positive guidance after anti-pattern 8.9.5 — 250-line threshold, distinct domains, REFERENCE.md extraction pattern. (4) "No time-sensitive information" check added to §7 Pre-Publication Checklist. `forge_version` updated to 1.7. |
| **1.8** | **2026-03-21** | E.7 OpenClaw Compatibility section added: `openclaw.plugin.json` manifest spec (configSchema, engines, capabilities), `openclaw-wrapper.md` adapter procedure template (translation table, sanitize, limitations, model routing), portability rules (${PLUGIN_BASE_DIR}, zero hardcoded paths, zero ~/.nexus/ dependency), 4 quality gate checks. §7 checklist updated with OpenClaw verification. Canonical references: Delphi Pro manifest + Audit Pro wrapper. `forge_version` updated to 1.8. |
| **1.9** | **2026-03-21** | §0.5 COMPONENT-REUSE Scan added: obligatoriu când §0 SKILL-SEARCH = NOT_FOUND. 3-step scan (plugin skills, Cortex components, agent capabilities) → REUSE TABLE. Canonical reusable components listed (Delphi scouts, stores, critic, reporter + Audit Pro). VK obligatoriu. Anti-pattern: no duplication of existing components. `forge_version` updated to 1.9. |
| **2.0** | **2026-03-28** | §4c Lobster Pattern Integration added: L1 Structural Approval Gates (approval-gate.sh), L2 Step Checkpointing (checkpoint.sh), L3 Typed JSON Envelopes (validate-contract.sh). Decision tree for when to apply each pattern. VK obligatoriu (`[LOBSTER]`). Examples from GENIE/SENTINEL/DELPHI. Clarified VK emission: 3 VKs when §4c applies (Execution + Save + Lobster). Compound case note in decision tree. Contract definition locations documented. Resume path example for L2. §8 ordering note added. AUDIT-PRO R1: 3.56 CONDITIONAL, converged to PASS after fixes. `forge_version` bumped to 2.0 (major: new mandatory section for V2). |
