---
type: procedure
created: 2026-03-22
status: active
slug: precision-mode
tags: [procedure, nexus]
---

# PRECISION-MODE v1.0 — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-02-28
**Versiune**: 1.1
**Regulă asociată**: COM-H-001 (Session management — extinsă pentru strict execution mode)
**Scope**: Mod de lucru strict cu traceability completă — pentru task-uri unde o greșeală e costisitoare sau greu reversibilă.
**Research basis**: Chain-of-Verification (Dhuliawala et al., Meta AI 2023), Agent Behavioral Contracts (2025), Checklist Manifesto (Gawande 2009), Miller's Law / Cowan revision (2001), Checklist Fatigue (Grigg 2015), AgentSpec Runtime Enforcement (Poskitt et al. ICSE 2026).

---

## Ce este Precision Mode — ghid pentru utilizator

Precision Mode transformă Genie dintr-un executor rapid într-un **auditor riguros cu traceability completă**. Activează-l când o greșeală ar fi costisitoare sau greu reversibilă.

### Ce face diferit față de normal mode
- **Confirmă** ce ai cerut înainte de a executa (comprehension check)
- **Anunță** toți pașii planificați înainte de a începe (pre-announce)
- **Tracează** fiecare acțiune până la regulă (chain trace: acțiune → procedură → regulă)
- **Verifică** la fiecare pas, nu doar la final (full VK + post-task checkpoint)
- **Explică** de ce ia fiecare decizie, nu doar ce face (show reasoning)
- **Auditează** la final: coverage %, skills saved, patterns discovered (session audit)

### Când îl activezi
- Research D3+ (deep/exhaustive)
- Production audit (security, infrastructure)
- Architecture decisions (new system design)
- HARD rule creation/modification
- Procedure creation (FORGE)
- Orice task unde o greșeală e costisitoare sau greu reversibilă

### Cum îl activezi
- Scrie: `precision`, `strict`, sau `precision mode`
- Dezactivare: `normal`, `precision off`
- Genie poate sugera activare, dar NU activează singur

### Ce vei vedea
- `🔒 PRECISION` la începutul fiecărui răspuns
- Chain traces: `🔗 [acțiune] ← procedură.step ← regulă`
- Comprehension check cu confirmare înainte de execuție
- Post-task checkpoint după fiecare task complex
- Session audit la exit cu statistici coverage

---

## 1. Problema

### Ce nu funcționează fără Precision Mode

Modul normal de lucru al Genie are 5 probleme în task-uri cu risc mare:

1. **Execuție fără confirmare** — Genie interpretează cererea și execută imediat. Dacă interpretarea e greșită, munca e irosită. Impact: 10+ minute pierdute per misunderstanding.
2. **Traceability lipsă** — Acțiunile se execută fără a documenta ce regulă, procedură, sau decizie le justifică. La audit, nu se poate reconstitui "de ce s-a făcut X".
3. **VK-uri selective** — În normal mode, acțiunile triviale nu emit VK. Dar la task-uri critice, un skip poate masca o omisiune.
4. **Post-task review absent** — Se trece la task-ul următor fără a verifica dacă precedentul a fost complet. Skills nu se salvează consistent.
5. **Self-verification bias** (CoVe, Meta AI 2023) — Agentul care verifică propriul output confirmă propriile erori. Verificarea trebuie factorizată (independent de draft-ul original).
6. **Prompt optimization invisibility** — Prompt-urile trimise către subagents sau Cortex fără optimizare PromptForge sunt invizibile lui Pafi, făcând imposibilă auditarea calității prompt-urilor.

### Ce rezolvă Precision Mode

- BEFORE: Confirmare explicită + plan vizibil → zero misunderstandings
- DURING: Chain trace + full VK → auditabilitate 100%
- AFTER: Checkpoint + session audit → zero omisiuni, skills capturate
- Factored verification → self-verification bias eliminat

---

## 2. Procedura

### 2.1 Activare / Dezactivare

**Activare**: `precision`, `strict`, `precision mode`
**Dezactivare**: `normal`, `precision off`
**Indicator**: `🔒 PRECISION` la începutul fiecărui răspuns
**Auto-suggest** (NU auto-activa): D3+ research, production audits, architecture, HARD rule changes, procedure creation
**Checklist type**: READ-DO (citește step → execută → marchează) — per Gawande 2009

Sugestie format: `💡 Recomand PRECISION MODE pentru acest task (audit/architecture/etc). Activez?`

Emit VK: `🔒 [PRECISION] ON | task: "descriere" | procedures: WISH, FORGE, DARE`

**Persistence (compaction-safe)**:
- La activare → write `~/.claude/active-mode.md`:
  ```
  PRECISION
  activated: {date}
  task: {descriere scurtă}
  ```
- La dezactivare (`precision off` / `normal`) → clear `~/.claude/active-mode.md` (write empty string)
- **Why**: Context compaction pierde starea modului. Fișierul pe disk persistă. Session Start Checklist Step 1.5 restaurează modul automat.

### 2.2 Structura: 3 Faze (BEFORE / DURING / AFTER)

**Cognitive load** (Cowan 2001): 10 steps, dar chunked în 3 faze de câte 2-5 items.
**Checklist fatigue** (Grigg 2015): MODIFIER steps sunt behavioral overlays — nu necesită atenție separată.
**Design by Contract** (Agent Behavioral Contracts 2025): Fiecare ACTION step are precondition/postcondition explicit.

### 2.3 BEFORE — Pre-execuție (2 ACTION steps)

**Step 0: COMPREHENSION CHECK**
- **Precondition**: Pafi a dat o cerere
- **Postcondition**: Cererea e confirmată, PF clasificat
- Reformulează ce a cerut Pafi în 1-3 propoziții clare
- Identifică ambiguități sau interpretări multiple
- Clasificare PromptForge explicită — SKIP / LIGHT / FULL cu motivul:
  - `PF: SKIP — conversational, nu necesită optimizare`
  - `PF: LIGHT — task simplu, aplică constraint anchoring silențios`
  - `PF: FULL — research D2+/audit/brief, aplic Audit Prompt Pattern`
- **Pafi confirmă** sau **corectează** → abia apoi WISH.W
- Dacă instrucțiuni explicite + zero ambiguitate → check scurt, dar PF tot se anunță

Emit VK: `🔍 [COMPREHENSION] "reformulare" | PF: LEVEL (motiv) | ambiguities: N | confirm?`

**Step 1: PRE-ANNOUNCE**
- **Precondition**: Comprehension check confirmat
- **Postcondition**: Plan vizibil cu enforcement chain
- Listează TOȚI pașii planificați cu chain-ul complet
- Template:
```
🔒 PRECISION | Task: "{descriere}"
📋 Proceduri active:
  1. WISH (W→I→S→H→Post-H) ← COM-H-001
  2. [alte proceduri relevante] ← [rule]
📋 Pași planificați:
  1. [pas] ← [chain]
  2. [pas] ← [chain]
▶ Execuție:
```

### 2.4 DURING — Execuție (6 MODIFIER steps)

Aceste MODIFIERS nu sunt checklist items — sunt **threshold changes** active pe tot parcursul. Nu au VK propriu, schimbă comportamentul altor steps.

**Step 2: CHAIN TRACE** — Fiecare acțiune anunță enforcement chain-ul complet:
```
🔗 [acțiune] ← procedure.step ← rule_id
```

**Step 3: ZERO ASSUMPTIONS** — Cortex search obligatoriu pe ORICE task. Verificare explicită pe orice afirmație. (ext PROC-H-001)

**Step 4: FULL VK** — Orice acțiune emite VK, inclusiv cele care în normal mode ar fi SKIP. (ext VK-H-001)

**Step 5: ENTRY/EXIT VK** — `▶ [ENTERING] PROC_NAME` ... `✅ [PROC] PROC_NAME | complete` pe fiecare procedură. (ext VK-H-001)

**Step 6: SHOW REASONING** — La fiecare decizie, explică DE CE, nu doar CE. Behavioral overlay.

**Step 7: PROMPT VISIBILITY (HARD)** — Orice prompt trimis către subagent, Cortex query, sau tool call complex TREBUIE afișat vizibil lui Pafi DUPĂ optimizare PromptForge. Format obligatoriu:
```
📝 [PROMPT-VIS] target: {subagent|cortex|tool} | PF: {SKIP|LIGHT|FULL} | prompt: "{text complet sau rezumat}"
```
- **Neafișat = neoptimizat** — dacă Genie trimite un prompt fără `[PROMPT-VIS]` → auto-corectare imediată: STOP, optimizează, afișează, apoi continuă.
- Regulă asociată: PROMPT-H-003
- Acționează ca **self-check structural**: emiterea VK-ului e dovada optimizării. Absența VK-ului e detectabilă în chain trace și session audit.

**Factored Verification** (CoVe, Meta AI 2023): Când verifici propriul output, NU citi draft-ul original. Generează întrebări de verificare independent, apoi compară. Previne confirmarea propriilor erori.

### 2.5 AFTER — Post-execuție (3 ACTION steps)

**Step 8: POST-TASK CHECKPOINT** (~15 sec per task complex)
- **Precondition**: Task completat
- **Postcondition**: Toate check-urile ✓ sau ✗ explicit
- [ ] Chain complet? (fiecare acțiune a avut `🔗` trace)
- [ ] VK-urile emise? (toate acțiunile non-triviale au VK)
- [ ] Comprehension check rulat? (Pafi a confirmat interpretarea)
- [ ] Learning Loop rulat? (dacă PF: FULL → Post-H.0 executat)
- [ ] Surprize? (ceva neașteptat → flag pentru session audit)
- [ ] **SKILLS CHECK** (MEM-H-002 forced): Am descoperit fix/pattern reutilizabil?
  - **DA** → FORGE template → Cortex save → `✅ [SKILL #N] "title" | FORGE ✓ | saved`
  - **NU** → `Skills: 0 (no reusable patterns discovered)`
- [ ] Prompt-uri afișate? (PRECISION.7 — toate prompt-urile au `[PROMPT-VIS]` emis)
- [ ] **AUDIT CHECK** (AUDIT-PRO): Task-ul a implicat review/audit? → aplică AUDIT-PRO (`~/.nexus/audit/AUDIT-PRO.md`) cu tier-ul potrivit
- **Skip** dacă task-ul a fost trivial (lookup, conversație, <2 tool calls)

Emit VK: `🔍 [CHECKPOINT] task "title" | chain:✓ VK:✓ comprehension:✓ learn:✓/N/A | skills: N saved/0 | surprize: 0`

**Step 9: DELEGARE** — PF: FULL obligatoriu pe ORICE delegare. Prompt-ul complet vizibil.
- **Precondition**: Task necesită subagent
- **Postcondition**: Prompt FULL emis, vizibil

**Step 10: SESSION AUDIT** (la `precision off`, `/sync`, sau exit — ~60 sec)
- **Precondition**: Sesiune PM activă
- **Postcondition**: Audit complet, Cortex saved
- **Completeness**: N/M tasks au avut chain complet (procent)
- **VK coverage**: N VK-uri emise, M expected — coverage %
- **Checkpoints**: N/M post-task checkpoints executate
- **Learning**: N Learning Loops executate, patterns: [S/P/F counts]
- **Skill delta**: N skills saved, M skills used, K procedures consumed
- Dacă chain coverage <90% → flag: `⚠️ Chain coverage sub 90%`
- Salvează summary în Cortex cu tag `precision-session-audit`

Emit VK: `🔓 [PRECISION-AUDIT] session | tasks: N | chain: X% | VK: Y% | checkpoints: Z% | learn: N(S/P/F) | skills: +N used:M`

### 2.6 Chain Trace — Exemple Concrete

```
🔍 Comprehension: "Pafi vrea X, Y, Z" ← PRECISION.0(comprehension) ← PRECISION MODE
📋 Pre-announce: 3 pași planificați ← PRECISION.1(pre-announce) ← PRECISION MODE
🔗 Cortex search obligatoriu ← PRECISION.3(zero-assumptions) ← PROC-H-001 ext
🔗 Clasificare task D2 ← WISH.I.0(Depth) ← REASON-H-001
🔗 Gap analysis ← WISH.I.2(Gap-Aware) ← REASON-H-001
🔗 PromptForge FULL check ← Pre-H(PF gate) ← PROMPT-H-003
🔗 FORGE compliance ← Post-H.2(FORGE) ← META-H-002
▶ [ENTERING] WISH ← PRECISION.5(entry/exit) ← VK-H-001 ext
✅ [PROC] WISH | W✓ I✓ S-skip H✓ PostH✓ ← PRECISION.5(entry/exit) ← VK-H-001 ext
📝 [PROMPT-VIS] target: opus-subagent | PF: FULL | prompt: "..." ← PRECISION.7(prompt-vis) ← PROMPT-H-003
🔍 Post-task checkpoint ← PRECISION.8(checkpoint) ← PRECISION MODE
🔓 Session audit ← PRECISION.10(session-audit) ← PRECISION MODE
```

Dacă un pas NU are chain → **STOP**: fie lipsește o procedură (creează), fie acțiunea nu e justificată (skip).

### 2.7 Enforcement Chain Map

**ACTION steps** (emit VK propriu):
| Step | Faza | Chain | Rule | VK |
|------|------|-------|------|----|
| Comprehension check | BEFORE | PRECISION.0 | COM-H-001 + PROMPT-H-003 | `🔍 [COMPREHENSION]` |
| Pre-announce | BEFORE | PRECISION.1 | COM-H-001 | (template block vizibil) |
| Post-task checkpoint | AFTER | PRECISION.8 | COM-H-001 + MEM-H-002 | `🔍 [CHECKPOINT]` |
| Delegare PF:FULL | AFTER | PRECISION.9 | PROMPT-H-003 | (reuses `✅ [PROMPT]` VK) |
| Session audit | AFTER | PRECISION.10 | COM-H-001 | `🔓 [PRECISION-AUDIT]` |

**MODIFIER steps** (behavioral overlays, fără VK separat):
| Step | Faza | Chain | Ce modifică |
|------|------|-------|-------------|
| Chain trace | DURING | PRECISION.2 | `🔗` prefix la fiecare acțiune |
| Zero assumptions | DURING | PRECISION.3 | Cortex search obligatoriu pe ORICE task |
| Full VK | DURING | PRECISION.4 | SKIP-uri emit și ele VK |
| Entry/Exit VK | DURING | PRECISION.5 | `▶ [ENTERING]` / `✅ [PROC]` la fiecare procedură |
| Show reasoning | DURING | PRECISION.6 | Explică DE CE la fiecare decizie |
| Prompt visibility | DURING | PRECISION.7 | `📝 [PROMPT-VIS]` obligatoriu înainte de orice prompt dispatch. Asociat: PROMPT-H-003 |

### Entry/Exit VKs

```
🔒 [PRECISION] ON | task: "descriere" | procedures: WISH, FORGE, DARE
... (execuție cu chain traces) ...
🔍 [CHECKPOINT] task "title" | chain:✓ VK:✓ comprehension:✓ learn:✓ | skills: 1 saved | surprize: 0
... (mai multe tasks + checkpoints) ...
🔓 [PRECISION-AUDIT] session | tasks: N | chain: X% | VK: Y% | checkpoints: Z% | learn: N(S/P/F) | skills: +N used:M
🔓 [PRECISION] OFF
```

---

## 3. Cortex Logging

### Session audit log (la finalul fiecărei PM session):
```json
{
  "text": "Precision Session: {N} tasks | chain: {X}% | VK: {Y}% | checkpoints: {Z}% | skills: +{S} saved, {U} used | learn: {L}(S/P/F)",
  "collection": "procedures",
  "metadata": {
    "type": "precision_session_audit",
    "procedure": "PRECISION-MODE",
    "rule_id": "COM-H-001",
    "tags": ["precision", "audit", "session"],
    "tasks_count": 0,
    "chain_coverage": 0,
    "vk_coverage": 0,
    "checkpoint_compliance": 0,
    "skills_saved": 0,
    "skills_used": 0,
    "learning_loops": 0,
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- La activarea `precision` / `strict` / `precision mode`
- La orice task clasificat D3+ care nu e deja în PM (Genie sugerează)
- La production audits, architecture decisions, HARD rule changes

### WHEN
- **Manual**: Pafi activează explicit
- **Auto-suggest**: Genie sugerează la trigger-uri (D3+, audits, architecture)
- **Session-long**: Rămâne activ până la `precision off` sau exit
- **Per-task within session**: BEFORE → DURING → AFTER pe fiecare task

### HOW (violation detection)
- Task executat fără comprehension check → violation
- Acțiune fără `🔗` chain trace → violation (chain coverage drops)
- Task non-trivial fără `🔍 [CHECKPOINT]` → violation
- Delegare fără PF: FULL → violation PROMPT-H-003
- Session exit fără `🔓 [PRECISION-AUDIT]` → violation
- Chain coverage < 90% → warning
- VK coverage < 95% → warning
- Skills discovered dar nesalvate → violation MEM-H-002
- Verificare propriului output fără factored verification → warning (CoVe principle)
- Prompt trimis fără `📝 [PROMPT-VIS]` display → violation PRECISION.7 + PROMPT-H-003

### CONNECT
- **COM-H-001** → Precision Mode e extensie a session management
- **PROMPT-H-003** → Comprehension check + PF classification + FULL delegare
- **MEM-H-002** → Skills check la checkpoint + session-end save
- **META-H-002** → FORGE template pe skills discovered
- **VK-H-001** → Full VK + Entry/Exit VK extensions
- **PROC-H-001** → Zero assumptions (Cortex search obligatoriu)
- **REASON-H-001** → Research chain cu DARE (Mechanisms 1-4)
- **Training Mode** → Skill gaps din PM checkpoints pot triggera Training Mode

### VERIFY (procedural checkpoint)
- [ ] Comprehension check rulat pe fiecare task?
- [ ] Pre-announce template emis?
- [ ] Chain traces prezente pe fiecare acțiune?
- [ ] Post-task checkpoints executate pe task-uri non-triviale?
- [ ] Session audit complet la exit?
- [ ] Skills captured sau explicit "0 discovered"?
- [ ] Prompt-uri afișate cu `[PROMPT-VIS]`? (PRECISION.7)

**Două VK-uri obligatorii per procedură FORGE** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] PRECISION-MODE | BEFORE✓ DURING✓ AFTER✓ | tasks: N | chain: X% | complete`
2. **Save VK**: `✅ [CORTEX] "Precision Session {summary}" | FORGE ✓ | rule: COM-H-001 | v1.1`

### MODEL ROUTING

> **RULE (HARD)**: Precision Mode sessions MUST run exclusively on **Opus model**. If no model is preset by the user, Genie switches to Opus before activating Precision Mode. Rationale: Sonnet omits chain traces, skips checkpoints under token pressure, and has lower factored verification quality. Precision Mode's rigor requires Opus-level reasoning.

| Activitate | Model | Motivul |
|-----------|-------|---------|
| **Entire PM session** | **Opus** (mandatory, switch if needed) | PM rigor requires Opus reasoning depth |
| Comprehension check | Opus (session model) | Needs session context + deep interpretation |
| Chain trace generation | Opus (session model) | Real-time per action, no omissions |
| Post-task checkpoint | Opus (session model) | Self-check with factored verification |
| Session audit | Opus (session model) | Session-wide metrics, comprehensive |
| Prompt visibility | Opus (session model) | Inline before each dispatch |
| Factored verification | Opus (independent pass) | Must NOT see original draft |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| WISH Pipeline | Steps modificate de PM modifiers | CLAUDE.md §WISH |
| PromptForge | PF classification la comprehension check | `memory/promptforge.md` |
| FORGE | Template pt skills discovered | `~/.nexus/procedures/FORGEBUILD.md` |
| Cortex KB | Session audit + skills storage | `http://localhost:6400/api/store` |
| Training Mode | Skill gap → training trigger | `~/.nexus/procedures/TRAINING-MODE.md` |
| Research Chain Enforcement | Mechanisms 1-4 active în PM | CLAUDE.md §Research Chain |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Chain coverage | Acțiuni cu `🔗` trace / total acțiuni | ≥ 90% |
| VK coverage | VK-uri emise / expected | ≥ 95% |
| Checkpoint compliance | Checkpoints executate / tasks non-triviale | 100% |
| Comprehension accuracy | Tasks fără re-do din cauza misunderstanding | ≥ 90% |
| Skill capture rate | Skills saved / skills discovered | 100% |
| Factored verification | Verificări independente (fără draft vizibil) | ≥ 1 per session |
| Prompt visibility compliance | Prompts cu `[PROMPT-VIS]` / total prompts dispatch | 100% |
| Session audit completion | Sessions cu audit complet la exit | 100% |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (COM-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Procesul din WHERE referențiază această procedură
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
- [x] ACTION/MODIFIER classification pe fiecare step
- [x] Chain Map cu Type + VK columns
- [x] Entry/Exit VKs definite
- [x] Onboarding description pentru first-time users
- [x] Research basis documented (CoVe, Agent Behavioral Contracts, Gawande, Miller/Cowan, Grigg, AgentSpec)
- [x] Design by Contract (precondition/postcondition) pe ACTION steps
- [x] Cognitive load chunking (3 faze: BEFORE/DURING/AFTER)
- [x] Checklist type specified (READ-DO)
- [x] Metrics section cu targets
