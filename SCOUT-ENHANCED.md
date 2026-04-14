---
type: procedure
created: 2026-03-18
status: active
slug: scout-enhanced
tags: [procedure, nexus]
---

# SCOUT-ENHANCED — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-15
**Versiune**: 1.0
**Regulă asociată**: ECHELON-S-001
**Scope**: Pipeline proactiv de intelligence per proiect cu acțiuni interactive via Telegram inline keyboard.

---

## 1. Problema

ECHELON Scout în forma inițială generează 10 query-uri bazate pe contextul sesiunii recente, rezultând în query-uri tematice identice (ex: 10 variații pe "AI agent automation") fără acoperire sistematică a proiectelor active. Alertele ajung pe claudeautomationbot (VPS) ca plain text fără posibilitate de acțiune. Loop-ul de intelligence este rupt: semnalele MEDIUM/HIGH rămân izolate în Cortex `scout-intel` fără a ajunge în Intel Hub sau a declanșa research aprofundat.

**Fără această procedură:**
- SMSads, GamblingSEO, Albastru etc. nu primesc niciodată semnale de piață dacă nu sunt în sesiunea recentă
- Alertele Telegram necesită acțiune manuală complexă pentru a fi valorificate
- Frecvența de 1×/zi la 07:00 ratează mișcări rapide de piață

---

## 2. Procedura

### Pas 1: Project Registry (`scout-projects.json`)

Fișierul `~/.openclaw/scripts/scout/scout-projects.json` definește proiectele active cu:
- `slug`, `name`, `status`, `card_path` (calea spre PROJECT-CARD.md)
- `query_slots` (număr query-uri per run — suma totală = 10)
- `focus` (array de keyword combinations fallback)
- `enabled` (true/false)

Proiecte active (8): SMSads (2 slots), SEO Group (2), Albastru (1), AI-B2B Agency (1), Saraimanic (1), Health (1), Crypto (1), NexusOS (1).

Când `enabled: false`, slot-urile libere se adaugă la proiectul cu cele mai multe slot-uri din lista activă.

### Pas 2: Query Generation per proiect

La fiecare run, pentru fiecare proiect enabled:
1. Citește `PROJECT-CARD.md` (primii 2000 chars) dacă există
2. Combină excerpt-ul cu `focus` keywords din registry
3. Ollama (qwen2.5:7b) generează `query_slots` query-uri specifice, acționabile, actuale (2026)
4. Fiecare query e tagged cu `project_slug`

Fallback: dacă PROJECT-CARD.md lipsește, folosește doar `focus` array.

### Pas 3: Search + Evaluare

Pentru fiecare query (10 total):
- Perplexity (`sonar`) + Brave Search în paralel (dacă ambele keys disponibile)
- Ollama evaluează fiecare candidat: `is_actionable()` → score LOW/MEDIUM/HIGH + reason
- LOW: ignorat complet
- MEDIUM/HIGH: stocat în Cortex `scout-intel` + fișier `pending/{uid}.json` + Telegram alert

### Pas 4: Telegram Alert (claudemacm4_bot)

Alertele merg pe `@claudemacm4_bot` (Lis/MacM4), Keychain: `telegram-bot-token-claudemacm4`.

Format mesaj (HTML parse_mode cu html.escape() pe conținut dinamic):
```
🔍 [Scout][MEDIUM] {query}
📁 Project: {project}
📰 {title[:120]}
🔗 {url}
💡 {reason[:300]}
```

Inline keyboard atașat:
```
[🔬 Research]  [➕ Add to Hub]  [❌ Dismiss]
```

Callback data: `{action}:{uid}` (ex: `research:abc123`).

### Pas 5: Callback Handler Daemon

`scout-callback-handler.py` rulează continuu pe MacM4 (KeepAlive LaunchAgent).

Long-polling Telegram getUpdates (timeout=30s, interval=3s). La fiecare callback_query:

1. `telegram_answer_callback()` — elimină spinner-ul
2. `telegram_edit_reply_markup()` — **elimină imediat keyboard-ul** (previne acțiuni duplicate)
3. Încarcă `pending/{uid}.json` — dacă lipsește: "⚠️ Finding expired"
4. Execută acțiunea:

**Dismiss:**
- Adaugă UID în `state.json` dismissed set
- Șterge `pending/{uid}.json`
- Editează mesajul: append "❌ Dismissed"

**Add to Hub:**
- Creează pagină în Intel Hub Notion DB (`327d31d1-1b7d-819a-972a-c10f82224289`)
- Properties: Name, Source=ECHELON, Priority={score}, Status=NEW, URL, Summary, User=Pafi
- Notion token din Keychain: `NOTION_API_KEY`
- Editează mesajul: append "➕ Added to Intel Hub"
- Șterge `pending/{uid}.json`

**Research:**
- Editează mesajul imediat: "🔬 Research in progress..."
- Dispatch `nexus-unified.sh --input {url|query} --depth deep` (Popen, non-blocking)
- Apelează Add to Hub (automat)
- Editează mesajul: "🔬 Research dispatched + ➕ Added to Intel Hub"

### Pas 6: Schedule (3 ore)

LaunchAgent `com.echelon.scout` cu 8 StartCalendarInterval: 00:00, 03:00, 06:00, 09:00, 12:00, 15:00, 18:00, 21:00.

### Pas 7: Pending TTL Cleanup

La fiecare `store_pending()`, șterge automat fișierele `pending/*.json` mai vechi de 7 zile (previne acumulare).

### Test Cases

1. **Normal flow**: Scout rulează la 09:00 → generează 2 query SMSads + 2 SEO + 1 Albastru + etc. → găsește 1 HIGH pentru SEO → stocat Cortex + pending/{uid}.json + Telegram cu keyboard → Pafi apasă Research → keyboard dispare imediat → nexus-unified.sh dispatched + Notion page creat → mesaj updatat
2. **Edge case — proiect disabled**: `crypto.enabled: false` → 1 slot redistribuit la SMSads (3 slots) → run continuă normal cu 10 query-uri
3. **Failure case — pending expirat**: Pafi apasă buton după 7+ zile → "⚠️ Finding expired or already processed"
4. **Failure case — double tap**: Pafi apasă Research de două ori rapid → primul tap elimină keyboard imediat → al doilea tap nu mai are buton activ, callback ignorat sau "Finding expired" după ce pending e șters

---

## 3. Cortex Logging

La fiecare run Scout, stocat în `scout-intel`:
```json
{
  "type": "scout_run",
  "procedure": "SCOUT-ENHANCED",
  "rule_id": "ECHELON-S-001",
  "projects_covered": ["smsads", "seo-group", ...],
  "stored": N,
  "alerts_sent": M,
  "has_enforcement_loop": true,
  "forge_version": "1.4"
}
```

La fiecare acțiune callback (Add/Research/Dismiss), stocat în `scout-intel`:
```json
{
  "type": "scout_action",
  "action": "add|research|dismiss",
  "uid": "{uid}",
  "project": "{slug}",
  "url": "{url}",
  "acted_at": "{iso}"
}
```

---

## 4. Enforcement Loop

### WHERE
La fiecare run Scout (8×/zi) + la fiecare apăsare buton Telegram.

### WHEN
- LaunchAgent `com.echelon.scout` la 0/3/6/9/12/15/18/21h
- LaunchAgent `com.echelon.scout-callback` KeepAlive continuu

### HOW (violation detection)
- `~/.openclaw/logs/scout.log` — verifică `[Scout] Done. Stored=0, Alerts=0` pe mai mult de 3 run-uri consecutive → semnal că API keys expirate sau Ollama down
- `~/.openclaw/logs/scout-callback.log` — verifică crash loops (ThrottleInterval=10s în plist previne spam restart)
- `pending/` dir size > 500 fișiere → TTL cleanup defect

### CONNECT
- Regulă: ECHELON-S-001 (governance ECHELON)
- Intel Hub: Notion DB `327d31d1-1b7d-819a-972a-c10f82224289`
- Cortex: `scout-intel` collection
- NexusOS: `nexus-unified.sh --depth deep` pentru Research dispatch

### VERIFY
- [ ] Procedura executată complet?
- [ ] Output satisface §1 (toate proiectele active acoperite per run)?
- [ ] Alertele ajung pe claudemacm4_bot cu keyboard?
- [ ] Butoanele funcționează (keyboard dispare imediat la tap)?
- [ ] VK emis?

### MODEL ROUTING
- Query generation: Ollama local (qwen2.5:7b) — zero cost
- Actionability eval: Ollama local — zero cost
- Research dispatch: nexus-unified.sh → IRIS (Perplexity deep)
- Add to Hub: Notion API direct — zero cost
