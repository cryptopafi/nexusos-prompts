---
type: procedure
created: 2026-03-20
status: active
slug: project-card-onboarding
tags: [procedure, nexus]
---

# PROJECT-CARD-ONBOARDING — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.2.1
**Regulă asociată**: PROJ-H-001 (orice proiect activ TREBUIE să aibă un Project Card)
**Scope**: Crearea și menținerea unui „Project Card" — fișier unic care centralizează TOTUL despre un proiect, astfel încât Genie să devină imediat operațional la comanda „încarcă proiectul X".

---

## 1. Problema

Fără un Project Card centralizat, la fiecare schimb de context Genie pierde 5-15 minute căutând prin Cortex, fișiere, Telegram, proceduri — reconstituind ad-hoc ce știe despre un proiect. Informațiile sunt fragmentate în:
- Cortex (10+ collections: knowledge, private_messages, research, gemini_activity, echelon, etc.)
- Fișiere locale (~/.nexus/procedures/training/*, memory/research/*, memory/projects-tracker.md)
- Mesaje TG/WA (ingerate dar nesintetizate)
- Google Sheets (accesate manual prin MCP)
- Research vechi (Gemini Canvase, SuperDelphi runs, NotebookLM)

**Fără procedura asta:**
- Genie întreabă de 3 ori aceleași lucruri
- Se pierd linkuri, decizii, contacte
- Proiectele personale nu au aceeași rigoare ca cele de business
- „Încarcă proiectul X" durează 10 min în loc de 10 secunde

---

## 2. Procedura

### MODEL ROUTING

| Pas | Executor | Condiție |
|-----|----------|---------|
| Pas 1 TRIGGER | Genie (Sonnet) | Automat la session start sau la cerere |
| Pas 2 DISCOVERY | Genie (Sonnet) pentru 2.1-2.2-2.4 + **Opus subagent** doar pentru 2.3 Nexus Research dacă D3 | D2 = Sonnet, D3 Nexus Research = Opus |
| Pas 3 SYNTHESIS | Genie (Sonnet) | Compilare directă |
| Pas 3.5 VERIFY | Genie (Sonnet) | Checklist automat post-synthesis |
| Pas 4 INGEST | Genie (Sonnet) | Cortex save + registry update |
| Pas 5 LOAD | Genie (Sonnet) | Citire Card + Brain Dump + discuție |
| Pas 6 UPDATE | Genie (Sonnet) | Inline la finalul sesiunii |
| Pas 7 BRAIN-DUMP | Genie (Sonnet) | Alimentare continuă între sesiuni |

---

### Pas 1: TRIGGER — Când se creează un Project Card

Se creează un Project Card când:
- **Proiect nou**: Pafi anunță un proiect nou → Card obligatoriu ÎNAINTE de orice altceva
- **Retroactiv**: Pafi zice „aplică Project Card pe X" sau „încarcă proiectul X" și Card-ul nu există
- **Session Start**: dacă proiectul activ nu are Card → flag Pafi

Card-ul se stochează la: `~/.nexus/projects/{SLUG}/PROJECT-CARD.md`
Brain Dump se stochează la: `~/.nexus/projects/{SLUG}/BRAIN-DUMP.md`
Slug = lowercase-kebab-case (ex: `smsads`, `gambling-seo`, `personal-hub`, `openclaw`)

La crearea unui proiect nou, se creează AMBELE fișiere (Card + Brain Dump).
Brain Dump template: vezi §10.

---

### Pas 2: DISCOVERY — Deep Research (prima creare)

**2.1 Cortex Scan** (obligatoriu, 5+ queries):
```
Query 1: "{project_name} {aliases}" (ex: "smsads clickwin click.win elevate marketing")
Query 2: "{key_people} {project_name}" (ex: "Leo Kody Bolivia MarathonBet")
Query 3: "{domain_keywords}" (ex: "SMS affiliate WAP billing VOX")
Query 4: collection=private_messages filter by project keywords
Query 5: collection=gemini_activity filter by project keywords
Query 6: collection=echelon filter by project keywords
Query 7: collection=research filter by project keywords
```
Collections to scan: knowledge, private_messages, gemini_activity, research, echelon, scout-intel, notebooklm_chats, sessions, procedures, training_procedures, skool, general

**2.2 File System Scan**:
```
- ~/.nexus/procedures/training/{domain}/ → list all procedures
- memory/research/*{project}* → list research files
- memory/projects-tracker.md → current status
- ~/.codex/genie-to-codex.md → related Codex tasks
- Radar configs → monitored sources for this project
- LaunchAgents → running automation
```

**2.3 Nexus Research External Scan** (dacă e proiect business/tehnic):
Folosește lista de întrebări din §6 mai jos. Adaptează la tipul de proiect.
Depth: D2 minim. D3 dacă e proiect nou fără context intern.

**2.4 Pafi Interview** (dacă lipsesc informații critice):
Întreabă DOAR ce nu poate fi găsit prin research. Max 5 întrebări. Folosește lista din §6.

**Zero results handling**: Dacă Discovery nu găsește rezultate relevante în niciuna din surse → informează Pafi, confirmă că proiectul e real, continuă cu Pafi Interview (Pas 2.4) ca sursă primară.

**Discovery VK** (obligatoriu la finalul Pas 2):
`🔍 [DISCOVERY] project={slug} | cortex_queries={N} | collections={N} | files_scanned={N} | nexus-research={D2|D3|SKIP} | interview={YES|NO}`

---

### Pas 3: SYNTHESIS — Compilare Project Card

Scrie fișierul `PROJECT-CARD.md` cu structura EXACTĂ din §5 (Template).

Reguli de compilare:
- **Fapte, nu opinii** — citează surse (Cortex ID, fișier, mesaj TG)
- **Linkuri absolute** — orice referință = path complet sau Cortex query
- **Rezumat executiv ≤ 10 rânduri** — restul în secțiuni detaliate
- **Nu duplica conținut** — link către fișier/Cortex entry, nu copy-paste
- **Marchează UNKNOWN** explicit — mai bine „UNKNOWN (de verificat cu Leo)" decât omisiune
- **Max 30 Comms Log entries** — cele mai recente, rezumate. Restul rămân în Cortex.

**Restricții Card (SAFETY)**:
- **NU pune** în Card: API keys, tokens, passwords, secrets de orice fel
- **NU pune** în Card: NDA content, contracte full-text (link, nu copy)
- **NU pune** în Card: mesaje TG/WA full-text (doar summary)
- Card-ul e un **index cu linkuri**, nu un dump de conținut

---

### Pas 3.5: VERIFY — Auto-verificare completitudine

Imediat după Synthesis, ÎNAINTE de Ingest, rulează acest checklist automat:

**A. Coverage Check — fiecare secțiune din Template:**
- [ ] Secțiunea are date reale SAU este marcată explicit `N/A` cu motiv?
- Dacă o secțiune e goală fără `N/A` → **STOP** → re-query Cortex targetat pe acel topic
- Secțiunile obligatorii per tip proiect (§7) NU pot fi `N/A`

**B. Cross-reference Check:**
- [ ] Fiecare persoană menționată în Card apare în **Team & Contacts**?
- [ ] Fiecare sumă/deal menționat apare în **Finances & Deals**?
- [ ] Fiecare tool/script menționat apare în **Tech Stack & Infra**?
- [ ] Fiecare decizie menționată în Comms Log are entry în **Decisions Log** (dacă majoră)?

**C. Source Completeness:**
- [ ] Toate Cortex results din Discovery au fost procesate? (verifică count query vs count entries)
- [ ] File system scan results au fost integrate? (procedures count, research count)
- [ ] Brain Dump entries (dacă există) au fost consolidate?

**D. Freshness & Accuracy:**
- [ ] Datele din Executive Summary reflectă cel mai recent status?
- [ ] Open Items includ TOT ce e blocat/pending din alte secțiuni?
- [ ] Niciun UNKNOWN care ar putea fi rezolvat cu o query suplimentară?

**Verify VK** (obligatoriu):
```
🔎 [VERIFY] project={slug} | sections_filled={N}/{TOTAL} | cross_ref_pass={YES|NO} | gaps_found={N} | requery_needed={YES|NO}
```

Dacă `gaps_found > 0` și `requery_needed=YES` → execută queries suplimentare → re-run Verify.
**Max 2 re-query cycles.** Dacă după 2 cycles gaps persistă → marchează UNKNOWN cu motiv și continuă.

---

### Pas 4: INGEST — Salvare Cortex + Actualizare Registru

4.1 Salvează Card-ul în Cortex:
```json
{
  "text": "{executive_summary}",
  "collection": "projects",
  "metadata": {
    "type": "project_card",
    "project": "{slug}",
    "version": "1.0",
    "last_updated": "{ISO_DATE}",
    "status": "{ACTIVE|PAUSED|DONE}",
    "genie_visible": true
  }
}
```

**Error handling**:
- Cortex save fail → retry 1x. Dacă fail persistent → Card rămâne local, flag Pafi, skip Cortex entry temporar.
- File write fail (disk/permissions) → verifică spațiu + permisiuni, retry. Flag Pafi dacă persistă.
- projects-tracker.md git conflict → rezolvă conflictul manual, apoi update.
- **Idempotency**: Înainte de Cortex save, query `collection=projects type=project_card project={slug}`. Dacă entry există → update metadata (version, last_updated). Dacă nu → create new.

4.2 Actualizează `memory/projects-tracker.md` cu link la Card.
4.3 Actualizează `memory/MEMORY.md` dacă proiectul e activ.

---

### Pas 5: LOAD — Cum se încarcă un proiect existent

Când Pafi zice „încarcă proiectul X" / „load project X" / „switch to X":
1. Citește `~/.nexus/projects/{slug}/PROJECT-CARD.md`
2. Citește `~/.nexus/projects/{slug}/BRAIN-DUMP.md` (dacă există și are entries ne-consolidate)
3. Verifică freshness per status:

| Status proiect | Freshness threshold | Acțiune dacă depășit |
|---------------|--------------------|--------------------|
| ACTIVE / IN PROGRESS | 7 zile | Pas 2 light (Cortex queries 1-3 + file scan) |
| EXPLORING | 14 zile | Pas 2 light |
| PAUSED | 30 zile | Pas 2 light |
| DONE | Nu se verifică | — |

4. Afișează Executive Summary + Open Items
5. **Brain Dump Processing** (dacă există entries ne-consolidate):
   - Prezintă entries ne-consolidate lui Pafi
   - Pentru fiecare entry: cere lămuriri dacă e ambiguu, propune unde se integrează în Card
   - Deschide topics de discuție dacă entries sugerează direcții noi
   - Face propuneri concrete bazate pe entries (ex: „ai notat că X — vrei să investigăm?")
   - După implementare și validare cu Pafi: **șterge entry-ul din Brain Dump** (evită bloat)
6. Genie e operațional — nu mai caută nimic

---

### Pas 6: UPDATE — Menținere continuă

Când se întâmplă ceva relevant pentru proiect:
- Decizie majoră → adaugă în §Decisions
- Task Codex livrat → adaugă în §Deliveries
- Mesaj TG/WA important → adaugă în §Comms Log (doar summary, nu full text)
- Research nou → adaugă în §Research

**Frecvență minimă**: update Card la fiecare sesiune care atinge proiectul.
**Full refresh**: la cerere sau la fiecare 30 zile automat.

**Update VK** (obligatoriu la fiecare update):
```
🔄 [PROJECT-CARD] UPDATE project={slug} | sections_updated={list} | cortex_saved=YES|NO
```

### Pas 7: BRAIN-DUMP — Alimentare continuă

Brain Dump-ul e un fișier live unde se acumulează informații nestructurate între sesiuni.

**Cine alimentează:**
- **Pafi/Leo manual**: oricând, cu orice format — idei, linkuri, observații, frustrări, viziuni
- **Genie automat**: când lucrează pe alt proiect și dă peste info relevantă pentru acest proiect
- **Genie din Radar/Cortex**: dacă Radar finding sau search result e relevant pentru proiect

**Format entry** (Genie adaugă):
```markdown
### {YYYY-MM-DD HH:MM} | {SOURCE}
{conținut liber}
**Tags**: #{tag1} #{tag2}
```

**Format entry** (Pafi adaugă — fără format impus):
```markdown
### {YYYY-MM-DD}
{orice text, orice format}
```

**Reguli Brain Dump:**
- NU consolida fără discuție cu Pafi — Brain Dump entries pot avea context pe care Genie nu-l vede
- **Workflow**: citește entry → propune implementare → implementează → validare Pafi → **șterge entry** (anti-bloat)
- Entries mai vechi de 90 zile neimplementate → flag inline la LOAD: `⚠️ Brain Dump: {N} entries mai vechi de 90 zile — implementăm sau ștergem?`
- Brain Dump NU înlocuiește Card-ul — e un inbox, nu un document de referință
- La LOAD: citește Brain Dump DUPĂ Card, prezintă entries, propune implementare, discută

---

## 3. Cortex Logging

La fiecare creare/update de Project Card:
```json
{
  "text": "Project Card {ACTION} for {project}: {one_line_summary}",
  "collection": "projects",
  "metadata": {
    "type": "project_card_event",
    "project": "{slug}",
    "action": "created|updated|refreshed",
    "sections_updated": ["team", "finances", "deliveries"],
    "genie_visible": true
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- WISH Step 1 (W — What): dacă task-ul e legat de un proiect → verifică Card
- **Session Start Step 7 (Briefing)**: dacă proiect activ → verifică freshness Card (integrat în CLAUDE.md Step 7 — PROJ-H-001)

### WHEN
- La fiecare task legat de un proiect → Card TREBUIE să existe
- La session start → dacă Card outdated per freshness table (Pas 5) → flag

### HOW (violation detection)
- Genie lucrează pe proiect fără Card încărcat → violation PROJ-H-001
- Card depășit freshness threshold fără refresh → flag la session briefing
- Proiect în projects-tracker.md fără Card → flag

### VERIFY
- [ ] Card-ul există la `~/.nexus/projects/{slug}/PROJECT-CARD.md`
- [ ] Executive Summary ≤ 10 rânduri
- [ ] Toate secțiunile din Template sunt completate (UNKNOWN acceptat, lipsa secțiunii = violation)
- [ ] Cortex entry există în collection `projects`
- [ ] projects-tracker.md are link la Card
- [ ] Brain Dump fișier există la `~/.nexus/projects/{slug}/BRAIN-DUMP.md`
- [ ] Brain Dump entries ne-consolidate au fost discutate la LOAD
- [ ] Procedura executată complet? (toți 7 pașii)
- [ ] Output-ul satisface criteriile din §1? (Card centralizat, operațional sub 10 secunde)
- [ ] VK emis?

### VK-uri obligatorii

**Execution VK** (la finalul creării/update-ului Card):
```
✅ [PROJECT-CARD] {ACTION} project={slug} | sections={N}/{TOTAL} | cortex_saved=YES | tracker_updated=YES | freshness=OK
```

**Save VK** (la Cortex save):
```
💾 [CORTEX] Project Card: {slug} v{version} | collection=projects | PROJ-H-001 ✓
```

### CONNECT
- META-H-002 (FORGE) → acest template respectă FORGE 1.3
- MEM-H-001 (Cortex save) → Card salvat în Cortex
- RESEARCH-H-001 (Radar extraction) → dacă Discovery găsește surse noi → add to Radar
- **CLAUDE.md Step 7** → verificare freshness Card la session briefing

---

## 5. Template — PROJECT-CARD.md

```markdown
# {PROJECT_NAME} — Project Card

**Slug**: {slug}
**Status**: ACTIVE | PAUSED | DONE | EXPLORING
**Owner**: {Pafi / Leo / shared}
**Start date**: {YYYY-MM-DD}
**Last updated**: {YYYY-MM-DD}

---

## Executive Summary
{Max 10 rânduri. Ce e proiectul, unde suntem, ce urmează imediat.}

---

## Identity
- **Entitate legală**: {firmă, jurisdicție}
- **Website/URLs**: {links}
- **Aliases**: {nume anterioare, domenii vechi}
- **Repo(s)**: {GitHub links}

## Team & Contacts
| Persoană | Rol | Contact | Notes |
|----------|-----|---------|-------|
| {Nume} | {Rol} | {TG/WA/email} | {ce face} |

## Business Model
- **Revenue model**: {CPA/RevShare/SaaS/etc.}
- **Key metrics**: {KPIs principale}
- **Current revenue**: {dacă e cunoscut}
- **Partners/Platforms**: {VOX, 3SNET, etc.}

## Markets & Audiences
| Market | Status | Audience Size | Notes |
|--------|--------|--------------|-------|
| {Țară} | {active/planned/blocked} | {N} | {detalii} |

## Active Campaigns
| Campaign | Platform | Status | Results |
|----------|----------|--------|---------|
| {Nume} | {MarathonBet etc.} | {running/paused} | {clicks, conversions} |

## Finances & Deals
- **Agreements**: {contracte, termeni — LINK, nu text}
- **Revenue splits**: {cine primește cât}
- **Costs**: {monthly burn, tools, infra}

## Timeline & Milestones
| Date | Milestone | Status |
|------|-----------|--------|
| {YYYY-MM-DD} | {Ce trebuie atins} | {done/pending/blocked} |

## Risks & Blockers
| Risk | Impact | Mitigation | Status |
|------|--------|------------|--------|
| {Ce poate merge prost} | {HIGH/MED/LOW} | {Ce facem} | {open/mitigated} |

## Project Dependencies
| Depends on | Type | Status | Notes |
|-----------|------|--------|-------|
| {Alt proiect / resursă externă} | {blocker/nice-to-have} | {resolved/open} | — |

## Decisions Log
| Date | Decision | Context |
|------|----------|---------|
| {YYYY-MM-DD} | {Ce s-a decis} | {De ce} |

## Procedures (linked)
{Lista de proceduri relevante cu path-uri absolute}
- `~/.nexus/procedures/training/{domain}/{file}.md`

## Research (linked)
{Lista de research files + Cortex entries}
- `memory/research/{file}.md`
- Cortex: `{collection}` query "{query}" → {N results}
- Gemini Canvas: "{title}"

## Radar — Monitored Objects
{Lista completă a obiectelor monitorizate activ pentru acest proiect.}

### Competitors
| Competitor | URL/Brand | What to Monitor | Tool | Frequency |
|-----------|-----------|-----------------|------|-----------|
| {Nume} | {URL} | {pricing/content/features/ads} | {ECHELON/Nexus Research/manual} | {daily/weekly} |

### Inbox Channels
| Channel | Type | Account/ID | Monitor For | Frequency |
|---------|------|-----------|-------------|-----------|
| {Gmail} | email | {account} | {client emails, invoices, alerts} | {realtime/daily} |
| {WhatsApp} | messaging | {group/contact} | {partner updates, client requests} | {realtime} |
| {Telegram} | messaging | {group/channel/DM} | {industry news, team updates} | {realtime} |

### Online Sources
| Source | Type | URL/ID | What to Track | Frequency |
|--------|------|--------|---------------|-----------|
| {Google Sheet} | spreadsheet | {URL} | {KPIs, revenue data} | {daily/weekly} |
| {Notion DB} | database | {DB ID} | {tasks, pipeline} | {realtime} |
| {RSS/Blog} | feed | {URL} | {industry updates} | {daily} |
| {Social} | platform | {handle/page} | {mentions, engagement} | {daily} |

### Files & Documents
| Document | Location | What to Track | Last Checked |
|----------|----------|---------------|-------------|
| {Contract PDF} | {Drive/local path} | {renewal dates, terms} | {date} |
| {Strategy doc} | {Notion/Cortex} | {updates, changes} | {date} |

---

## Scheduled Actions
{Acțiuni recurente care trebuie executate pe acest proiect. Genie le verifică la session start.}

| Action | Frequency | Time | Tool/Script | Output |
|--------|-----------|------|-------------|--------|
| ECHELON scan pe Radar list | Daily | 06:00 | ECHELON channels | New findings → Brain Dump |
| Run INSIGHT pe findings | Daily | Post-ECHELON | SuperInsight | Synthesized intel → Cortex |
| Inbox triage (email/TG/WA) | Daily | 07:00-08:30 | gmail-inbox-pull + relay | Prioritized items → Pafi |
| Competitor check | Weekly | Monday | Nexus Research/ECHELON | Competitor moves → Card §Radar |
| KPI review | Weekly | Friday | Sheets/Notion pull | Metrics update → Card §Business Model |
| Card freshness refresh | Per freshness table (Pas 5) | Session start | Genie auto-check | Updated Card if stale |
| Brain Dump consolidation | At each LOAD | — | Genie | Entries → Card sections |
| Cortex knowledge sync | Weekly | — | Cortex search | New relevant entries → Card |

**Customize per project**: Nu toate acțiunile se aplică la fiecare proiect. La Card creation, marchează cu ✅/❌ care sunt active.

## Tech Stack & Infra
- **Scripts**: {paths}
- **LaunchAgents**: {labels}
- **VPS services**: {ports, systemd units}
- **APIs/Tools**: {ce se folosește}

## Codex History
| Task | Status | Description |
|------|--------|-------------|
| m4-XXX | DONE/PENDING/ERROR | {one-liner} |

## Comms Log (recent, max 30 entries, summary only)
| Date | From | Channel | Summary |
|------|------|---------|---------|
| {YYYY-MM-DD} | {Cine} | TG/WA/Email | {Ce s-a zis pe scurt} |

## Open Items / Next Steps
- [ ] {Ce trebuie făcut next}
- [ ] {Blocker sau întrebare deschisă}

## Raw Cortex References
{IDs Cortex pentru acces rapid la date brute}
- `{cortex_id}` — {description}
```

---

## 6. Discovery Questions (pentru Nexus Research + Pafi Interview)

### Business Projects — Nexus Research Queries
1. "{project_name} business model revenue pricing 2026" — cum fac bani alții în nișa asta?
2. "{project_name} competitors alternatives market size" — cine mai e pe piață?
3. "{industry} compliance regulations {target_countries} 2026" — ce reglementări se aplică?
4. "{industry} best practices conversion rates benchmarks" — ce KPIs sunt normale?
5. "{technology_stack} tutorials setup guide production" — cum se implementează stack-ul?
6. "{industry} legal risks lawsuits penalties {target_countries}" — ce riscuri legale există?

### Personal Projects — Nexus Research Queries
1. "{topic} best tools apps 2026" — ce tool-uri folosesc alții?
2. "{topic} workflow automation integration" — cum automatizez?
3. "{topic} community forums experts to follow" — unde învăț mai mult?
4. "{skill} learning path roadmap beginner to advanced" — care e drumul?

### Pafi Interview — Întrebări standard (max 5, doar ce lipsește)
1. **Cine**: Cine mai e implicat? Ce rol are fiecare?
2. **Bani**: Revenue model? Cine plătește pe cine? Revenue split?
3. **Status real**: Ce merge acum, ce nu merge? Ce e blocat și de ce?
4. **Prioritate**: Care e cel mai important next step? Ce deadline avem?
5. **Viziune**: Unde vrei să ajungă proiectul în 3 luni? Ce ar fi succesul?
6. **Legal/Compliance**: Sunt riscuri legale? NDA-uri? Reglementări specifice?

### Tech Discovery — File System Checklist
1. Există repo? → `find ~/repos -name "*{slug}*"`
2. Există proceduri? → `ls ~/.nexus/procedures/training/{domain}/`
3. Există Radar sources? → grep în radar*.js
4. Există LaunchAgents? → grep în ~/Library/LaunchAgents/
5. Există Codex tasks? → grep în genie-to-codex.md + codex-to-genie.md
6. Există VPS services? → ssh check systemd
7. Există Google Sheet/Notion DB? → check MCP configs

---

## 7. Tipuri de proiecte suportate

| Tip | Exemplu | Secțiuni obligatorii | Secțiuni opționale |
|-----|---------|---------------------|-------------------|
| **Business** | SMSads, Gambling SEO | Toate | — |
| **SaaS/Product** | OpenClaw, Nexus | Toate minus Campaigns | — |
| **Client Work / Freelance** | Leo SEO projects | Toate minus Markets | — |
| **Investment / Due Diligence** | Crypto analysis | Identity, Research, Risks, Finances, Open Items | Team, Campaigns, Tech Stack |
| **Partnership** | Vox collaboration | Identity, Team, Finances, Decisions, Open Items | Markets, Tech Stack |
| **Personal Learning** | Notion expertise, MBA | Identity, Research, Procedures, Open Items | Markets, Finances |
| **Side Project** | Personal Hub, ECHELON | Identity, Tech Stack, Open Items | Team, Finances |
| **Research** | Nexus Research cycles, Longevity | Identity, Research, Open Items | Restul |

---

## 8. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| FORGEBUILD.md | Template standard | `~/.nexus/procedures/FORGEBUILD.md` |
| Cortex | Storage + search | `http://localhost:6400` |
| Nexus Research | External research la Discovery | `/nexus [topic]` sau WISH Step I |
| Radar | Monitorizare continuă surse proiect | `~/repos/course-extractor-m4/src/radar*.js` |
| projects-tracker.md | Registru central proiecte | `memory/projects-tracker.md` |
| CLAUDE.md | Session Start Step 7 enforcement | `.claude/projects/-Users-pafi/CLAUDE.md` |

---

## 9. Checklist Pre-Publicare

- [ ] Toate secțiunile FORGE 1.3 prezente (§1-§4 + §5+)
- [ ] VK-uri definite (Execution + Save)
- [ ] MODEL ROUTING specificat
- [ ] Enforcement Loop complet (WHERE/WHEN/HOW/VERIFY/CONNECT)
- [ ] Template §5 testat pe un proiect real (SMSads ✅)
- [ ] Discovery Questions acoperă business + personal + legal
- [ ] Safety constraints definite (ce NU se pune în Card)
- [ ] PROJ-H-001 adăugat în rules-hard.md
- [ ] CLAUDE.md Step 7 actualizat cu Card freshness check
- [ ] Brain Dump template creat pentru proiect test

---

## 10. Template — BRAIN-DUMP.md

```markdown
# {PROJECT_NAME} — Brain Dump

**Proiect**: {slug}
**Card**: `~/.nexus/projects/{slug}/PROJECT-CARD.md`

> Acest fișier e un inbox nestructurat. Scrie orice, oricând.
> Genie va citi la fiecare LOAD, va propune implementare și va șterge entries după validare.

---

### {YYYY-MM-DD}
{notă liberă}
```

### Brain Dump — Reguli de consolidare la LOAD

1. **Citește** toate entries fără tag `[CONSOLIDATED]`
2. **Grupează** pe teme (dacă sunt 3+ entries pe aceeași temă → sugerează secțiune Card)
3. **Întreabă** pentru fiecare entry ambiguu: „Ai notat X — ce ai vrut să zici? Vrei să..."
4. **Propune** acțiuni concrete: research, Codex task, decizie de luat, update Card
5. **Deschide discuție** dacă entry sugerează o direcție nouă sau o schimbare de strategie
6. **Integrează** în Card secțiunile relevante (cu acordul lui Pafi)
7. **Șterge** entry-ul din Brain Dump DUPĂ implementare și validare cu Pafi (anti-bloat)
8. **NU implementa** automat fără discuție — Brain Dump entries pot avea nuanțe pe care Genie nu le vede
