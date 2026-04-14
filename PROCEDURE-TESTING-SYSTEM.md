---
type: procedure
created: 2026-03-17
status: active
slug: procedure-testing-system
tags: [procedure, nexus]
---

# PROCEDURE-TESTING-SYSTEM — Testare Automată 3-Tier a Tuturor Procedurilor

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: META-H-001
**Scope**: Testare automată multi-tier a tuturor procedurilor din sistem — health check zilnic, E2E lite săptămânal, E2E full bi-săptămânal

---

## 1. Problema

Sistemul are 30+ proceduri (14 canale ECHELON, 10 Delphi tools, 6+ proceduri operaționale) care trebuie verificate periodic. Fără testare automată:

- Proceduri pot deveni stale fără să se știe (API keys expirate, endpoints schimbate)
- Scripturi pot avea erori de sintaxă introduse la ultimul edit
- LaunchAgent-uri pot fi unloaded fără notificare
- Enforcement loops lipsesc sau sunt incomplete
- Cortex entries orfane sau lipsă
- API-uri plătite pot fi consumate inutil la teste prea frecvente

Fără procedură:
- Testarea manuală e uitată sau inconsistentă
- Problemele se descoperă doar când ceva se strică în producție
- Cost overrun pe API-uri plătite (Apify $5/1K, ScrapeCreators $0.002/req)
- Nu e clar ce se testează, ce nu, și cât de des

Situații acoperite:
- **Tier 1 (Daily)**: Health check — fișiere, syntaxă, LaunchAgents, staleness, Cortex
- **Tier 2 (Weekly)**: E2E lite — API-uri gratuite (Bluesky AT Protocol, GitHub REST, RSS, PodcastIndex, Coursera HTTP)
- **Tier 3 (Bi-weekly)**: E2E full — API-uri plătite (ScrapeCreators, Apify) + toate cele gratuite
- **On-demand**: Test individual per canal/procedură

---

## 2. Procedura

```
TIER SELECTION → CHECKS per tier → REPORT (JSON + MD) → TELEGRAM alert (if failures)
```

### Tier 1 — Daily Health Check (02:00, $0)
| Check | Ce verifică | Tool |
|-------|------------|------|
| File existence | Proceduri + scripturi prezente | fs.existsSync |
| Syntax check | .js → `node --check`, .py → `python3 -m py_compile`, .sh → `bash -n` | spawnSync |
| LaunchAgent | Label loaded | launchctl list |
| Log health | No [fatal]/traceback/exception în ultimele 150 linii | tail + grep |
| Enforcement loop | WHERE + WHEN + HOW + CONNECT în procedură | regex |
| Cortex presence | Procedura există în Cortex cu rule_id | API search |
| Staleness | last_review + review_cycle_days < today | procedure-health.json |

### Tier 2 — Weekly E2E Lite (Luni 03:00, $0)
Toate check-urile din Tier 1 PLUS:
| Canal | API Test | Expected |
|-------|----------|----------|
| Bluesky | AT Protocol `getAuthorFeed` (karpathy.bsky.social) | ≥1 post |
| GitHub | REST API `search/repositories?q=AI` | ≥1 repo |
| GitHub Atom | `anthropics/claude-code/releases.atom` | valid XML |
| Newsletter | RSS feedparser (techcrunch.com/feed) | ≥1 entry |
| Podcast | PodcastIndex API HMAC search | ≥1 feed |
| Coursera | HTTP HEAD coursera.org | 200 |
| Reddit | json.reddit.com/r/MachineLearning | ≥1 post |
| X | Verificare doar script syntax (API restricted) | syntax OK |
| YouTube | Verificare doar script syntax (API restricted) | syntax OK |
| Scribd | HTTP HEAD scribd.com | 200 |

### Tier 3 — Bi-weekly E2E Full (1st + 15th, 04:00, ~$0.05)
Toate check-urile din Tier 2 PLUS:
| Canal | API Test | Cost | Expected |
|-------|----------|------|----------|
| LinkedIn | ScrapeCreators profile (1 request) | $0.002 | profile data |
| Instagram | ScrapeCreators profile (1 request) | $0.002 | profile data |
| Threads | ScrapeCreators profile (1 request) | $0.002 | profile data |
| TikTok | Apify actor accessibility check (1 request) | ~$0.005 | actor accessible |

### Flag/invocation
```bash
# Tier 1 (daily)
node test-all-procedures.js --tier daily

# Tier 2 (weekly)
node test-all-procedures.js --tier weekly

# Tier 3 (bi-weekly)
node test-all-procedures.js --tier full

# Single channel
node test-all-procedures.js --channel bluesky
```

---

## 3. Cortex Logging

Per test run:
```json
{
  "text": "[PROCEDURE-TEST] Tier {tier} — {date}: {pass}/{total} PASS, {fail} FAIL. Failures: [{failed_names}]",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_test_result",
    "source": "test-all-procedures",
    "tier": "daily|weekly|full",
    "total": N,
    "pass": N,
    "fail": N,
    "failed": ["name1", "name2"],
    "e2e_results": {"bluesky": "PASS", "github": "PASS"},
    "cost_usd": 0.00,
    "procedure": "PROCEDURE-TESTING-SYSTEM",
    "rule_id": "META-H-001",
    "tags": ["procedure-test", "tier-{N}", "monitoring"]
  }
}
```

Per scan staleness: `"Procedure health scan {date}: {N} total, {M} action needed, {K} degraded"`

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- LaunchAgent `com.genie.procedure-test` (daily 02:00)
- LaunchAgent `com.genie.procedure-test-weekly` (Monday 03:00)
- LaunchAgent `com.genie.procedure-test-full` (1st+15th 04:00)
- LaunchAgent `com.genie.procedure-health` (Monday 08:00 — staleness scan)
- Manual invocation via CLI flags

### WHEN
- Tier 1: Every day at 02:00
- Tier 2: Every Monday at 03:00
- Tier 3: 1st and 15th of each month at 04:00
- Staleness: Every Monday at 08:00 (existing)

### HOW (violation detection)
- Daily test skip >48h = violation (LaunchAgent not loaded or machine off)
- Weekly test skip >10 days = violation
- Bi-weekly test skip >20 days = violation
- Test reports >3 consecutive FAIL on same channel = escalate to Telegram
- New procedure added but not in test registry within 24h = violation

### CONNECT
- META-H-001 (hard rule) — procedure monitoring mandate
- `test-all-procedures.js` — main test runner script
- `procedure-health-check.sh` — staleness scanner
- `procedure-health.json` — state file for all 30 procedures
- `telegram-notify.sh` — alert delivery
- ECHELON-S-001 — all ECHELON channels are test subjects

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| **test-all-procedures.js** | Test runner principal | `~/.nexus/monitoring/` |
| **procedure-health-check.sh** | Staleness scanner | `~/.openclaw/scripts/` |
| **procedure-health.json** | State file (30 procedures) | `~/.openclaw/` |
| **telegram-notify.sh** | Alert delivery | `~/.openclaw/scripts/` |
| **LaunchAgents** | Cron scheduling | `~/Library/LaunchAgents/com.genie.procedure-*` |
| **Cortex** | Test result storage | `localhost:6400` (VPS) |
| **Keychain** | API keys for Tier 3 | SCRAPECREATORS_KEY, APIFY_API_KEY, PODCASTINDEX_API_KEY/SECRET |
| **Node.js** | Runtime | `/opt/homebrew/bin/node` |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Procedures tracked | 30+ |
| Daily health check uptime | >95% |
| Weekly E2E lite uptime | >90% |
| Bi-weekly E2E full uptime | >85% |
| False positive rate | <5% |
| Tier 3 cost/month | <$0.20 |
| Time to detect broken channel | <24h (Tier 1), <7d (Tier 2) |

---

## Checklist Pre-Publicare (FORGE §7)

- [x] Regulă asociată există: META-H-001
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT
- [x] Procesul din WHERE documentat (4 LaunchAgents)
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] Scripturi existente funcționale (test-all-procedures.js 429L, procedure-health-check.sh 378L)
- [x] Cortex JSON structure definit (§3)
- [x] Dependențe listate cu paths (§5)
- [x] Metrics cu targets (§6)
- [x] Script actualizat cu 12 canale ECHELON noi (Codex m4-160)
- [x] LaunchAgents create pentru Tier 2 + Tier 3 (Codex m4-160)
- [x] Entry adăugat în procedure-health.json
- [x] Salvat în Cortex (id=8206455c)
- [x] Status: ACTIVE
