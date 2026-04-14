---
type: procedure
created: 2026-03-17
status: active
slug: notion-sync
tags: [procedure, nexus]
---

# NOTION-SYNC — Bidirectional Notion Portal for Business & Personal Intelligence

**Version**: 1.2
**Date**: 2026-03-01
**Scope**: Creare portal Notion complet cu pagini per topic, populat din Cortex/ECHELON/SuperInsight, cu sync bidirecțional
**Owner**: Genie
**FORGE**: META-H-002

---

## 1. Trigger

- Pafi cere vizualizare centralizată a tuturor business-urilor și proiectelor personale
- SuperInsight nou generat → push automat la pagina topic-ului relevant
- ECHELON daily digest → push la LIVE INTELLIGENCE
- Pafi modifică task/status în Notion → pull la session start

---

## 2. Prerequisites

### 2.1 Fix Notion MCP Token (BLOCKER)
Token-ul din `~/.claude/.mcp.json` e expirat (401). Înainte de orice:
1. Citește token valid din Keychain: `security find-generic-password -a "notion-api" -s "genie-keys" -w`
2. Actualizează `~/.claude/.mcp.json` → `OPENAPI_MCP_HEADERS.Authorization`
3. Testează: `mcp__notion__API-get-self` → trebuie să returneze user info

### 2.2 Existing Notion Structure (din Research)
```
NEXUS (root: 306d31d1-1b7d-807e-b65e-d3170cbb8807)
├── BUSINESSES (311d31d1-...-0637c0)
│   ├── Clickwin.vip → RENAME to SMSAds
│   ├── Albastru ✅
│   ├── Solnest.ai ✅
│   ├── AI B2B ✅
│   └── Proiect Nou → DELETE or repurpose
├── INFRASTRUCTURE ✅ (7 pagini, complet)
├── LIVE INTELLIGENCE ✅ (3 DB-uri)
├── TASK COMMAND ✅ (3 DB-uri)
├── KNOWLEDGE BASE ✅
├── SYSTEM OPERATIONS ✅
└── DISCUSSIONS ✅
```

### 2.3 Topics To Add
**Business (lipsește 1)**:
- AI Automations (cea mai mare colecție, ~2000 items)

**Training (hub complet nou — 5 sub-pagini)**:
- Finance (Cortex data exists)
- Legal (nou)
- Marketing (nou)
- Sales (nou)
- Accounting (nou)

**Personal (hub complet nou — 5 sub-pagini)**:
- Personal Dashboard (general)
- Health & Fitness (Cortex: 4 items)
- Travel (gol)
- Knowledge Vault (nou)
- Crypto/Trading (folder + SuperInsight + Cortex data exist — mutat din Business)

**Infrastructure (2 pagini noi sub hub existent)**:
- SKILLS → Procedures + Skills (sub-pagini)
- ECHELON Radar (toate sursele monitorizate)

---

## 3. Design — Ierarhie Notion Propusă

### 3.1 Modificări la structura existentă
| Acțiune | Pagina | Detaliu |
|---------|--------|---------|
| RENAME | Clickwin.vip | → **SMSAds.net** |
| DELETE | Proiect Nou [Fără Nume] | Placeholder gol |
| ADD | 1 pagină business nouă | AI Automations |
| ADD | TRAINING hub (secțiune nouă) | Sub NEXUS root — 5 sub-pagini: Finance, Legal, Marketing, Sales, Accounting |
| ADD | Personal Hub (secțiune nouă) | Sub NEXUS root — 5 sub-pagini: Personal Dashboard, Health & Fitness, Travel, Knowledge Vault, Crypto/Trading |
| ADD | SKILLS sub INFRASTRUCTURE | 2 sub-pagini: Procedures, Skills |
| ADD | ECHELON Radar sub INFRASTRUCTURE | Toate sursele monitorizate |

### 3.2 Template pagină topic — 3 DB-uri obligatorii + secțiuni standard

**ORICE pagină topic (business, personal, infrastructure, training) TREBUIE să aibă:**

#### DB-uri obligatorii (child databases, accesibile direct din pagină):
1. **📋 Procedures DB** — proceduri specifice acestui topic
2. **🎯 Skills DB** — skills relevante acestui topic
3. **🔬 Research DB** — rapoarte de research, YouTube, articole, studii ingerate manual sau auto

**Research DB — proprietăți standard:**
- Name (title), Type (YouTube/Study/Article/Book/Podcast/Website), Author, Status (Raw/Summarized/Verified/Actionable), Priority (High/Medium/Low), Topics (multi_select), Date, URL, Bias Flag (checkbox)

#### Secțiuni în pagină (content blocks, după DB-uri):
```
# [Topic Name]
Status: Active / Paused / Planning
Ultima actualizare: [auto-updated]

---
[📋 Procedures DB]
[🎯 Skills DB]
[🔬 Research DB]
---

## TLDR
- [Max 5 bullet points — ce e important acum]

---

## Task-uri Active
- [ ] Task 1 (prioritate, deadline)
[Sincronizat din pafi-tasks.md — filtrare per topic tag]

---

## Intelligence Feed
[Ultimele 10 items de la ECHELON/Radar/SuperInsight]
| Data | Sursa | TLDR | Grade |
|------|-------|------|-------|

---

## Actionable Items
[Extrase din Research DB + SuperInsight Section 6]
- [ ] Item 1 — din Research [data] — prioritate: HIGH

---

## Cortex Links
[Top 5 cele mai relevante items din Cortex KB]
```

**Audit check**: O pagină e completă DACĂ și NUMAI DACĂ are toate cele 3 DB-uri create ca child_database blocks.

### 3.3 Ierarhia finală propusă
```
NEXUS
├── 💼 BUSINESSES
│   ├── SMSAds.net (RENAME din Clickwin)
│   ├── Albastru
│   ├── Solnest.ai
│   ├── AI B2B Agency
│   └── AI Automations (NOU)
│
├── 📚 TRAINING (NOU)
│   ├── Finance
│   ├── Legal
│   ├── Marketing
│   ├── Sales
│   └── Accounting
│
├── 👤 PERSONAL (NOU)
│   ├── Personal Dashboard
│   ├── Health & Fitness
│   ├── Travel
│   ├── Knowledge Vault
│   └── Crypto/Trading (NOU — mutat din Business)
│
├── ⚙️ INFRASTRUCTURE
│   ├── ... (paginile existente, fără modificări)
│   ├── 🛠️ SKILLS (NOU)
│   │   ├── Procedures
│   │   └── Skills
│   └── 📡 ECHELON Radar (NOU — toate sursele monitorizate)
│
├── 📊 LIVE INTELLIGENCE (fără modificări)
├── 📋 TASK COMMAND (fără modificări)
├── 🧠 KNOWLEDGE BASE (fără modificări)
├── 🏗️ SYSTEM OPERATIONS (fără modificări)
└── 💬 DISCUSSIONS (fără modificări)
```

---

## 4. Pipeline — Sync Bidirecțional

### 4.1 Genie → Notion (PUSH)
| Trigger | Ce se trimite | Unde în Notion | Script |
|---------|--------------|----------------|--------|
| SuperInsight completat | TLDR + full report + actionable items | Topic page → Intelligence Feed + Research Reports + Actionable Items | `notion-push-superinsight.sh` (NOU) |
| ECHELON daily digest | Digest sumar per canal | LIVE INTELLIGENCE → ECHELON Reports DB | `echelon-digest.py` (EXISTENT) |
| Task status change | Task updated/completed | Topic page → Task-uri Active | `notion-sync-tasks.sh` (NOU) |
| Delphi research | Full research report cu TLDR | Topic page → Research Reports | `notion-push-research.sh` (NOU) |

### 4.2 Notion → Genie (PULL)
| Trigger | Ce se citește | De unde | Destinație locală |
|---------|--------------|---------|-------------------|
| Session start | Modified pages (last 24h) | Notion MCP → search recent | pafi-tasks.md update |
| Session start | New tasks added by Pafi | TASK COMMAND → Human Tasks DB | pafi-tasks.md + Codex queue |
| Session start | Status changes pe business | BUSINESSES pages | projects-tracker.md |
| Manual `/notion-pull` | Full sync | All topic pages | Local files update |

### 4.3 Conflict Resolution Strategy (F2 fix)

**Source of Truth**: `pafi-tasks.md` (local) is PRIMARY for tasks. Notion is PRIMARY for manual notes/TLDR edits.

| Conflict Type | Resolution | Rationale |
|---------------|-----------|-----------|
| Task status differs (local vs Notion) | **Notion wins** — Pafi edits there manually | User intent > automation |
| Task exists locally but not in Notion | Push to Notion (new task) | Local is source of creation |
| Task exists in Notion but not locally | Pull to local (Pafi created in Notion) | Respect manual additions |
| Both modified same field since last sync | **Notion wins** + log conflict to `~/.nexus/sync/conflicts.log` | Prefer human edits, keep audit trail |
| Intelligence/Research content | **Local wins** (append-only to Notion) | Generated content is authoritative locally |
| TLDR/notes manually edited in Notion | **Notion wins** — never overwrite manual edits | Respect user editorial intent |

**Conflict Detection**: Compare `last_edited_time` from Notion API with local `last_sync_timestamp` stored in `~/.nexus/sync/sync-state.json`.

**Conflict Log Format**:
```
[2026-03-01T10:00:00] CONFLICT: task "SMSAds audit" — local=IN_PROGRESS, notion=DONE — resolved: Notion wins
```

### 4.4 Sync Flow Diagram
```
SuperInsight/ECHELON/Delphi
    │
    ▼
notion-push-superinsight.sh ──────► Notion Topic Page
    │                                     │
    │                                     │ (Pafi edits in Notion)
    │                                     │
    ▼                                     ▼
~/.nexus/business/<topic>/     Notion MCP pull at session start
    │                                     │
    ▼                                     ▼
Cortex KB (backup)              pafi-tasks.md updated
```

---

## 5. Error Handling & Recovery (F1 fix)

### 5.1 Per-Step Error Handling
| Step | Failure Mode | Detection | Recovery |
|------|-------------|-----------|----------|
| MCP token update | 401 persists after update | `mcp__notion__API-get-self` returns error | Re-read Keychain → retry once → if still fails, STOP and notify Pafi on Telegram |
| Page creation | API error (400/429/500) | HTTP status check | 429: wait 60s + retry (max 3) / 400: log error + skip page + continue / 500: retry 2x then skip |
| Cortex query | VPS unreachable | Connection timeout (10s) | Use cached data from `~/.nexus/business/<topic>/` local files. Mark page as "Cortex Offline — Partial Data" |
| Push script | Partial write (some blocks written, others fail) | Track block IDs written | On failure: delete partially written blocks (rollback) → retry full push |
| Pull at session start | MCP unavailable | Tool call fails | Skip Notion pull silently, log warning, proceed with local data only |
| Bidirectional sync | Conflict detected | Timestamps mismatch (§4.3) | Apply conflict resolution table, log all conflicts |

### 5.2 Rollback Plan
- **Before any page modification**: snapshot current page content via `mcp__notion__API-get-block-children`
- **Store snapshots**: `~/.nexus/sync/snapshots/<page_id>_<timestamp>.json`
- **Retention**: Keep last 3 snapshots per page (auto-cleanup older)
- **Rollback trigger**: Manual `/notion-rollback <page_id>` command or automatic on 3+ consecutive push failures

### 5.3 Degraded Mode
| Component Down | Behavior |
|---------------|----------|
| Notion API | All push/pull queued to `~/.nexus/sync/pending-queue.json`, processed next session |
| Cortex KB | Pages created with local data only, "Cortex Offline" marker, re-populate when back |
| Both | Session proceeds normally without Notion sync, warning in briefing |

---

## 6. Implementation Steps

### Step 1: Fix MCP Token
- Read token: `security find-generic-password -a "notion-api" -s "genie-keys" -w`
- Update `~/.claude/.mcp.json` → `OPENAPI_MCP_HEADERS.Authorization` = `Bearer <token>`
- Verify: `mcp__notion__API-get-self` → must return user info
- **On failure**: Stop execution. Notify Pafi. Do not proceed to Step 2.

### Step 2: Rename + Cleanup
- Rename Clickwin.vip → SMSAds.net (page ID: `311d31d1-1b7d-8193-a12b-ca4dd8aceda2`)
- Delete/archive "Proiect Nou [Fără Nume]" (confirm with Pafi first per §8 Guards)

### Step 3: Create Missing Business Page + Training Hub
**Business** (1 page under BUSINESSES `311d31d1-1b7d-8142-b211-f0833c0637c0`):
- AI Automations — populate from Cortex (~2000 items, top 10) + pafi-tasks.md

**Training Hub** (new section under NEXUS root `306d31d1-1b7d-807e-b65e-d3170cbb8807`):
- Create TRAINING parent page
- Create 5 sub-pages: Finance, Legal, Marketing, Sales, Accounting
- Finance: populate from Cortex data. Others: 7-section template with "Needs Intelligence" marker.
- **On 429**: wait 60s, retry. **On other error**: skip page, log, continue to next.

### Step 4: Create Personal Hub + Infrastructure Pages
**Personal Hub** (new section under NEXUS root):
- Create PERSONAL parent page
- Create 5 sub-pages: Personal Dashboard, Health & Fitness, Travel, Knowledge Vault, Crypto/Trading
- Populate: Health (4 Cortex items), Crypto/Trading (SuperInsights + Cortex data), others with "Needs Intelligence" marker

**Infrastructure additions** (under existing INFRASTRUCTURE):
- Create SKILLS page → 2 sub-pages: Procedures, Skills
- Create ECHELON Radar page (sursele monitorizate — populate pe parcurs)

### Step 5: Populate Existing Pages
- SMSAds: 3 SuperInsights + 481 intel items (top 10) + 1 active task
- Albastru: 53 Cortex items (top 5) + 1 active task
- Solnest: 189 Cortex items (top 5) + 1 task (BLOCKED)
- AI B2B: 68 Cortex items (top 5) + 1 active task
- **Rate limit**: max 3 API calls/second, use 400ms sleep between calls

### Step 6: Create Push/Pull Scripts (Codex briefs)
Scripts use Notion MCP tools directly (no separate `notion-lib.sh` — Genie calls MCP inline). Codex briefs for:
- `notion-push-superinsight.sh` — triggered after each SuperInsight, pushes to correct topic page
- `notion-sync-tasks.sh` — syncs pafi-tasks.md ↔ Notion TASK COMMAND DB
- `notion-pull-session.sh` — reads modified pages (last 24h), applies conflict resolution (§4.3), updates local files

### Step 7: Add Pull Logic to Session Start
- Extend Genie session start checklist (Step 2 in CLAUDE.md) to include Notion pull
- Read recently modified Notion pages via MCP
- Apply conflict resolution per §4.3
- Update local task list with any changes

### Step 8: Validate (Happy Path + Failure Scenarios)
**Happy path:**
- Modify a task in Notion → verify appears in pafi-tasks.md
- Generate a SuperInsight → verify appears in Notion topic page
- Check all 12 topic pages have real content (not placeholders)

**Failure scenarios:**
- Disconnect Cortex → verify pages still create with local data + "Cortex Offline" marker
- Push with invalid token → verify error logged, rollback clean, Telegram notification sent
- Create conflicting edits (local + Notion) → verify conflict resolution applied per §4.3

---

## 7. Criterii de Succes

1. Minimum 17 pagini topic (5 business + 5 training + 5 personal + 2 infrastructure) cu toate 7 secțiunile
2. Fiecare pagină are cel puțin: TLDR, task-uri, și intelligence items (unde există date)
3. Push funcțional: SuperInsight nou → apare automat în Notion
4. Pull funcțional: modificare în Notion → reflectată în sesiunea Genie
5. Zero pagini goale (cele fără date au marker "Needs Intelligence")
6. Pafi deschide Notion → vede instant statusul fiecărui business

---

## 8. Limits & Guards

- **Nu șterge** nimic din Notion fără confirmare Pafi
- **Nu crea DB-uri noi** dacă Gen2 DB acoperă nevoia — refolosește
- **Max items per push**: 20 (previne spam în Notion)
- **Pull frequency**: doar la session start (nu polling continuu)
- **Rate limit Notion API**: max 3 requests/second

---

## 9. Dependencies

- Notion MCP server (token valid — Keychain: service=`genie-keys`, account=`notion-api`)
- Cortex KB (VPS 100.81.233.9:6400) — degraded mode without it (§5.3)
- pafi-tasks.md (task source)
- ingest-smart-config.json (topic routing)
- Codex (for push/pull scripts creation)
- `~/.nexus/sync/` directory (created at first run): `sync-state.json`, `conflicts.log`, `pending-queue.json`, `snapshots/`

### Token Expiry Monitoring (F4 fix)
- At each session start pull: check if Notion MCP responds (get-self)
- If 401: auto-refresh from Keychain, update `.mcp.json`, retry
- If Keychain token also expired: notify Pafi on Telegram, skip Notion pull
- **Review schedule**: Monthly (1st of each month) — verify token, check Notion API changelog for breaking changes

---

## 10. Changelog

| Version | Date | Changes |
|---|---|---|
| 1.0 | 2026-03-01 | Initial design based on research. Pending FORGE audit + Pafi approval. |
| 1.1 | 2026-03-01 | FORGE fixes: F1 (error handling §5), F2 (conflict resolution §4.3), F3 (removed notion-lib.sh, MCP inline), F4 (token monitoring §9), F7 (negative test scenarios §6 Step 8), F8 (review schedule §9). |
| 1.2 | 2026-03-01 | Pafi feedback: Added TRAINING hub (5 sub-pages: Finance, Legal, Marketing, Sales, Accounting). Removed Automation (only AI Automations stays). Crypto/Trading moved to PERSONAL. Added SKILLS (Procedures + Skills) and ECHELON Radar under INFRASTRUCTURE. Total: 17 topic pages. |
| 1.3 | 2026-03-01 | **BREAKING**: Template actualizat — Research DB devine al 3-lea DB obligatoriu pe ORICE pagină (alături de Procedures + Skills). Fix: template anterior definea "Research Reports" ca secțiune inline, nu ca child_database. Health page: Research DB creat + entry Retatrutide+5AMQ. Task pending: creare Research DB pe restul de ~30 pagini. |
