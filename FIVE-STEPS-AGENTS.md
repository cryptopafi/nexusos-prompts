---
type: procedure
created: 2026-03-22
status: active
slug: five-steps-agents
tags: [procedure, nexus]
---

# FIVE-STEPS-AGENTS — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-19
**Versiune**: 1.4
**Regulă asociată**: META-S-012 (Multi-Agent Design Standard)
**Scope**: Framework standardizat pentru arhitectura oricărui sistem multi-agent — de la pipelines simple (2-3 agenți) la orchestrări complexe (10+ agenți). Aplică 5 pași obligatorii: Boundaries, Signal Tiers, Error Handling, Tool Handling, Model Routing.

---

## 1. Problema

### Ce se întâmplă fără un framework standardizat de design multi-agent

1. **Agent overlap** — Fără boundaries clare, agenții fac treaba celorlalți. Signal Detector-ul începe să cerceteze companii. Enrichment Agent-ul scrie copy. Rezultat: date duplicate, conflicte, costuri duble.
2. **Noise flooding** — Fără signal tiers (HIGH/MED/LOW), totul e tratat egal. Un VP hire și un junior hire primesc aceeași prioritate. Pipeline-ul se înecă în noise.
3. **Silent data loss** — Fără error handling explicit, când un agent returnează date incomplete, orchestratorul pur și simplu sare peste entry. Lead-uri pierdute, fără trace.
4. **Tool confusion** — Când toți agenții au acces la toate tool-urile, agenți simpli (ex: signal detection) apelează tool-uri scumpe sau irelevante. Costuri crescute, rezultate impredictibile.
5. **Wrong model, wrong job** — Fără model routing, totul rulează pe același model (de obicei cel mai scump). Signal detection pe Opus = waste. Orchestrare pe Haiku = calitate slabă.

### Situații acoperite

- Design-ul oricărui sistem multi-agent nou
- Refactorizarea unui sistem multi-agent existent care nu respectă cele 5 steps
- Evaluarea arhitecturii unui pipeline (pre-build review)
- Crearea de pipelines productizate (lead finder, content pipeline, etc.)

---

## 2. Procedura

### Phase 0: INTAKE — Structured Discovery Questions

**OBLIGATORIU** înainte de orice design. Skill-ul pune întrebări structurate în 3 runde.

#### Round 1: Scope & Purpose (întotdeauna)

| # | Întrebare | Ce determină |
|---|-----------|-------------|
| Q1 | Ce problemă rezolvă acest sistem? Descrie use case-ul concret. | Scopul pipeline-ului |
| Q2 | Cine e utilizatorul final? (tu/echipa/clienți/automat-fără-om) | Human-in-the-loop, error handling |
| Q3 | Ce INPUT primește sistemul? (date, URL-uri, comenzi, events, cron) | Trigger mechanism, signal detection |
| Q4 | Ce OUTPUT produce? (raport, fișiere, acțiuni, notificări, date structurate) | Output format, responsabilități ultimul agent |
| Q5 | Câte sarcini DISTINCTE vezi? Listează-le pe scurt. | Agent decomposition |

#### Round 2: Architecture Constraints (pe baza R1)

| # | Întrebare | Ce determină |
|---|-----------|-------------|
| Q6 | Unde rulează? (Claude Code local / VPS / API / mix) | Deployment target, file structure |
| Q7 | Budget de cost: gratuit / low-cost / nu contează? | Model routing (Haiku-heavy vs Opus-heavy) |
| Q8 | Frecvență: one-shot / la cerere / scheduled / real-time? | Cron/state management |
| Q9 | Ce tool-uri/API-uri are nevoie? (web search, GitHub, Notion, etc.) | Step 4 Tool Handling |
| Q10 | Ce se întâmplă când ceva eșuează? (retry / flag / alertează / oprește tot) | Step 3 Error Handling |

#### Round 3: Quality & Integration (pentru sisteme complexe, 4+ agenți)

| # | Întrebare | Ce determină |
|---|-----------|-------------|
| Q11 | Există agenți sau sisteme existente cu care trebuie să se integreze? | Handoff points, deduplicare |
| Q12 | Nivel de autonomie dorit: full-auto / human-approval / manual? | Approval gates, safety rails |
| Q13 | Trebuie să țină state între rulări? (persistent / stateless) | State management |
| Q14 | Cum verifici că funcționează corect? Ce înseamnă "succes"? | Success criteria |
| Q15 | Asemănător cu un pattern cunoscut? (lead finder, client onboarding, content pipeline, competitive intel) | Template matching |

**Skip rules:**
- Pipeline simplă (≤3 agenți) → R1 + Q6 + Q7, skip R3
- Pipeline complexă (4+ agenți) → Toate 3 runde
- Template match la Q15 → Folosește template, pune doar delta questions

---

### Phase 1: DECOMPOSE — Agent Roster

Pe baza răspunsurilor din Phase 0:

1. Listează toate responsabilitățile sistemului (din Q5)
2. Grupează în agenți distincți (1 cluster de responsabilități = 1 agent)
3. Identifică orchestratorul (agentul care coordonează, NU execută)
4. Dacă Q15 a dat match pe template → adaptează template-ul, nu reconstrui de la zero:
   - **Lead Finder**: Signal Detector (Haiku) → Enrichment Agent (Sonnet) → Orchestrator. Găsește buying signals, îmbogățește cu date company/decision-maker.
   - **Client Onboarding**: Intake Agent (Sonnet) → Setup Agent (Haiku) → Orchestrator. Colectează date client, provisionează conturi.
   - **Content Pipeline**: Research Agent (Sonnet) → Writer Agent (Sonnet) → Orchestrator. Cercetează topic, scrie conținut, validează contra brief.
   - **Competitive Intel**: Monitor Agent (Haiku) → Analyst Agent (Sonnet) → Orchestrator. Scanează competitori, analizează impact, produce weekly brief.

> **Notă**: P5, P16, P17 sunt improvement proposals din SOL (Self-Optimization Loop) — tehnici de prompt engineering aplicate pe această procedură pentru a îmbunătăți calitatea output-ului.

**Branch-Then-Prune (P16)**: Pentru sisteme cu 4+ agenți, propune 2-3 decompoziții alternative. Evaluează fiecare pe 3 criterii și alege cea mai bună:
1. **Minimize handoff count** — mai puține handoff-uri = mai puțin overhead, mai puține puncte de eșec
2. **Maximize single-responsibility clarity** — fiecare agent are o responsabilitate clară, fără ambiguitate
3. **Respect token budgets** — fiecare agent rămâne sub 800 tokens; dacă depășește, split-uiește

Ex: "Opțiunea A: 3 agenți (signal+enrich+orchestrator) — 2 handoff-uri, responsabilități clare, sub budget. Opțiunea B: 4 agenți (+qualify separat) — 3 handoff-uri, qualify agent prea thin (sub 200 tokens). A preferred."

**Merge Challenge (P17)**: După decompoziție, verifică: "Pot merge 2 agenți fără să violez single-responsibility?" Dacă da și agentul merged rămâne sub 800 tokens → merge. Fewer agents = less handoff overhead.

**Reasoning (P5)**: Pentru fiecare agent, scrie O propoziție care justifică de ce e agent separat (nu merged cu vecinul). Dacă nu poți justifica → merge.

**Output**: Agent roster (tabel)

| Agent | Responsabilitate primară | Model | Tools |
|-------|-------------------------|-------|-------|
| Orchestrator | Coordonare pipeline, compilare output | Sonnet/Opus | — |
| Agent 1 | ... | Haiku/Sonnet | ... |
| Agent N | ... | Haiku/Sonnet | ... |

---

### Phase 2: ARCHITECT — Aplică cele 5 Steps

#### Step 1: Boundaries (pentru FIECARE agent)

Definește explicit ce face ȘI ce NU face fiecare agent.

**GOOD example** (Signal Detector din Lead Finder):
```markdown
## Boundaries
You find hiring signals on job boards.
You return structured data per company.

You do NOT research companies.
You do NOT find decision makers.
You do NOT write outreach copy.
You do NOT assess company quality.
```

**BAD example** (anti-pattern):
```markdown
## What you do
Find leads and research them and write reports about them.
```
→ Zero boundaries. Agentul va face tot, prost.

**Checklist Step 1:**
- [ ] Fiecare agent are secțiune "You do NOT..."
- [ ] Zero overlap între boundaries-urile agenților
- [ ] Orchestratorul NU face treaba sub-agenților

#### Step 2: Signal Tiers (pentru agenți de detecție/clasificare)

Definește criterii specifice per tier, nu binary (relevant/irelevant).

**GOOD example** (Signal Detector):
```markdown
## Signal Tiers
- HIGH SIGNAL → Prioritize: VP or Director-level product hire
- MEDIUM SIGNAL → Include: Multiple mid-level PM hires within 30 days
- LOW SIGNAL → Deprioritize: Single junior hire, no pattern
```

**BAD example** (anti-pattern):
```markdown
## Signals
Find companies that are hiring.
```
→ Zero specificity. Totul e "relevant," noise maxim.

**Checklist Step 2:**
- [ ] Minimum 3 tiers definite (HIGH/MED/LOW)
- [ ] Fiecare tier are criterii concrete, măsurabile
- [ ] Tier-urile sunt mutual exclusive (un signal nu poate fi simultan HIGH și LOW)

#### Step 3: Error Handling (pentru orchestrator + fiecare agent)

Regula de aur: **flag incomplete, never drop.**

**GOOD example** (Orchestrator):
```markdown
## Error Handling
If the enrichment agent returns incomplete data, flag the company
as "needs manual review" instead of dropping it.
If signal detector times out, retry once. If still fails, log and continue
with remaining results.
Never silently skip a lead.
```

**BAD example** (anti-pattern):
```markdown
(no error handling section)
```
→ Pipeline pierde date fără trace.

**Error handling pe baza Q2 (utilizator final):**
- **Human-in-the-loop** (Q2 = tu/echipa): flag → "needs manual review" e suficient
- **Fully autonomous** (Q2 = automat-fără-om): flag → PLUS alerting obligatoriu (webhook, log file, notification). "Flag for human review" fără alerting = items acumulate silențios.

**Circuit breaker**: Dacă TOȚI sub-agenții eșuează într-un pipeline run, orchestratorul emite status error cu zero results și triggerează alerting indiferent de Q2. Nu se livrează raport gol fără notificare.

**Checklist Step 3:**
- [ ] Orchestratorul definește ce face la date incomplete de la fiecare sub-agent
- [ ] "Flag, don't drop" e regula default
- [ ] Fiecare failure path e documentat explicit
- [ ] Dacă Q2 = automat → alerting mechanism definit (nu doar "flag")
- [ ] Circuit breaker: all-fail scenario documentat

#### Step 4: Tool Handling (diferite per agent)

Fiecare agent primește DOAR tool-urile de care are nevoie. Diferite unelte pentru diferiți agenți.

**GOOD example**:
```markdown
Signal Detector tools:
- Job Board API (LinkedIn, Indeed)
- Company Search
- Data Formatter

Enrichment Agent tools:
- Web Search (news, funding, press)
- LinkedIn Lookup (decision makers + titles)
- Company Intel API (size, industry, tech stack)
```
→ Zero overlap. Signal Detector nu are Web Search. Enrichment nu are Job Board.

**BAD example** (anti-pattern):
```markdown
All agents have access to: Web Search, LinkedIn, Job Board,
Company API, Data Formatter, Email Sender
```
→ Toți au totul. Confusion, scope creep, costuri.

**Checklist Step 4:**
- [ ] Fiecare agent are lista proprie de tools
- [ ] Zero tools duplicate între agenți (sau justificare clară dacă da)
- [ ] Orchestratorul NU are tool-urile sub-agenților (el coordonează, nu execută)

#### Step 5: Model Routing (cheapest viable model per agent)

Modelul potrivit = output mai bun + cost mai mic.

**GOOD example**:
```markdown
## Model Routing
- Signal Detector → Haiku (lightweight scraping, cheap)
- Enrichment Agent → Sonnet (synthesis, moderate cost)
- Orchestrator → Sonnet or Opus (coordinating, edge cases)
```

**BAD example** (anti-pattern):
```markdown
All agents use Claude Opus.
```
→ Signal detection pe Opus = 10x costul fără 10x calitatea.

**Routing decision tree (cu cost ratios aproximative):**

| Task Complexity | Model | Când | Cost relativ |
|----------------|-------|------|-------------|
| Scraping, formatting, detection | Haiku | Task simplu, input→output direct | 1x (baseline) |
| Synthesis, assessment, qualification | Sonnet | Trebuie reasoning moderat | ~5x vs Haiku |
| Orchestration, edge cases, complex reasoning | Opus | Coordonare, decizii ambigue | ~30-60x vs Haiku |

Un pipeline bine rutat (Haiku detection + Sonnet enrichment + Sonnet orchestration) costă ~80% mai puțin decât all-Opus.

**Checklist Step 5:**
- [ ] Niciun agent nu folosește un model mai scump decât necesarul
- [ ] Haiku e default-ul; Sonnet/Opus doar cu justificare
- [ ] Orchestratorul specifică model routing-ul în prompt-ul său

---

### Phase 2b: HANDOFF CONTRACTS (între agenți)

Pentru fiecare pereche de agenți care comunică, definește un handoff contract:

**Standard**: Output-ul agentului N = Input-ul agentului N+1. Formatul de output al unui agent E contractul de handoff.

**Format recomandat** (structured JSON):
```json
{
  "agent": "signal-detector",
  "status": "complete|partial|error",
  "items": [
    {
      "company": "Acme Corp",
      "signal_tier": "HIGH",
      "data": { ... },
      "confidence": 0.85
    }
  ],
  "errors": [],
  "metadata": { "timestamp": "...", "items_total": 5, "items_complete": 4 }
}
```

**Confidence calibration**: Câmpul `confidence` din handoff indică cât de sigur e agentul pe datele returnate:
- **> 0.8 (HIGH)**: Date verificate din surse primare (API oficial, bază de date). Acțiune: procesează direct.
- **0.5–0.8 (MEDIUM)**: Date inferate sau din surse secundare (web scraping, mention indirect). Acțiune: procesează dar marchează ca "needs verification".
- **< 0.5 (LOW)**: Date nesigure, incomplete, sau contradictorii. Acțiune: flag ca "needs manual review".

**Reguli handoff:**
- Fiecare handoff include `status` (complete/partial/error) pentru error handling
- `metadata.items_total` vs `items_complete` → orchestratorul detectează date incomplete
- Dacă `status: "partial"` → orchestratorul aplică error handling din Step 3 (flag, don't drop)
- Dacă handoff-ul e structurally invalid (malformed JSON, câmpuri obligatorii lipsă) → orchestratorul tratează ca `status: "error"` și aplică Step 3 error handling

**Checklist handoff:**
- [ ] Output format-ul fiecărui agent e documentat explicit
- [ ] Output Agent N = Input Agent N+1 (contract match)
- [ ] `status` field prezent în fiecare handoff
- [ ] Orchestratorul parsează `status` și aplică error handling

---

### Phase 2c: STATE MANAGEMENT (dacă Q13 = persistent)

Dacă intake-ul la Q13 indică nevoia de state persistent:

**Stateless (default)** — Fiecare rulare e independentă. Zero fișiere între rulări. Recomandat pentru pipelines on-demand.

**Persistent state** — Pentru pipelines scheduled sau cu continuity needs:
- **State file**: `{project}/state.json` — conține ultima rulare, items procesate, erori anterioare
- **History**: `{project}/history/{date}-run.json` — log per rulare
- **Resume**: dacă pipeline-ul se oprește mid-run, state file permite resume de unde a rămas

**Pattern**:
```json
{
  "last_run": "2026-03-19T10:00:00Z",
  "items_processed": ["id1", "id2"],
  "items_pending": ["id3"],
  "items_flagged": ["id4"],
  "run_count": 5
}
```

**State corruption recovery**: Dacă `state.json` e malformed (JSON invalid), rename la `state.json.corrupt`, start fresh, și log incident.

**Guideline**: Stateless by default. Adaugă state doar dacă Q8 = scheduled SAU Q13 = persistent.

---

### Phase 2d: IDEMPOTENCY (safe re-runs)

**Regula**: Orice pipeline trebuie să fie safe de re-rulat pe același input.

**Patterns de idempotency:**
1. **Output naming**: Folosește timestamps în output files (`report-2026-03-19.json`, nu `report.json`). Re-run = fișier nou, nu overwrite.
2. **Deduplication**: Dacă pipeline-ul procesează items (leads, articole, etc.), verifică dacă item-ul a fost deja procesat (`items_processed` din state file).
3. **Lock file**: Pentru pipelines scheduled, folosește lock file (`{project}/.lock`) cu PID și timestamp. Șterge lock-ul la finalizare. Dacă lock-ul e mai vechi decât durata maximă a pipeline-ului (stale lock de la crash), log warning și continuă.

**Checklist idempotency:**
- [ ] Re-run pe același input nu produce duplicate
- [ ] Output files au timestamp (nu overwrite)
- [ ] Lock file previne concurrent runs (dacă scheduled)
- [ ] State file trackează items procesate (dacă persistent)

---

### Phase 3: GENERATE — Produce Files

Pe baza arhitecturii din Phase 2, generează:

1. **orchestrator.md** — System prompt pentru coordinator
   - Sub-Agents list cu paths
   - Workflow sequence (ordinea agenților)
   - Error handling rules
   - Model routing instructions
   - ≤1500 tokens

2. **N × sub-agent .md files** — System prompt per agent
   - Boundaries (does + does NOT)
   - Signal tiers (dacă e relevant)
   - Output format (structured, specific)
   - Tools available
   - ≤800 tokens per agent

3. **AGENTS.md** (opțional, pentru sisteme complexe)
   - Routing table
   - Safety table
   - ≤1500 tokens

4. **Route command** (opțional)
   - One-line command în agents.md care declanșează pipeline-ul

---

### Phase 4: VALIDATE — Checklist Final

| # | Check | Pass? |
|---|-------|-------|
| 1 | Fiecare agent are boundaries ("does" + "does NOT")? | |
| 2 | Signal tiers definite cu criterii concrete (min 3 tiers)? | |
| 3 | Error handling: "flag, don't drop" pe fiecare path? | |
| 4 | Tool-uri izolate per agent, zero overlap nejustificat? | |
| 5 | Model routing: cheapest viable per agent (cu justificare)? | |
| 6 | Token budget: SOUL.md ≤800, orchestrator ≤1500? | |
| 7 | Orchestratorul NU execută (doar coordonează)? | |
| 8 | Handoff contract: output Agent N = input Agent N+1, cu `status` field? | |
| 9 | Output format specificat per agent (structured data)? | |
| 10 | Idempotency: re-run safe? Fără duplicate? Timestamps pe outputs? | |
| 11 | State management: stateless explicit SAU persistent pattern implementat? | |
| 12 | Dacă Q2=automat → alerting mechanism definit (nu doar "flag")? | |

Scor: X/12. Minimum 10/12 pentru PASS.

---

### Phase 5: DEPLOY

1. Plasează fișierele în path-urile corecte (Claude Code, project folder, etc.)
2. Înregistrează route command (dacă e cazul)
3. Salvează arhitectura în Cortex
4. Emit VK: `✅ [5STEPS] pipeline: {name} | agents: {N} | models: {list} | score: {X}/12`

---

### Test Cases

1. **Normal flow — Lead Finder Pipeline**:
   - Input: "Vreau un pipeline care găsește companii ce angajează VP Product"
   - Expected: 3 agents (Signal Detector Haiku + Enrichment Sonnet + Orchestrator Sonnet), boundaries clare, signal tiers HIGH/MED/LOW, error handling "flag don't drop"
   - Validate: Checklist ≥10/12

2. **Edge case — Doar 2 agenți**:
   - Input: "Content pipeline simplu: research + writing"
   - Expected: 2 agents + orchestrator minimal, skip Round 3 din Intake
   - Validate: Checklist ≥10/12

3. **Failure case — Single agent**:
   - Input: "Un chatbot care răspunde la întrebări"
   - Expected: Gate = NO (1 responsabilitate). Recomandă SOUL.md template standard, NU Five Steps Agents.

---

## 3. Cortex Logging

```json
{
  "text": "FIVE-STEPS-AGENTS: Pipeline {name} architected. {N} agents, models: {list}. Checklist: {X}/12.",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "FIVE-STEPS-AGENTS",
    "rule_id": "META-S-012",
    "has_enforcement_loop": true,
    "forge_version": "1.5",
    "tags": ["multi-agent", "orchestration", "architecture", "five-steps"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- La orice creare de sistem multi-agent (2+ agenți)
- La refactorizarea unui sistem multi-agent existent
- La review-ul arhitecturii unui pipeline

### WHEN
- De fiecare dată când se creează un nou set de agenți
- La request explicit: "five steps agents" / "build pipeline for X"

### HOW (violation detection)
- Sistem multi-agent creat fără cele 5 steps → violation
- Agent fără boundaries ("does NOT" section) → violation
- Pipeline fără error handling explicit → violation
- Toți agenții pe același model fără justificare → violation
- Tool-uri identice pe toți agenții → violation
- Checklist < 10/12 fără re-work → violation

### CONNECT
- **META-S-012** → Multi-Agent Design Standard
- **FORGEBUILD.md** → Template-ul care structurează această procedură
- **PROMPTING.md** → Calitatea prompt-urilor generate (agent system prompts)
- `procedure-health.json` → entry adăugat

### VERIFY (procedural checkpoint)
- [ ] Procedura a fost executată complet? (Phase 0-5)
- [ ] Toate cele 5 steps aplicate pe fiecare agent?
- [ ] Checklist ≥ 10/12?
- [ ] VK emis?

**VK-uri obligatorii:**
1. **Execution VK**: `✅ [PROC] FIVE-STEPS-AGENTS | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "FIVE-STEPS-AGENTS: {pipeline}" | FORGE ✓ | rule: META-S-012 | v1.4`

### MODEL ROUTING

| Activitate | Model | Motivul |
|-----------|-------|---------|
| Intake questions (Phase 0) | Sonnet (Genie) | Conversational, nu necesită deep reasoning |
| Agent decomposition (Phase 1) | Sonnet (Genie) | Standard architecture task |
| Architecture design (Phase 2) | Opus subagent | Complex systems need deep reasoning |
| File generation (Phase 3) | Sonnet (Genie) | Template-based, straightforward |
| Validation (Phase 4) | Sonnet (Genie) | Checklist verification |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| FORGEBUILD.md | Template structură proceduri | `~/.nexus/procedures/FORGEBUILD.md` |
| PROMPTING.md | Calitate prompts generate | `~/.nexus/procedures/PROMPTING.md` |
| SOL v1.5 | Audit periodic pe agent prompts generate | `~/.nexus/procedures/SELF-OPTIMIZATION-LOOP.md` |
| AUDIT-PRO | DSE-WORKFLOW audit pe pipelines | `~/.nexus/audit/AUDIT-PRO.md` |
| Skill Five Steps Agents | Entry point (UI) | `~/.claude/skills/five-steps-agents/SKILL.md` |
| Templates | 4 pipeline templates | `~/.claude/skills/five-steps-agents/references/templates.md` |
| Cortex API | Salvare arhitecturi | `localhost:6400/api/store` |
| procedure-health.json | Registry proceduri | `~/.openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Adoption rate | % sisteme multi-agent create cu Five Steps | ≥ 90% |
| Checklist pass rate | % pipelines cu ≥ 10/12 la validation | ≥ 85% |
| Agent boundary compliance | % agenți cu "does NOT" section | 100% |
| Model routing savings | % cost reduction vs all-Opus | ≥ 40% |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (META-S-012)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu checks obligatorii
- [x] Descrie CE nu CUM
- [x] VK format specificat (per VK-H-001)
- [x] Entry adăugat în `procedure-health.json` (2026-03-19)
- [x] Salvat în Cortex cu metadata FORGE (2026-03-19, id: 3f868fcc)
- [x] Test cases documentate (3: normal, edge, failure)

---

## Changelog

*Changelog — newest first:*

| Versiune | Data | Modificări |
|---------|------|-----------|
| **1.4** | **2026-03-19** | SOL 3 LOW fixes: confidence calibration (>0.8/0.5-0.8/<0.5 cu acțiuni), branch-then-prune selection criteria (3 criterii explicite), template summaries inline (4 templates cu arhitectură). SOL score: 89→est.91. |
| 1.3 | 2026-03-19 | FORGE-AUDIT fix (6 findings): F1 P-code explanation added (D4), F2 changelog reordered (D5), F3 malformed handoff handling (W1), F4 circuit breaker for all-fail (W3), F5 stale lock handling (W4), F6 state corruption recovery (W2). |
| 1.2 | 2026-03-19 | SOL v1.4 sync: +P16 Branch-Then-Prune, +P17 Merge Challenge, +P5 Reasoning. Dependencies updated. |
| 1.1 | 2026-03-19 | FORGE-AUDIT fixes: Phase 2b Handoff Contracts, Phase 2c State Management, Phase 2d Idempotency. Cost ratios, autonomous alerting, checklist 10→12. |
| 1.0 | 2026-03-19 | Versiune inițială. 5 steps + Phase 0 Intake + calibration examples + 4 pipeline templates + validation checklist. |
