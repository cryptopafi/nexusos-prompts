---
type: procedure
created: 2026-03-22
status: active
slug: wish-handoff
tags: [procedure, nexus]
---

# WISH-HANDOFF v1.1 — Hand-off Execution Procedure

> **NOTE**: This is the WISH Step H task execution/delegation procedure (flux selection, Codex briefs, subagent delegation).
> This is NOT the `/handoff` session command (save state, compact memory, git sync).
> For the session handoff skill, see: `~/.claude/plugins/genie-process/skills/handoff/SKILL.md`

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: WISH-H-002
**Scope**: Procedura completă pentru WISH Step H (Hand-off) — selecția fluxului de execuție, delegare, monitorizare, și verificare post-livrare.

---

## 1. Problema

### Ce nu funcționează fără o procedură de Hand-off standardizată

1. **Flux selection ad-hoc** — fără criterii clare, Genie decide aleator dacă execută direct, deleagă la Codex, sau lansează subagent. Rezultat: task-uri complexe executate direct (lent, buggy) sau task-uri simple delegate (overhead irosit).
2. **Execuție serială manuală** — Genie execută secvențe mecanice (5+ comenzi Bash) pas cu pas, blocând conversația și irosind tokens, in loc să delege la subagent (incident 2026-02-28: install gh + auth + git init + create repo + push — 5 pași seriali executați manual).
3. **Debug spirals** — Genie investighează crash loops și bug-uri în cod sursă (.ts/.js/.py) în loc să brief Codex după prima constatare (incident 2026-02-28: 5+ încercări restart/wait/check logs pe relay 409 crash loop).
4. **Briefs malformate ignorate silențios** — header greșit (`### Tasks` in loc de `### Prompt pentru Codex`), status lipsă, template delivery conținând `[DONE]` — daemon-ul nu parsează, task-urile nu se execută (incident 2026-02-28 pt4: 6 task-uri ignorate).
5. **Claim complete fără verificare** — task delegat la subagent sau Codex, marcat DONE fără auditare (FM-1 din WISH procedure).
6. **Skills pierdute post-execuție** — fix-uri și descoperiri nelog-ate deoarece skill save amânat la "sync" sau "final" în loc de imediat (incident 2026-02-27: 3 din 5 skills nesalvate).
7. **PromptForge skip-uit pe delegări complexe** — briefs trimise fără optimizare, cu placeholders bracketed, rol nedefinit, context incomplet (incident 2026-02-28: subagent lansat cu `[bracketed instructions]`).
8. **Acțiuni distructive fără backup** — kill/rm/overwrite fără snapshot prealabil, fără reversibilitate.

### Situații acoperite

- Execuție directă Genie (Flux A): ops, config, comenzi simple <2 min
- Delegare full TECH (Flux B): TECH proiectează + implementează via Redis queue
- Delegare split (Flux C): Genie proiectează, TECH implementează via Redis queue
- Delegare subagent: task-uri mecanice, parallelizable
- TECH offline fallback: file-based dispatch (DISPATCH.md + PROGRESS.md direct)
- Post-handoff: audit livrări, skill extraction, learning loop

---

## 2. Procedura

### Pas 0: FLUX SELECTION — Arbore de decizie

Aplică acest arbore **secvențial** — prima condiție care matchează determină fluxul:

```
Task primit din Step S (sau direct din W dacă e execuție simplă)
│
├── E informație/întrebare simplă?
│   └── YES → Răspunde direct (nu e Hand-off)
│
├── E distructiv? (kill, rm -rf, overwrite, push to prod, delete)
│   └── YES → OPS-H-001 GATE (vezi Pas 0b) → apoi continuă selecția
│
├── Task < 2 min ȘI < 50 linii cod ȘI e ops/config (nu debugging)?
│   └── YES → **FLUX A** (Genie direct)
│
├── Task > 3 comenzi secvențiale fără judecată între pași?
│   └── YES → **SUBAGENT** (task mecanic — vezi Pas 0c)
│
├── Task implică debugging cod sursă (.ts/.js/.py)?
│   └── YES → **FLUX B** (TECH via Redis) — DEV-H-013 gate
│   └── Genie NU debug-uiește. Dispatch TECH cu tot contextul descoperit.
│
├── Design necesar ȘI Genie l-a produs la Step S?
│   └── YES → **FLUX C** (Genie design + TECH implementare via Redis)
│
├── Task > 50 linii ȘI/SAU > 2 min?
│   └── YES → **FLUX B** (TECH via Redis — design + implementare)
│
└── DEFAULT → **FLUX A** (Genie direct)
```

**Emit VK obligatoriu**: `✅ [WISH-H] flux:{A|B|C|SUBAGENT} | task:"{scurt}" | reason:"{de ce acest flux}"`

### Pas 0b: OPS-H-001 GATE — Acțiuni distructive

Dacă acțiunea e distructivă (kill, rm, overwrite, force push, delete):

1. **BACKUP** — snapshot/copy ÎNAINTE de execuție
2. **LOG** — scrie motivul acțiunii (ce, de ce, ce se poate restaura)
3. **REVERSIBILITATE** — asigură o cale de rollback (backup path, git reflog, snapshot ID)
4. **NU cere confirmare lui Pafi** — Genie acționează autonom (associate, nu employee)
5. **Excepții** — cere confirmare DOAR dacă:
   - Cost > 50K tokens opțional
   - Irreversibil absolut (delete production data, force push to main)
   - 2+ opțiuni echivalente pe care doar Pafi le poate alege

**Emit VK**: `✅ [OPS] destructive:{acțiune} | backup:{path} | reversible:{da/nu}`

### Pas 0c: SUBAGENT DELEGATION — Task-uri mecanice

**Trigger**: >3 comenzi secvențiale, fără decizie/judecată între pași.

Exemple: brew install + config, git init + commit + push, bulk file edits, ingest batch.

1. **Genie decide CE** — definește scopul, output-ul așteptat, criteriile de succes
2. **Aplică Pre-H PromptForge** (Pas 1 de mai jos) — clasifică SKIP/LIGHT/FULL
3. **Lansează subagent** cu instrucțiuni complete (nu bracketed placeholders)
4. **Review rezultatul** — verifică output, raportează lui Pafi
5. **NU claim DONE** până nu ai verificat efectiv output-ul

**Emit VK**: `✅ [SUBAGENT] task:"{ce}" | steps:{N} | verified:{da/nu}`

---

### Pas 1: PRE-H PromptForge Gate (PROMPT-H-003)

**Aplică ÎNAINTE de orice delegare** — Codex brief, Opus subagent, Delphi query.
**NU se aplică**: Flux A (execuție directă Genie), tool calls simple, conversație.

#### 1a. Clasificare

| Nivel | Trigger | Acțiune |
|---|---|---|
| **SKIP** | Conversational, lookup simplu, <50 chars | Nu aplica PromptForge |
| **LIGHT** | Task simplu, 1 tool call | Constraint anchoring + role definition silențios |
| **FULL** | Research D2+, audit, Codex brief, delegare Opus, decizie producție | Pipeline complet: Intent Diagnostic + Pre-send checklist |

#### 1b. FULL Pipeline — Faza 0: Intent Diagnostic (v3)

```
WHY:    De ce e nevoie acum? Ce e blocat fără asta?
        → Trebuie să menționeze un blocker concret sau timing reason
NOT:    Ce NU trebuie să facă prompt-ul? (scope boundaries)
        → Cel puțin 1 non-goal explicit
UNLOCK: Dacă funcționează perfect, ce acțiune nouă devine posibilă?
        → Trebuie să descrie un next step measurable
```

Dacă oricare e vag: 1 clarifying question max, apoi continuă.
Stochează tuple-ul WHY/NOT/UNLOCK persistent — scrie la `~/.genie/intent-diagnostic.json` (nu doar working memory, care se pierde la session end/compaction). Format: `{"task": "description", "why": "...", "not": "...", "unlock": "...", "timestamp": "ISO"}`. Post-H Learning Loop citește de aici.

#### 1c. FULL Pipeline — Faza 1: Pre-send checklist (self-check, invizibil)

- [ ] Persona: `Act as world-class` sau echivalent prezent?
- [ ] `## Approach` separat de `## Response Format`?
- [ ] `## Instructions` separat de `## Formatting Rules`?
- [ ] Bullet counts specificate per secțiune?
- [ ] Word count limit explicit?
- [ ] Cel puțin 1 threshold numeric in output requirements?
- [ ] Cel puțin 1 comandă/snippet concretă cerută?
- [ ] Interdicții hedging explicite?

Dacă 2+ din checklist lipsesc, rescrie prompt-ul ÎNAINTE de trimitere.

**Emit VK**:
- FULL: `✅ [PROMPT] target:{dest} | PF: FULL {N}/8 | intent:✓`
- FULL rescris: `🔧 [PROMPT] target:{dest} | PF: FULL rewritten {N}→8/8 | intent:✓`
- LIGHT: `✅ [PROMPT] target:{dest} | PF: LIGHT`
- SKIP: nu emite VK

---

### Pas 2: EXECUȚIE per Flux

#### Flux A — Execuție directă (Genie)

**Condiții**: task < 2 min, < 50 linii cod, ops/config (nu debugging).
**Ce face Genie**: execută direct, raportează rezultatul.
**Ce NU face Genie**: debugging cod sursă, investigare crash loops, fixuri in .ts/.js/.py.

Pași:
1. Verifică OPS-H-001 dacă acțiunea e distructivă (Pas 0b)
2. Execută task-ul
3. Verifică rezultatul (output valid? erori?)
4. Raportează lui Pafi

**Emit VK**: `✅ [EXEC-A] task:"{ce}" | result:{success/fail} | duration:{Xs}`

#### Flux B — TECH design + implementare (via Redis)

**Condiții**: task > 50 linii, debugging, feature nou, multi-fișier.
**TECH proiectează ȘI implementează**. Genie dispatch via Redis queue.

Pași:
1. Construiește payload JSON — format TECH-COMM-001 (vezi Pas 2f)
2. Push la Redis: `python3 ~/.nexus/scripts/nexus-queue.py push --agent tech --task-id {task_id} --payload '{json}'`
3. Persistează dispatch: scrie `{task_id, goal, dispatched_at}` în `~/.nexus/workspace/intel/.tech-pending-dispatches.json`
4. Anunță Pafi: `🔧 Task dispatched → TECH | goal: "{goal}" | task_id: {task_id} | ETA: ~5-30min`
5. **Lansează background completion poller** — Agent tool cu `run_in_background: true`:
   ```
   Agent(run_in_background=true, name="tech-watch-{task_id}"):
     "Poll ~/.nexus/workspace/active/{task_id}/PROGRESS.md every 30s.
      When status changes from EXECUTING/CLAIMED to DONE/REVIEWING/FAILED/BLOCKED:
      - Read PROGRESS.md fully
      - Report back: status, output_location, notes, blocked_reason (if any)
      - If DONE/REVIEWING: read output.md or EXECUTION.log tail for summary
      Stop polling after 30 minutes (timeout)."
   ```
   Sesiunea primește notificarea automat când agentul termină — fără polling manual.
6. La completion (detect via background agent sau Monitor): audit AUDIT-PRO pe delivery

**Emit VK**: `✅ [EXEC-B] tech:{task_id} | status:DISPATCHED | sub_agent:{sub_agent_hint} | watcher:background`

#### Flux C — Genie design, TECH implementare (via Redis)

**Condiții**: design produs de Genie la Step S, auditat de Opus.
**Genie a proiectat, TECH implementează** design-ul primit.

Pași:
1. Confirmă design-ul e auditat de Opus (din Step S)
2. Construiește payload JSON DETALIAT — format TECH-COMM-001 (Pas 2f)
   - `spec` trebuie să includă: design complet, specificații I/O, error paths, criterii de succes
   - `sub_agent_hint`: `builder` (feature nou) sau `integrator` (wiring sisteme)
3. Push la Redis + persistează dispatch context (ca Flux B pașii 2-5, inclusiv background completion poller)
   - `sub_agent_hint` tipic: `builder` (feature nou) sau `integrator` (wiring sisteme)
4. La completion: audit AUDIT-PRO — verifică implementarea contra design-ului

**Diferență vs Flux B**: `spec` e mai detaliat (design inclus), audit-ul verifică conformitate cu design-ul.

**Emit VK**: `✅ [EXEC-C] tech:{task_id} | design:audited | sub_agent:{sub_agent_hint}`

#### Schema obligatorie de delivery (Flux A/B/C)

Orice payload de handoff/delivery trebuie să includă explicit aceste câmpuri:

```json
{
  "handoff_prompt": "...",
  "why": "...",
  "confidence": 0.0,
  "confidence_flags": []
}
```

- `handoff_prompt`: promptul exact trimis subagentului/Codex.
- `why`: justificarea deciziei (de ce acest flux și această acțiune).
- `confidence`: scor numeric 0.0-1.0.
- `confidence_flags`: listă de riscuri/limitări (ex. `low_context`, `ambiguous_intent`, `high_risk`).

#### Flux SUBAGENT — Task-uri mecanice

Pași:
1. Definește scopul + output așteptat + criterii succes
2. Lansează subagent cu instrucțiuni PromptForge-compliant (Pas 1)
3. Monitorizează (nu bloca conversația)
4. Review output — verifică efectiv, nu claim blind
5. Raportează rezultatul lui Pafi

**Emit VK**: `✅ [EXEC-SUB] task:"{ce}" | steps:{N} | verified:{da/nu}`

#### Fallback — TECH Redis down

Dacă Redis nu răspunde (detectare 2-step: `~/.nexus/config/redis-queue-enabled` absent SAU `python3 ~/.nexus/scripts/nexus-queue.py health` returnează DOWN):

| Condiție | Acțiune |
|---|---|
| Task < 50 linii | Genie execută direct (temporar Flux A) |
| Task > 50 linii | Scrie DISPATCH.md + PROGRESS.md direct în `~/.nexus/workspace/active/{task_id}/`; GENIE heartbeat Step 2.5 va invoca `nexus-agent-execute.sh` la next cycle |
| Redis revine | Watcher preia automat orice task DISPATCHED rămas |

**Emit VK**: `⚠️ [TECH-REDIS-DOWN] task:"{ce}" | action:{direct/file-dispatch}`

---

### Pas 2e: CODEX BRIEF FORMAT — CODEX-COMM-001

**Format obligatoriu** (daemon-ul parsează strict):

```markdown
## Task m4-XXX: Titlu descriptiv

**Status**: PENDING
**Prioritate**: HIGH/MEDIUM/LOW
**Model**: gpt-5.3-codex
**handoff_prompt**: "<prompt exact trimis lui Codex>"
**why**: "<de ce a fost ales acest handoff>"
**confidence**: 0.0-1.0
**confidence_flags**: [low_context|ambiguous_intent|high_risk]

### Prompt pentru Codex

[Conținut brief complet:
- Context: ce există, unde, de ce
- Task: ce trebuie făcut (pași concreți)
- Output: format așteptat, fișiere create/modificate
- Criterii succes: cum se verifică că funcționează
- Constrângeri: ce NU trebuie făcut, limite]

### Delivery

Livrează rezultatul în acest format:
- Fișiere create/modificate: [lista]
- Test: [cum se testează]
- Status: DONE / BLOCKED cu motiv
```

**Checklist ÎNAINTE de submit** (CODEX-COMM-001 enforcement):

1. Header = `### Prompt pentru Codex` (NU `### Tasks`, NU `### Context`, NU alt header)
2. `**Status**: PENDING` (exact, cu bold markdown)
3. Template Delivery NU conține `[m4-XXX DONE]` — daemon-ul matchează ca "already delivered"
4. Verifică un brief funcțional anterior din `~/.codex/genie-to-codex.md` ca referință
5. Model specificat corect (gpt-5.3-codex default, gpt-5.3-codex-spark for speed, gpt-5.1-codex-mini for simple tasks)

**Post-submit — verificare in 2 pași**:
1. `grep -c "Status.*PENDING" ~/.codex/genie-to-codex.md` — confirmă task-ul e vizibil
2. După 1 ciclu daemon (~90s): verifică `~/.codex/codex-to-genie.md` PRIMUL (nu daemon.log)

---

### Pas 2f: TECH DISPATCH FORMAT — TECH-COMM-001

**Format obligatoriu** — JSON payload pentru `nexus-queue.py push --agent tech`:

```json
{
  "task_id": "tech-{YYYYMMDD}-{ts_ms}",
  "from": "genie",
  "goal": "<1-2 propoziții — ce trebuie să existe la final>",
  "spec": "<detalii complete: fișiere, pași, criterii de succes, constrângeri>",
  "complexity": "low|medium|high",
  "budget_usd": 2.00,
  "max_turns": 10,
  "priority": "normal|high|critical",
  "sub_agent_hint": "builder|fixer|integrator|pipeliner|direct|auto"
}
```

**Mapare `sub_agent_hint`**:

| Task tip | sub_agent_hint |
|----------|---------------|
| Feature nou / script nou (>50 linii) | `builder` |
| Aplicare findings audit / bug fix | `fixer` |
| Wiring între 2+ sisteme (API, subprocess) | `integrator` |
| Daemon / LaunchAgent / cron | `pipeliner` |
| Fix mic (<50 linii) sau config | `direct` |
| Neclar — lasă TECH să decidă | `auto` |

**Mapare `complexity`**:

| Estimare | complexity | budget_usd | max_turns |
|----------|-----------|------------|-----------|
| <50 linii | `low` | 0.5–1.0 | 5 |
| 50–200 linii | `medium` | 2.0 | 10 |
| >200 linii / multi-fișier | `high` | 5.0 | 15 |

**Comanda de dispatch**:
```bash
TASK_ID="tech-$(date +%Y%m%d)-$(date +%s%3N)"
python3 ~/.nexus/scripts/nexus-queue.py push \
  --agent tech \
  --task-id "$TASK_ID" \
  --payload '{"task_id":"'"$TASK_ID"'","from":"genie","goal":"...","spec":"...","complexity":"medium","budget_usd":2.0,"max_turns":10,"priority":"normal","sub_agent_hint":"auto"}'
```

**Tracking dispatch** — scrie IMEDIAT după push CONFIRMAT (push exit code 0):
```bash
# ~/.nexus/workspace/intel/.tech-pending-dispatches.json
# Format: array JSON, append entry
python3 -c "
import json, os, fcntl
f='$HOME/.nexus/workspace/intel/.tech-pending-dispatches.json'
entry={'task_id':'$TASK_ID','goal':'...','dispatched_at':'$(date -u +%Y-%m-%dT%H:%M:%SZ)'}
if os.path.exists(f):
    with open(f, 'r+') as fh:
        fcntl.flock(fh, fcntl.LOCK_EX)
        arr = json.load(fh)
        arr.append(entry)
        fh.seek(0); fh.truncate()
        json.dump(arr, fh, indent=2)
        fcntl.flock(fh, fcntl.LOCK_UN)
else:
    with open(f, 'w') as fh:
        fcntl.flock(fh, fcntl.LOCK_EX)
        json.dump([entry], fh, indent=2)
        fcntl.flock(fh, fcntl.LOCK_UN)
"
```

**Checklist ÎNAINTE de push** (TECH-COMM-001 enforcement):
1. `task_id` format: `tech-{YYYYMMDD}-{ts_ms}` (nu reutiliza ID-uri)
2. `goal` e o propoziție completă, nu un placeholder
3. `spec` include fișierele relevante cu căi absolute (`spec` = `task_description` flattened — TECH primește spec ca task description)
4. `sub_agent_hint` ales corect din tabelul de mai sus
5. `budget_usd` realist pentru `complexity`
6. Push Redis ÎNTÂI → dacă succes → scrie tracking file (evită orphan entries la push failure)

**Post-dispatch — verificare**:
```bash
python3 ~/.nexus/scripts/nexus-queue.py status | python3 -c "import sys,json; d=json.load(sys.stdin); print('tech queue:', d['queues']['tech'])"
# Expected: tech queue: 1 (sau 0 dacă watcher-ul a preluat deja)
```

---

### Pas 3: POST-H — Verificare și Skill Extraction

Rulează **după fiecare execuție** (oricare flux). Budget: 90 secunde.

**Non-trivial** = mai mult de un tool call, SAU output > 500 tokeni, SAU a implicat eroare/rework.
**Skip** dacă: lookup simplu, conversațional, fără lecție nouă.

#### 3a. Learning Loop (CONDITIONAL — doar dacă PF: FULL)

Compară rezultatul vs Intent Diagnostic din Pre-H:

| Criteriu | Check |
|---|---|
| WHY satisfied? | Blocker-ul menționat a fost deblocat? |
| NOT respected? | Non-goal-urile au fost respectate? |
| UNLOCK achieved? | Next step-ul measurable e posibil acum? |

Clasificare: `success` / `partial` / `failure`

Dacă `partial` sau `failure`:
- Root cause: too vague / too broad / conflicting constraints / missing context / wrong output spec
- Pattern repetat 2+ ori in 7 zile: promovează ca default guardrail
- Pattern success 3+ ori: promovează ca reusable template snippet

**Emit VK**: `✅ [PF-LEARN] "{target}" | WHY:{yes/no} NOT:{yes/no} UNLOCK:{yes/no} | {success/partial/failure}`
Dacă partial/failure: `⚠️ [PF-LEARN] "{target}" | {result} → root: {cause}`
Skip dacă LIGHT/SKIP (nu are Intent Diagnostic).

#### 3b. Surprise Check (CONDITIONAL — doar dacă rezultat neașteptat)

**Trigger**: rezultat neașteptat, corecție Pafi, near-miss, eroare surpriză.

Dacă trigger:
- Root cause in 2 propoziții max
- **Emit VK**: `⚠️ [SURPRISE] "{ce s-a întâmplat}" | root: {cause}`

Dacă NU e surprise: skip, continuă la 3c.

#### 3c. FORGE Compliance Check (ALWAYS — META-H-002)

ÎNAINTE de orice save in Cortex, verifică:

| Check | Ce verifică |
|---|---|
| §1 Problema | Textul conține descrierea problemei? |
| §2 Procedura | Pași numerotați? |
| §3 Cortex Logging | Template de logging? |
| §4 Enforcement Loop | WHERE + WHEN + HOW + CONNECT + VERIFY? |
| Metadata | `rule_id`, `has_enforcement_loop: true`, `forge_version`? |

Dacă NU: aplică FORGE template ÎNAINTE de curl.
**Bypass**: session_summary/note/audit-report: `⚠️ [CORTEX] "{title}" | FORGE bypass: {reason}`

#### 3d. Save + VK (ALWAYS — MEM-H-002)

**IMEDIAT** — nu la sync, nu la exit, nu in batches.

1. Salvează in Cortex (`procedures`, confidence=0.6, `last_applied`=azi)
2. Emit: `✅ [SKILL #N] "{title}" | saved`
3. Emit: `✅ [CORTEX] "{title}" | FORGE ✓ | rule: {X} | v{Y}`
4. Dacă FORGE aplicat la 3c: `🔧 [CORTEX] "{title}" | FORGE applied (was missing §{N})`
5. Dacă Cortex unreachable: save to `memory/procedures/pending-cortex-sync/` — retry path: `/sync` Step 1 checks this directory and retries all pending saves. `/handoff` Step 1 also checks.

#### 3e. Codex Brief Check (ALWAYS — DEV-H-014)

Am făcut un fix/debug in sesiunea curentă? Am scris un Codex brief corespunzător?

- Dacă fix/debug executat ȘI lipsă brief Codex: scrie brief ACUM
- Dacă fix/debug delegat corect la Codex: skip

#### 3f. Delivery Audit Gate (Flux B/C — DEV-H-010)

La primirea oricărei livrări Codex:

1. Citește delivery report din `~/.codex/codex-to-genie.md`
2. Verifică fișiere create/modificate (există? syntax OK?)
3. Verifică LaunchAgents (loaded?) dacă relevant
4. Spot-check conținut (head, grep key sections)
5. Lansează AUDIT-PRO (Opus subagent) pe livrare
6. Raportează: PASS / PASS with notes / FAIL
7. Dacă FAIL: brief Codex cu fix-urile necesare

**Emit VK**: `✅ [AUDIT-H] delivery:m4-{XXX} | verdict:{PASS/CONDITIONAL/FAIL} | score:{X}/4.0`

---

### Pas 4: MONITOR (permanent, pe toată sesiunea)

| Check | Frecvență | Cum |
|---|---|---|
| TECH result check | La fiecare răspuns dacă există dispatch pending | Citește `.tech-pending-dispatches.json` + PROGRESS.md per task_id |
| PromptForge session log | La /sync, /push, exit | Raport: `[PF session: N FULL (avg X/8), M LIGHT, K SKIP]` |

**TECH result monitoring** (dacă `.tech-pending-dispatches.json` există sau task_id în sesiune):

1. Citește `.tech-pending-dispatches.json` — extrage task_id-urile pending
2. Per task_id: citește `~/.nexus/workspace/active/{task_id}/PROGRESS.md` sau `completed/{task_id}/PROGRESS.md`
3. Status handling:
   - `REVIEWING` / `DONE` → anunță Pafi: `✅ TECH task complete | goal: "{goal}" | output: workspace/active/{task_id}/` → AUDIT-PRO pe delivery → elimină din pending
   - `FAILED` → anunță Pafi cu `blocked_reason` din PROGRESS.md → elimină din pending
   - `BLOCKED` → warn: `⏳ TECH task BLOCKED | task_id: {id} | reason: {reason}`
   - `DISPATCHED` / `EXECUTING` → skip (în lucru)
4. Dacă task_id absent din workspace și au trecut >60 min de la dispatch → warn: `⏳ TECH task >60min | poate verifica manual`

---

## Known Failure Modes

| # | Anti-pattern | Root cause | Fix in procedură |
|---|---|---|---|
| FM-H1 | Genie execută 5+ comenzi secvențiale manual | Nu a aplicat decision tree (>3 comenzi = subagent) | Pas 0 arbore + Pas 0c subagent delegation |
| FM-H2 | Genie debug-uiește crash loop (5+ retries) | DEV-H-013 nerespectată | Pas 0 arbore explicit: debugging = Flux B |
| FM-H3 | Brief Codex ignorat de daemon | Header greșit / status lipsă | Pas 2e format + checklist 5 puncte |
| FM-H4 | Task delegat, clamat DONE fără verificare | Lipsă audit step post-delegare | Pas 3f Delivery Audit Gate |
| FM-H5 | Skills pierdute post-execuție | Save amânat la sync/exit | Pas 3d IMEDIAT enforcement |
| FM-H6 | Brief subagent cu [bracketed placeholders] | PromptForge skip-uit | Pas 1 Pre-H gate obligatoriu |
| FM-H7 | Acțiune distructivă fără backup | OPS-H-001 neaplicat | Pas 0b gate explicit |
| FM-H8 | VK emis înainte de execuție | Cognitive shortcut | VK-uri plasate DUPĂ pașii de execuție |

---

## 3. Cortex Logging

La fiecare Hand-off completat, salvează:

```json
{
  "text": "[WISH-H] Flux {A/B/C/SUB} | task: {descriere} | result: {success/fail} | Pre-H: PF:{SKIP/LIGHT/FULL} | Post-H: skill={da/nu}, surprise={da/nu}, audit={verdict}",
  "handoff_prompt": "{prompt_trimisa_catre_executor}",
  "why": "{justificare_decizie}",
  "confidence": 0.0,
  "confidence_flags": [],
  "collection": "procedures",
  "metadata": {
    "type": "execution-log",
    "procedure": "WISH-HANDOFF",
    "rule_id": "WISH-H-002",
    "flux": "{A/B/C/SUBAGENT}",
    "promptforge_level": "{SKIP/LIGHT/FULL}",
    "result": "{success/fail}",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "forge_bypass": true,
    "forge_bypass_reason": "execution-log",
    "tags": ["wish", "handoff", "execution", "{flux-type}"]
  }
}
```

Per livrare Codex auditată (separat):

```json
{
  "text": "AUDIT: Codex delivery m4-{XXX} | score: {X}/4.0 | verdict: {PASS/CONDITIONAL/FAIL}. Findings: {lista}.",
  "collection": "procedures",
  "metadata": {
    "type": "audit-report",
    "procedure": "AUDIT-PRO",
    "rule_id": "QUAL-H-004",
    "audit_tier": "{LIGHT/STANDARD}",
    "audit_subject": "m4-{XXX}",
    "audit_score": "{X}",
    "audit_verdict": "{VERDICT}",
    "forge_bypass": true,
    "forge_bypass_reason": "audit-report",
    "tags": ["audit", "codex", "delivery"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — executat la FIECARE task care ajunge la Hand-off
- Post-H — executat automat după fiecare H completat
- Session Monitor — verificare continuă delivery flags
- Precision Mode Step 7 — checkpoint post-task când PM activ

### WHEN
- La fiecare task clasificat W care necesită execuție (nu doar informație)
- La fiecare brief Codex trimis
- La fiecare livrare Codex primită
- La fiecare /sync, /push, sau exit (PF session summary)

### HOW (violation detection)

| Violation | Cum se detectează | Severitate |
|---|---|---|
| Flux selection fără VK | Absența `✅ [WISH-H] flux:` in răspuns | HIGH |
| Genie execută >3 comenzi secvențiale fără subagent | Pattern: 4+ Bash calls fără delegare | HIGH |
| Genie debug-uiește cod sursă | Bash calls cu grep/sed pe .ts/.js/.py pentru debugging | HIGH (DEV-H-013) |
| Brief Codex fără CODEX-COMM-001 format | Missing `### Prompt pentru Codex` header | HIGH |
| Delegare fără PromptForge gate | Brief trimis fără `✅ [PROMPT]` VK | MEDIUM |
| Post-H skills nesalvate | Session end cu fix-uri nelog-ate | HIGH (MEM-H-002) |
| Delivery Codex neauditată | Delivery flag consumat fără AUDIT-PRO VK | HIGH (DEV-H-010) |
| Acțiune distructivă fără backup | Kill/rm/overwrite fără `✅ [OPS]` VK | HIGH (OPS-H-001) |
| Poll loop Codex | 2+ checks fără muncă utilă între ele | MEDIUM |
| VK emis înainte de execuție | VK timestamp anterior comenzii de execuție | HIGH |

**Runner**: self-check la fiecare răspuns + Precision Mode Step 7 audit + nightly FORGE procedure health check

### CONNECT

| Componentă | Relație | Path |
|---|---|---|
| **WISH procedure** | Pas H din pipeline-ul WISH complet | `memory/wish-procedure.md` |
| **PromptForge v2.1/v3** | Pre-H gate (PROMPT-H-003) | `memory/promptforge.md`, `memory/promptforge-v3.md` |
| **CODEX-QUEUE-MONITORING** | Monitoring Codex in Flux B/C | `~/.nexus/procedures/CODEX-QUEUE-MONITORING.md` |
| **AUDIT-PRO** | Audit livrări Codex (DEV-H-010) | `~/.nexus/audit/AUDIT-PRO.md` |
| **FORGE template** | FORGE compliance check (Post-H) | `~/.nexus/procedures/FORGEBUILD.md` |
| **Skill Saving** | MEM-H-002 save-as-you-go | `memory/skill-saving.md` |
| **OPS-H-001** | Gate acțiuni distructive | `memory/rules/rules-hard.md` |
| **DEV-H-013** | Nu debug-ui, deleagă la Codex | `memory/rules/rules-hard.md` |
| **DEV-H-010** | Dual Audit pe livrări Codex | `memory/rules/rules-hard.md` |
| **TECH-COMM-001** | Format dispatch TECH via Redis | WISH-HANDOFF.md Pas 2f |
| **nexus-queue.py** | Redis queue client (push/pop/ack/notify) | `~/.nexus/scripts/nexus-queue.py` |
| **tech-queue-watcher.sh** | BRPOP daemon TECH | `~/.nexus/agents/tech/scripts/tech-queue-watcher.sh` |
| **write-tech-task.py** | Payload writer DISPATCH+PROGRESS | `~/.nexus/agents/tech/scripts/write-tech-task.py` |
| `.tech-pending-dispatches.json` | Dispatch tracking (persistă cross-session) | `~/.nexus/workspace/intel/.tech-pending-dispatches.json` |
| `nexus-agent-execute.sh` | Task executor (claim + Claude CLI) | `~/.nexus/scripts/nexus-agent-execute.sh` |
| `procedure-health.json` | Registry proceduri | `~/.openclaw/procedure-health.json` |

### VERIFY (procedural checkpoint)

La finalul FIECĂRUI Hand-off, Genie verifică:

- [ ] Flux selection corect? (decision tree respectat)
- [ ] Pre-H PromptForge aplicat? (dacă delegare)
- [ ] Execuție completă? (task finished, nu partial)
- [ ] Post-H rulat? (skill save, FORGE check, surprise check)
- [ ] Delivery auditată? (dacă Flux B/C)
- [ ] VK-uri emise? (toate VK-urile obligatorii prezente)
- [ ] Dacă oricare = NU: procedura NU e completă, nu marca DONE

**VK-uri obligatorii per procedură FORGE** (per VK-H-001):

1. **Execution VK**: `✅ [PROC] WISH-HANDOFF | flux:{X} pre-H✓ exec✓ post-H✓ | complete`
2. **Save VK**: `✅ [CORTEX] "WISH-H execution log" | FORGE ✓ | rule: WISH-H-002 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Flux selection (Pas 0) | Genie (Sonnet) | Decision tree deterministă |
| Pre-H PromptForge (Pas 1) | Genie (Sonnet) | Prompt optimization = core Genie |
| Flux A execution (Pas 2) | Genie (Sonnet) | Direct ops/config |
| Flux B/C brief writing (Pas 2) | Genie (Sonnet) | Brief = orchestration |
| Codex implementation | Codex (GPT-5.2/5.3) | Code execution |
| Delivery audit (Pas 3f) | Opus subagent | Cross-file verification, bug detection |
| Post-H skill save (Pas 3d) | Genie (Sonnet) | Cortex API call |
| Subagent tasks (Pas 0c) | Per task type | Mechanical = any; deep = Opus |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|---|---|---|
| WISH procedure | Pipeline complet W-I-S-H | `memory/wish-procedure.md` |
| PromptForge v2.1 | Prompt optimization system | `memory/promptforge.md` |
| PromptForge v3 | Intent Diagnostic + Learning Loop | `memory/promptforge-v3.md` |
| FORGE template | Standard structure for procedures | `~/.nexus/procedures/FORGEBUILD.md` |
| AUDIT-PRO | Universal audit template | `~/.nexus/audit/AUDIT-PRO.md` |
| CODEX-QUEUE-MONITORING | Active waiting procedure | `~/.nexus/procedures/CODEX-QUEUE-MONITORING.md` |
| Cortex API | Knowledge base storage | `http://100.81.233.9:6400/api/store` |
| Codex daemon | Code execution queue | `~/.openclaw/scripts/codex-daemon.sh` |
| genie-to-codex.md | Codex brief queue | `~/.codex/genie-to-codex.md` |
| codex-to-genie.md | Codex delivery file | `~/.codex/codex-to-genie.md` |
| rules-hard.md | HARD rules reference | `memory/rules/rules-hard.md` |
| procedure-health.json | Procedure health registry | `~/.openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---|---|---|
| Flux selection accuracy | % task-uri cu flux corect (nu over/under-delegate) | >= 95% |
| Pre-H PromptForge coverage | % delegări cu PF gate aplicat | 100% |
| Brief format compliance | % briefs Codex cu CODEX-COMM-001 valid | 100% |
| Delivery audit coverage | % livrări Codex auditate cu AUDIT-PRO | 100% |
| Skill save rate | % skills detectate și salvate imediat | >= 95% |
| Mechanical task delegation | % task-uri >3 pași delegated la subagent | >= 90% |
| Debug delegation | % debugging tasks rutate la Codex (nu Genie direct) | 100% |
| Post-H completion | % Hand-off-uri cu Post-H complet (all steps) | >= 95% |
| Codex queue block time | Minute blocate de polling/waiting | 0 min |
| OPS-H-001 backup rate | % acțiuni distructive cu backup prealabil | 100% |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există: WISH-H-002
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu checks obligatorii
- [x] Procesul din WHERE referențiat (wish-procedure.md §H)
- [x] Entry pending in `procedure-health.json`
- [x] Salvat in Cortex cu metadata: `rule_id: WISH-H-002`, `type: procedure`, `has_enforcement_loop: true`, `forge_version: "1.3"`
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
- [x] Known Failure Modes documentate cu root cause si fix
- [x] Decision tree testabil (arbore determinist, nu ambiguu)
- [x] Toate reflexele din MEMORY.md integrate (CODEX-COMM-001, DEV-H-013, task-uri mecanice, PromptForge gate)

## Changelog

- **v1.1** (2026-03-17): H1 spec flattening doc, H2/H4 Redis 2-step detection, M3 push-before-track ordering, M5 sub_agent_hint in VK formats, Step 5 background completion poller, Flux C cross-ref update, tracking file locking with fcntl, descriptive notifications, watcher pre-created workspace support. WISH-H-002 rule created.
- **v1.0** (2026-03-15): Initial version.
