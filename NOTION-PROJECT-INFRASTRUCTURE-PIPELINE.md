---
type: procedure
created: 2026-03-17
status: active
slug: notion-project-infrastructure-pipeline
tags: [procedure, nexus]
---

# NOTION-PROJECT-INFRASTRUCTURE-PIPELINE — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.1
**Regula asociata**: PROJ-H-001 (orice proiect activ TREBUIE sa aiba un Project Card) + META-H-002 (FORGE)
**Scope**: Pipeline automat care, la crearea unui proiect nou in Nexus, genereaza infrastructura Notion optimizata per tip de proiect (Business, Personal, Infrastructure), folosind procedurile Notion existente in ordine logica.

---

## 1. Problema

Cand se creeaza un proiect nou in Nexus, infrastructura Notion se construieste ad-hoc: DB-uri create fara schema standard, fara relatii cu master DB-urile existente, fara dashboard, fara AI properties, fara automatizari. Rezultatul: fiecare proiect arata diferit, lipsesc view-uri critice, relatiile cu Tasks/Procedures/Research centralizate nu exista, iar timpul de setup creste exponential cu fiecare proiect nou.

**Fara procedura asta:**
- Proiecte noi fara DB-uri standard (Tasks, KPIs, Knowledge) — totul manual
- Zero relatii cu master DB-urile centralizate (Projects, Tasks, Procedures)
- Fiecare proiect reinventeaza structura — inconsistenta, nescalabil
- Lipsesc AI properties pe DB-uri text-heavy — scanabilitate zero
- Nicio automatizare Slack pentru proiecte Business (schimbari de status invizibile)
- Dashboard-ul proiectului lipseste sau e minimal
- La PROJECT-CARD-ONBOARDING (Pas 1), infrastructura Notion nu se creeaza — ramane pe Pafi manual

**Situatii acoperite:**
- Proiect nou creat in Nexus (Business B1-B8, Personal P1-P9, Infrastructure N1-N10)
- Proiect existent fara infrastructura Notion completa (retroactiv)
- Restructurare infrastructura proiect existenta (tip de proiect se schimba)
- Audit periodic de completitudine infrastructura Notion per proiect

---

## 2. Procedura

### MODEL ROUTING

| Pas | Executor | Conditie |
|-----|----------|---------|
| Pas 1 TRIGGER | Genie (Sonnet) | Automat la PROJECT-CARD-ONBOARDING Pas 1, sau la cerere |
| Pas 2 CLASIFICARE | Genie (Sonnet) | Decision tree simplu — 3 tipuri |
| Pas 3-9 CONSTRUCTIE | Genie (Sonnet) | Executie secventiala a procedurilor Notion |
| Pas 10 VERIFY | Genie (Sonnet) | Checklist automat post-constructie |
| Design arhitectural complex | Opus | Cand structura proiectului e atipica sau cross-domain |

---

### Pas 1: TRIGGER — Cand se activeaza pipeline-ul

Pipeline-ul se activeaza in urmatoarele situatii:
- **Proiect nou**: PROJECT-CARD-ONBOARDING Pas 1 apeleaza automat acest pipeline DUPA crearea Project Card
- **Retroactiv**: Pafi cere "aplica infrastructura Notion pe proiectul X"
- **Audit**: la verificarea trimestriala de completitudine, se identifica un proiect fara infrastructura completa

**Input necesar:**
- Slug-ul proiectului (din `~/.nexus/projects/{SLUG}/PROJECT-CARD.md`)
- Tipul proiectului (Business / Personal / Infrastructure) — determinat la Pas 2
- Workspace Notion existent cu structura din NOTION-SYNC (NEXUS root page)

**Emit VK**: `[PIPELINE] TRIGGER project={slug} | source={onboarding|retroactiv|audit}`

---

### Pas 2: CLASIFICARE TIP PROIECT (decision tree)

Citeste Project Card-ul si determina tipul:

```
Proiectul genereaza revenue direct?
  |
  YES --> BUSINESS
  |        Proiecte: B1-B8 (SMSads, Gambling SEO, Solnest, AI B2B, etc.)
  |        Structura: Tasks DB + CRM/Contacts DB + Finance Tracking DB
  |                   + KPI Dashboard + Knowledge Base + Slack integration
  |
  NO --> E proiect Nexus tehnic / infrastructura?
          |
          YES --> INFRASTRUCTURE
          |        Proiecte: N1-N10 (OpenClaw, ECHELON, Cortex, Delphi, etc.)
          |        Structura: Tasks DB + Docs DB + Monitoring DB
          |                   + Integrations Log + Error Tracking
          |
          NO --> PERSONAL
                   Proiecte: P1-P9 (Health, Travel, Learning, Crypto, etc.)
                   Structura: Goals DB + Habits/Tracking DB + Notes DB
                              + Resources DB
```

**Principiu arhitectural din SuperDelphi D4 (68/100):**
- **Linked DBs centralizate + filtered views per proiect** — NU copii de DB-uri
- Master DB-uri (Tasks, Procedures, Research, Contacts) sunt UNICE la nivel de workspace
- Fiecare proiect primeste **filtered views** pe aceste master DB-uri, NU DB-uri duplicate
- DB-urile specifice proiectului (KPIs, Finance, Monitoring) sunt child databases locale

**Emit VK**: `[PIPELINE] CLASSIFY project={slug} | type={BUSINESS|PERSONAL|INFRASTRUCTURE}`

---

### Pas 3: WORKSPACE SETUP (ref: PROC-008)

Verifica ca proiectul are un loc in ierarhia Notion existenta:

**Business**: sub pagina `BUSINESSES` din NEXUS root
**Personal**: sub pagina `PERSONAL` din NEXUS root
**Infrastructure**: sub pagina `INFRASTRUCTURE` din NEXUS root

Daca pagina proiectului nu exista:
1. Creeaza pagina proiect sub hub-ul corespunzator (naming convention: `[Emoji] [Project Name]`)
2. Aplica naming conventions din PROC-008 Pas 4
3. Creeaza pagina wiki "Start Here" cu link catre Project Card si overview

Daca pagina exista: verifica naming + structura conform PROC-008.

---

### Pas 4: DB DESIGN — Creeare DB-uri specifice tipului (ref: NOTION-DATABASE-DESIGN-PATTERN)

Per tip de proiect, creeaza DB-urile locale (child databases pe pagina proiectului) folosind NOTION-DATABASE-DESIGN-PATTERN Pas 1-6:

#### BUSINESS — DB-uri locale:
| DB | Proprietati cheie | View-uri minime |
|----|-------------------|----------------|
| **KPI Dashboard DB** | Metric (title), Value (number), Target (number), Period (date), Status (select: On Track/Behind/Exceeded) | Table + Board (by Status) + Calendar |
| **Finance Tracking DB** | Transaction (title), Amount (number), Type (select: Income/Expense), Category (select), Date (date), Status (select: Pending/Confirmed) | Table + Board (by Type) + Calendar |
| **CRM / Contacts DB** (linked view) | Filtered view pe master Contacts DB cu filter: Project = {slug} | Table + Board (by Status) |

#### PERSONAL — DB-uri locale:
| DB | Proprietati cheie | View-uri minime |
|----|-------------------|----------------|
| **Goals DB** | Goal (title), Category (select: Health/Finance/Learning/Career/Social), Target Date (date), Progress (number %), Status (select: Active/Paused/Done) | Table + Board (by Status) + Calendar |
| **Habits Tracking DB** | Habit (title), Frequency (select: Daily/Weekly/Monthly), Streak (number), Last Done (date), Active (checkbox) | Table + Board (by Frequency) |
| **Notes DB** | Note (title), Topic (multi-select), Created (created time), Tags (multi-select) | Table + Calendar |

#### INFRASTRUCTURE — DB-uri locale:
| DB | Proprietati cheie | View-uri minime |
|----|-------------------|----------------|
| **Docs DB** | Document (title), Type (select: ADR/Runbook/Config/Schema), Status (select: Draft/Active/Deprecated), Owner (person), Last Reviewed (date) | Table + Board (by Status) |
| **Monitoring DB** | Alert (title), Severity (select: Critical/Warning/Info), Component (select), Timestamp (date), Resolved (checkbox) | Table + Board (by Severity) + Calendar |
| **Integrations Log DB** | Integration (title), Source (select), Target (select), Status (select: Active/Broken/Deprecated), Last Checked (date) | Table + Board (by Status) |

**Per DB creat**: aplica NOTION-DATABASE-DESIGN-PATTERN complet (Pas 1-6 inclusiv Lock dupa stabilizare).

---

### Pas 5: LINKED VIEWS — Filtered views pe master DB-uri (ref: NOTION-RELATION-ROLLUP-SETUP)

Creeaza **linked database views** (NU copii) pe pagina proiectului pentru master DB-urile centralizate:

**Toate tipurile de proiect primesc:**

| Master DB | Filtered View | Filter |
|-----------|---------------|--------|
| **Tasks DB** (centralizat) | `{Project} Tasks` | Project relation = {slug} |
| **Procedures DB** (centralizat) | `{Project} Procedures` | Domain/Project tag = {slug} |
| **Research DB** (centralizat) | `{Project} Research` | Topic/Project tag = {slug} |

**Pasi:**
1. Pe pagina proiectului, insereaza `/linked view of database`
2. Selecteaza master DB-ul
3. Configureaza filtru: proprietatea Project/Domain/Topic = slug-ul proiectului
4. Redenumeste view-ul descriptiv
5. Verifica ca relatia two-way exista pe master DB (daca lipseste, aplica NOTION-RELATION-ROLLUP-SETUP Pas 2-3)

**Rollup-uri pe Projects master DB** (aplica NOTION-RELATION-ROLLUP-SETUP Pas 5):
- `Active Tasks Count` — Rollup pe Tasks relation, Calculate: Count all where Status != Done
- `Procedures Count` — Rollup pe Procedures relation, Calculate: Count all
- `Research Items Count` — Rollup pe Research relation, Calculate: Count all

**Performanta (SuperDelphi D4 finding):**
- Archive completed items (Status = Done > 30 zile) in archive views
- Split DB-uri care depasesc 500 items in sub-DB-uri per domeniu
- Limit rollup complexity: max 3 rollup-uri per DB care refera alta DB
- Foloseste filtered views in loc de duplicate DB-uri — ALWAYS

---

### Pas 6: HOME DASHBOARD proiect (ref: PROC-006)

Configureaza pagina principala a proiectului ca mini-dashboard operational:

**Layout standard per tip:**

```
# [Project Name]
Status: {Active/Paused/Planning}
Last updated: {auto}
Card: link catre ~/.nexus/projects/{slug}/PROJECT-CARD.md

---

## Quick Actions
[Button: + New Task] [Button: + New Note/Doc]

---

## Active Tasks (linked view — filtered din Tasks master DB)
[Board view, grouped by Status, filter: Project = {slug}, Status != Done]

---

## KPIs / Goals / Monitoring (DB local specific tipului)
[Table view, sorted by relevance]

---

## Recent Research (linked view — filtered din Research master DB)
[Table view, filter: Project = {slug}, sort: Created DESC, limit: 10]

---

## Procedures (linked view — filtered din Procedures master DB)
[Table view, filter: Domain/Project = {slug}]

---

## Intelligence Feed (daca Business)
[Ultimele 10 items ECHELON/SuperInsight relevante — linked din LIVE INTELLIGENCE]
```

**BUSINESS extra**: sectiune Finance Tracking DB view + KPI Dashboard DB view
**PERSONAL extra**: sectiune Goals DB view + Habits Tracking DB view
**INFRASTRUCTURE extra**: sectiune Monitoring DB view + Integrations Log DB view

---

### Pas 7: AI PROPERTIES autofill (ref: PROC-007)

Configureaza AI Autofill properties pe DB-urile text-heavy ale proiectului:

**Se aplica pe (minim):**
- **Docs DB** (Infrastructure) → AI Summary property (mode: Summary, prompt: "Summarize this document in 2 sentences: what it covers and its current status.")
- **Notes DB** (Personal) → AI Summary property
- **Research DB** (linked, toate tipurile) → AI Summary + AI Action Items
- **Knowledge Base DB** (Business) → AI Summary + AI Key Quotes

**Per DB text-heavy**: aplica PROC-007 complet (Pas 1-7, inclusiv batch autofill si edge-case review).

**SuperDelphi D4 finding**: AI autofill pentru Summary, Status prediction, SEO metadata din continut. Activeaza auto-update pe toate AI properties.

**Conditie**: Notion AI plan activ (~$12/month). Daca plan inactiv, skip acest pas si marcheaza `[AI-SKIP: no Notion AI plan]`.

---

### Pas 8: BUTTON AUTOMATION (ref: PROC-009)

Creeaza Buttons standard pe pagina proiectului:

**Toate tipurile:**
| Button | Actiuni | Plasat pe |
|--------|---------|-----------|
| `+ New Task` | Create in Tasks master DB + Project relation = {slug} + Status = To Do + Date = Today | Pagina proiect — sectiunea Quick Actions |
| `+ New Note/Doc` | Create in Notes/Docs DB local + Status = Draft + Date = Today | Pagina proiect — sectiunea Quick Actions |

**Business extra:**
| Button | Actiuni |
|--------|---------|
| `+ New Contact` | Create in Contacts master DB + Project relation = {slug} |
| `+ New KPI Entry` | Create in KPI Dashboard DB + Period = Today |
| `+ Log Transaction` | Create in Finance Tracking DB + Date = Today + Status = Pending |

**Infrastructure extra:**
| Button | Actiuni |
|--------|---------|
| `+ New Doc` | Create in Docs DB + Type = Runbook + Status = Draft |
| `+ Log Alert` | Create in Monitoring DB + Timestamp = Now + Resolved = false |

**Per Button**: aplica PROC-009 complet (Pas 1-7, inclusiv testare si documentare callout).

---

### Pas 9: INTEGRARI EXTERNE (conditionat pe tip)

#### 9A. Slack Integration (doar BUSINESS) — ref: NOTION-SLACK-AUTOMATION

Configureaza automatizari Notion → Slack:
- **Tasks DB** (filtered pe proiect): trigger Status → "Done" → mesaj in canal proiect Slack
- **KPI Dashboard DB**: trigger Status → "Behind" → mesaj in #alerts
- **Finance Tracking DB**: trigger Amount > threshold → mesaj in #finance-alerts

Aplica NOTION-SLACK-AUTOMATION Pas 1-8 per automatizare.

#### 9B. Forms (daca relevant) — ref: PROC-010

Daca proiectul necesita colectare date externe:
- **Business**: feedback form de la clienti → submissions in Feedback DB
- **Personal**: self-assessment forms → submissions in Tracking DB

Aplica PROC-010 Pas 1-7 per formular.

#### 9C. Zapier (daca relevant) — ref: NOTION-ZAPIER-PIPELINE-SETUP

Daca proiectul necesita integrari cu tool-uri fara integrare nativa Notion:
- Gmail → Notion (capturare email-uri proiect)
- Google Sheets → Notion (sync KPI data)

Aplica NOTION-ZAPIER-PIPELINE-SETUP Pas 1-6 per pipeline.

#### 9D. NOTION-SYNC setup — ref: NOTION-SYNC.md

Asigura ca proiectul e integrat in sync-ul bidirectional:
- Push: SuperInsight/ECHELON findings relevante → pagina proiect Intelligence Feed
- Pull: modificari Notion (task status changes) → pafi-tasks.md local
- Conflict resolution: per reguli din NOTION-SYNC §4.3

---

### Pas 10: VERIFY — Checklist completitudine infrastructura

Imediat dupa constructie, ruleaza acest checklist:

**A. Structural Check:**
- [ ] Pagina proiect exista sub hub-ul corect (BUSINESSES/PERSONAL/INFRASTRUCTURE)?
- [ ] Naming convention respectata (PROC-008)?
- [ ] Wiki "Start Here" page creata cu link la Project Card?

**B. DB Check:**
- [ ] Toate DB-urile locale per tip create (conform tabel Pas 4)?
- [ ] Fiecare DB are minim Title + Status + Date + Owner?
- [ ] Fiecare DB are minim 2 view-uri (Table + Board)?
- [ ] Schema locked pe DB-urile stabile?

**C. Linked Views Check:**
- [ ] Tasks master DB linked view cu filter Project = {slug}?
- [ ] Procedures master DB linked view cu filter Domain = {slug}?
- [ ] Research master DB linked view cu filter Topic = {slug}?
- [ ] Relatii two-way configurate pe master DB-uri?
- [ ] Rollup-uri (Active Tasks Count, Procedures Count) prezente pe Projects master DB?

**D. Dashboard Check:**
- [ ] Pagina proiect are layout standard (sectiunile din Pas 6)?
- [ ] Quick Actions buttons create si testate?
- [ ] Toate linked views si DB views afisate pe dashboard?

**E. AI & Automation Check:**
- [ ] AI properties configurate pe DB-uri text-heavy (sau [AI-SKIP] documentat)?
- [ ] Auto-update activat pe toate AI properties?
- [ ] Slack automatizari configurate (doar Business) si testate?
- [ ] NOTION-SYNC integrat (push + pull)?

**F. Performance Check (SuperDelphi D4):**
- [ ] Zero DB-uri duplicate (linked views, nu copii)?
- [ ] Rollup complexity max 3 per DB?
- [ ] Archive view configurata pentru items Done > 30 zile?

**Verify VK** (obligatoriu):
```
[PIPELINE] VERIFY project={slug} | type={TYPE} | dbs_created={N} | linked_views={N} | buttons={N} | ai_props={N|SKIP} | slack={N|N/A} | checks_pass={N}/{TOTAL}
```

Daca `checks_pass < TOTAL` → fix imediat pe items failed → re-verify.

---

## 2b. Error Handling, State Management & Rollback

### Pipeline State File

La inceputul executiei (Pas 1), creeaza un state file:
`~/.nexus/sync/pipeline-state-{slug}.json`

```json
{
  "project": "{slug}",
  "type": "{BUSINESS|PERSONAL|INFRASTRUCTURE}",
  "started": "{ISO_DATE}",
  "current_step": 1,
  "completed_steps": [],
  "failed_steps": [],
  "created_resources": {
    "pages": [],
    "databases": [],
    "linked_views": [],
    "buttons": [],
    "automations": []
  },
  "status": "IN_PROGRESS"
}
```

**La fiecare pas completat**: actualizeaza `current_step`, adauga la `completed_steps`, adauga resursele create la `created_resources`.

**Resume capability**: daca pipeline-ul se intrerupe (session end, crash, API down), la reluare:
1. Citeste state file → identifica `current_step`
2. Verifica `completed_steps` — nu re-executa pasii deja completati
3. Reia de la `current_step` cu resursele deja create disponibile
4. Emit VK: `[PIPELINE] RESUME project={slug} | from_step={N} | completed={list}`

### Per-Step Error Handling

| Pas | Failure Mode | Detection | Recovery |
|-----|-------------|-----------|----------|
| Pas 3 (Workspace) | Pagina nu se creeaza (API error) | HTTP 400/429/500 de la Notion MCP | 429: wait 60s + retry (max 3). 400: log error, notify Pafi, STOP. 500: retry 2x then STOP. |
| Pas 4 (DB Create) | DB creation fail | Notion MCP error | Retry 2x. Daca fail persistent: skip DB-ul, log in state file ca `failed_steps`, continua cu urmatorul DB. La final, raport failures. |
| Pas 5 (Linked Views) | Master DB nu exista sau relatie lipseste | Linked view creation fail | Verifica existenta master DB. Daca lipseste: log warning, skip linked view, continua. Daca relatie lipseste: creeaza relatie (NOTION-RELATION-ROLLUP-SETUP) → retry linked view. |
| Pas 7 (AI Props) | Notion AI plan inactiv | AI property creation fail | Log `[AI-SKIP: no Notion AI plan]`, continua pipeline fara AI properties. |
| Pas 8 (Buttons) | Button creation fail | Notion MCP error | Retry 1x. Daca fail: log, skip button, continua. Buttons sunt nice-to-have, nu blocker. |
| Pas 9 (Integrations) | Slack/Zapier not connected | Authorization error | Log `[INTEGRATION-SKIP: {tool} not connected]`, continua. Flag Pafi sa configureze manual. |

### Degraded Mode

| Component Down | Pipeline Behavior |
|---------------|-------------------|
| Notion API (complet) | STOP pipeline. Save state file. Queue pentru re-executie la urmatoarea sesiune. Notify Pafi. |
| Notion API (intermitent, 429) | Wait 60s + retry per step. Max 3 retries per step. Dupa 3: skip step, log, continua. |
| Cortex (down) | Pipeline continua (Cortex e doar logging). Skip §3 Cortex save. Retry la session end. |
| Master DB lipseste | Skip linked views pentru acel DB. Log warning. Continua cu restul pipeline-ului. |

### Rollback (abandonare pipeline la mijloc)

Daca pipeline-ul e abandonat (Pafi decide sa nu continue, sau failures critice):
1. Citeste `created_resources` din state file
2. **NU sterge automat** — resursele partial create pot fi utile
3. Marcheaza state file: `"status": "ABANDONED"` + `"abandoned_at": "{ISO_DATE}"`
4. La audit trimestrial: state files cu `ABANDONED` sunt revizuite — Pafi decide: resume, cleanup, sau delete
5. Cleanup manual: lista resursele create → Pafi confirma stergerea → delete pages/DBs via Notion MCP

**Pipeline completat cu succes**: state file se actualizeaza cu `"status": "COMPLETED"` si ramane ca audit trail (nu se sterge).

---

## 3. Cortex Logging

La completarea pipeline-ului pentru un proiect:

```json
{
  "text": "NOTION-PROJECT-INFRASTRUCTURE-PIPELINE completed for project {slug} (type: {TYPE}). Created: {N} local DBs, {N} linked views, {N} buttons, {N} AI properties, {N} Slack automations. All checks PASS.",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "NOTION-PROJECT-INFRASTRUCTURE-PIPELINE",
    "rule_id": "PROJ-H-001",
    "project": "{slug}",
    "project_type": "{BUSINESS|PERSONAL|INFRASTRUCTURE}",
    "dbs_created": "{N}",
    "linked_views": "{N}",
    "buttons_created": "{N}",
    "ai_properties": "{N}",
    "slack_automations": "{N}",
    "tags": ["notion", "infrastructure", "pipeline", "project-setup", "automation"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- **PROJECT-CARD-ONBOARDING Pas 1** — cand se creeaza un proiect nou, acest pipeline se apeleaza automat DUPA Project Card
- **Audit trimestrial** — verificare completitudine infrastructura Notion per proiect
- **Manual** — Pafi cere "aplica infrastructura Notion pe X"

### WHEN
- La FIECARE creare de proiect nou in Nexus (via PROJECT-CARD-ONBOARDING)
- La cerere retroactiva pentru proiecte existente fara infrastructura completa
- La audit trimestrial de workspace (detectare proiecte fara infrastructura)
- La schimbare de tip proiect (ex: side project → business)

### HOW (violation detection)
- Proiect cu Project Card dar fara infrastructura Notion → violation PROJ-H-001
- DB-uri create ca copii in loc de linked views pe master DB-uri → violation (duplicare)
- Proiect Business fara Slack integration configurata → violation
- DB text-heavy (>50 pagini) fara AI properties → violation (PROC-007)
- Pagina proiect fara Quick Actions buttons → violation (PROC-009)
- Linked views fara filter pe proiect → violation (afiseaza date din toate proiectele)
- Pipeline executat partial (pasi sarit fara [SKIP] cu motiv) → violation
- Runner: Opus audit trimestrial + FORGE-AUDIT per proiect + checklist Pas 10 la fiecare executie

**Actiuni interzise:**
- Crearea de DB-uri duplicate pentru entitati care au master DB (Tasks, Procedures, Research, Contacts)
- Executarea pipeline-ului fara Project Card existent (Card e prerequisite)
- Crearea de relatii one-way in loc de two-way pe master DB-uri
- Skip AI properties fara documentare motiv explicit

### CONNECT
- **PROJ-H-001** → orice proiect activ TREBUIE sa aiba infrastructura Notion completa
- **META-H-002** → FORGE enforcement standard
- **PROJECT-CARD-ONBOARDING** → Pas 1 apeleaza acest pipeline (integrare directa)
- **NOTION-SYNC** → pipeline-ul configureaza sync bidirectional per proiect
- **PROC-008** → workspace architecture folosita la Pas 3
- **NOTION-DATABASE-DESIGN-PATTERN** → DB design folosit la Pas 4
- **NOTION-RELATION-ROLLUP-SETUP** → relatii + rollup-uri configurate la Pas 5
- **PROC-006** → dashboard configurare la Pas 6
- **PROC-007** → AI properties configurate la Pas 7
- **PROC-009** → buttons create la Pas 8
- **NOTION-SLACK-AUTOMATION** → Slack integration la Pas 9A
- **PROC-010** → forms la Pas 9B (conditionat)
- **NOTION-ZAPIER-PIPELINE-SETUP** → Zapier pipelines la Pas 9C (conditionat)
- `procedure-health.json` → entry adaugat

### VERIFY (procedural checkpoint)
La finalul executiei, agentul verifica:
- [ ] Procedura executata complet? (toti pasii Pas 1-10)
- [ ] Checklist Pas 10 executat si PASS pe toate items?
- [ ] Tipul de proiect clasificat corect?
- [ ] Toate DB-urile specifice tipului create?
- [ ] Linked views pe master DB-uri configurate cu filtre corecte?
- [ ] Dashboard layout conform template-ului din Pas 6?
- [ ] Cortex save executat?
- [ ] VK emis in sesiune?

**Doua VK-uri obligatorii per executie** (per VK-H-001):
1. **Execution VK**: `[PROC] NOTION-PROJECT-INFRASTRUCTURE-PIPELINE | type={TYPE} | slug={slug} | §1-§4 VER | complete`
2. **Save VK**: `[CORTEX] "NOTION-PROJECT-INFRASTRUCTURE-PIPELINE: {slug}" | FORGE | rule: PROJ-H-001 | v1.0`

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| PROJECT-CARD-ONBOARDING | Trigger la Pas 1 — Card trebuie sa existe | `~/.nexus/procedures/PROJECT-CARD-ONBOARDING.md` |
| PROC-008 (Workspace Architecture) | Pas 3 — setup ierarhie Notion | `~/.nexus/procedures/training/notion/PROC-008-NOTION-WORKSPACE-ARCHITECTURE.md` |
| NOTION-DATABASE-DESIGN-PATTERN | Pas 4 — design DB-uri locale | `~/.nexus/procedures/training/notion/NOTION-DATABASE-DESIGN-PATTERN.md` |
| NOTION-RELATION-ROLLUP-SETUP | Pas 5 — relatii + rollup-uri | `~/.nexus/procedures/training/notion/NOTION-RELATION-ROLLUP-SETUP.md` |
| PROC-006 (Home Dashboard) | Pas 6 — dashboard proiect | `~/.nexus/procedures/training/notion/PROC-006-NOTION-HOME-PAGE-DASHBOARD.md` |
| PROC-007 (AI Autofill) | Pas 7 — AI properties | `~/.nexus/procedures/training/notion/PROC-007-NOTION-AI-PROPERTY-AUTOFILL.md` |
| PROC-009 (Button Automation) | Pas 8 — buttons | `~/.nexus/procedures/training/notion/PROC-009-NOTION-BUTTON-WORKFLOW-AUTOMATION.md` |
| PROC-010 (Forms) | Pas 9B — forms (conditionat) | `~/.nexus/procedures/training/notion/PROC-010-NOTION-FORMS-DATA-COLLECTION.md` |
| NOTION-SLACK-AUTOMATION | Pas 9A — Slack (doar Business) | `~/.nexus/procedures/training/notion/NOTION-SLACK-AUTOMATION.md` |
| NOTION-ZAPIER-PIPELINE-SETUP | Pas 9C — Zapier (conditionat) | `~/.nexus/procedures/training/notion/NOTION-ZAPIER-PIPELINE-SETUP.md` |
| NOTION-FORMULA-CREATION | Formule pe KPI/Finance DB-uri | `~/.nexus/procedures/training/notion/NOTION-FORMULA-CREATION.md` |
| NOTION-SYNC | Pas 9D — sync bidirectional | `~/.nexus/procedures/NOTION-SYNC.md` |
| Notion workspace NEXUS | Workspace target | `notion.so` |
| Cortex API | Logging | `localhost:6400/api/store` |
| procedure-health.json | Registry proceduri | `~/.openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Infrastructure coverage | % proiecte active cu infrastructura Notion completa | 100% |
| Pipeline execution time | Timp total Pas 1-10 per proiect | Sub 45 minute |
| DB completeness | % DB-uri cu schema completa (Title+Status+Date+Owner+2 views) | 100% |
| Linked view accuracy | % linked views cu filter corect pe proiect | 100% |
| AI property coverage | % DB-uri text-heavy cu AI Summary | 100% (sau [AI-SKIP] documentat) |
| Zero DB duplicates | DB-uri duplicate per tip de entitate | 0 |
| Button test pass rate | % buttons testate si functionale | 100% |
| Slack automation coverage (Business) | % proiecte Business cu Slack automations | 100% |

---

## 7. Output per tip — Inventar complet infrastructura creata

### BUSINESS (ex: SMSads, Gambling SEO)
**Pagini create:** 1 pagina proiect + 1 wiki "Start Here"
**DB-uri locale:** KPI Dashboard DB + Finance Tracking DB
**Linked views pe master DB-uri:** Tasks (filtered) + Procedures (filtered) + Research (filtered) + Contacts (filtered)
**Rollup-uri pe Projects master DB:** Active Tasks Count + Procedures Count + Research Items Count
**Buttons:** + New Task, + New Note, + New Contact, + New KPI Entry, + Log Transaction
**AI Properties:** Summary pe Research linked view + KPI Dashboard DB
**Slack automations:** Tasks Done → canal, KPI Behind → #alerts, Finance threshold → #finance-alerts
**NOTION-SYNC:** push Intelligence Feed + pull task status
**Total elemente:** ~20-25

### PERSONAL (ex: Health, Travel, Learning)
**Pagini create:** 1 pagina proiect + 1 wiki "Start Here"
**DB-uri locale:** Goals DB + Habits Tracking DB + Notes DB
**Linked views pe master DB-uri:** Tasks (filtered) + Procedures (filtered) + Research (filtered)
**Rollup-uri pe Projects master DB:** Active Tasks Count + Procedures Count + Research Items Count
**Buttons:** + New Task, + New Note
**AI Properties:** Summary pe Notes DB + Research linked view
**Slack automations:** N/A
**NOTION-SYNC:** pull task status
**Total elemente:** ~12-15

### INFRASTRUCTURE (ex: OpenClaw, ECHELON, Cortex)
**Pagini create:** 1 pagina proiect + 1 wiki "Start Here"
**DB-uri locale:** Docs DB + Monitoring DB + Integrations Log DB
**Linked views pe master DB-uri:** Tasks (filtered) + Procedures (filtered) + Research (filtered)
**Rollup-uri pe Projects master DB:** Active Tasks Count + Procedures Count + Research Items Count
**Buttons:** + New Task, + New Doc, + Log Alert
**AI Properties:** Summary pe Docs DB + Research linked view
**Slack automations:** N/A (monitoring handled by ECHELON/scripts, not Notion)
**NOTION-SYNC:** push ECHELON findings + pull task status
**Total elemente:** ~15-18

---

## Checklist Pre-Publicare

- [x] Regula asociata exista (PROJ-H-001, META-H-002)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu checks obligatorii
- [x] Procesul din WHERE editat sa referentieze aceasta procedura (PROJECT-CARD-ONBOARDING Pas 1)
- [x] Entry de adaugat in `procedure-health.json`
- [x] De salvat in Cortex cu metadata FORGE
- [x] Descrie CE nu CUM (referinte la proceduri existente, nu cod inline)
- [x] VK format specificat (per VK-H-001)
- [x] Integrare cu PROJECT-CARD-ONBOARDING documentata (Pas 1 trigger)
- [x] Toate 10 proceduri Notion referentiate in pasii corecti
- [x] SuperDelphi D4 findings integrate (linked DBs, performance, AI properties, Smart Templates)
- [x] Output inventar per tip de proiect documentat (§7)
- [x] v1.1 (2026-03-05): FORGE-AUDIT fixes — §2b adaugat: pipeline state file (W2 fix), per-step error handling + degraded mode (W3 fix), rollback instructions (D6 fix)
