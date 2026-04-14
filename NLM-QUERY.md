---
type: procedure
created: 2026-03-17
status: DEPRECATED
deprecated_at: 2026-03-28
deprecated_reason: "Absorbed into Health Pro plugin cross-reference-nlm skill (~/.claude/plugins/health/skills/cross-reference-nlm/)"
successor: "~/.claude/plugins/health/skills/cross-reference-nlm/SKILL.md"
slug: nlm-query
tags: [procedure, nexus, DEPRECATED]
---

# [DEPRECATED] NLM-QUERY v2.0 — NotebookLM Universal Access Procedure

> **DEPRECATED 2026-03-28**: This procedure has been absorbed into the Health Pro plugin `cross-reference-nlm` skill. Use `/health` command for NLM queries. This file is kept for reference only.

**Status**: DEPRECATED
**Created**: 2026-03-17 | **Updated**: 2026-03-17
**Version**: 2.0
**Rule**: TOOL-INVENTORY-FIRST (check available interfaces before improvising)
**Scope**: Any NexusOS agent (DELPHI, GENIE, Lis, FORGE, bash scripts) that needs to query Google NotebookLM. This procedure is the single source of truth for NLM integration.
**Owner**: GENIE (procedure) / FORGE (code changes)

---

## What This Procedure Covers

NLM-QUERY defines how any NexusOS component queries Google NotebookLM notebooks. It abstracts away the MCP protocol, authentication, and Chrome/Playwright complexity behind a simple HTTP API.

### When to Use NLM vs Cortex

| Dimension | NotebookLM (NLM) | Cortex |
|-----------|-------------------|--------|
| **Purpose** | Deep synthesis on a curated corpus | Fast semantic search on all ingested content |
| **Data source** | Google NotebookLM notebooks (PDFs, docs, URLs added manually) | Qdrant vector DB (auto-ingested from procedures, rules, decisions) |
| **Query style** | Natural language questions expecting synthesized answers | Keyword/semantic similarity search returning ranked chunks |
| **Latency** | 10-90s (Playwright + Chrome + Google API) | <1s (local Qdrant) |
| **Best for** | Health/longevity research (Dr. Trevor Bachmeister corpus), deep "what does the literature say" questions | "Find the procedure for X", "What rule covers Y", operational lookups |
| **Availability** | Requires Chrome auth session, can expire | Always available if VPS reachable |

**Rule of thumb**: If you need a synthesized answer from a specific knowledge corpus, use NLM. If you need to find a specific document/rule/procedure, use Cortex.

---

## 1. Architecture

```
+---------------------------+
|  Agent                    |
|  (Python / TS / bash)     |
+---------------------------+
            |
            | HTTP POST/GET
            v
+---------------------------+
|  nlm-service.py           |  Port 18790 (127.0.0.1 ONLY)
|  LaunchAgent:             |  com.nexus.nlm-service
|  ThreadingHTTPServer      |
+---------------------------+
            |
            | JSON-RPC over stdio
            v
+---------------------------+
|  notebooklm-mcp           |  /opt/homebrew/bin/notebooklm-mcp
|  (Playwright + Chrome)    |  Manages browser session
+---------------------------+
            |
            | HTTPS (browser automation)
            v
+---------------------------+
|  notebooklm.google.com    |  Google's NotebookLM web app
|  Auth: Google account     |  Sources managed manually
+---------------------------+
```

### Key Components

| Component | Path | Language | Role |
|-----------|------|----------|------|
| HTTP Service | `~/.nexus/services/nlm-service.py` | Python | Persistent daemon, MCP lifecycle, HTTP API |
| Python Client | `~/.nexus/lib/nlm_client.py` | Python | Zero-dependency thin wrapper (urllib only) |
| TS Client | `~/repos/godagoo/claude-telegram-relay/src/nlm-client.ts` | TypeScript | Thin wrapper with AbortSignal support |
| TS Handler | `~/repos/godagoo/claude-telegram-relay/src/notebooklm-handler.ts` | TypeScript | Caching, rate limiting, state persistence |
| LaunchAgent | `~/Library/LaunchAgents/com.nexus.nlm-service.plist` | XML | Keeps nlm-service.py alive |
| State File | `~/.nexus/state/notebooklm-state.json` | JSON | Persists notebook URL across restarts |

### Master Notebook

- **Subject**: Dr. Trevor Bachmeister — health, longevity, peptides, protocols
- **URL**: `https://notebooklm.google.com/notebook/cb9cf1fd-850d-4369-9434-607fa171636b`
- **Sources**: 286 (PDFs, docs, URLs — managed manually at notebooklm.google.com)
- **Env var**: `NLM_DEFAULT_NOTEBOOK_URL` (set in `.env` and LaunchAgent plist)

---

## 2. API Reference — What Works vs What Does Not

| Endpoint | Method | Status | Request Body | Response | Notes |
|----------|--------|--------|--------------|----------|-------|
| `/health` | GET | WORKS | — | `{"status":"ok\|mcp_down","mcp_pid":N,"uptime_s":N}` | Use for preflight checks |
| `/default-notebook` | GET | WORKS | — | `{"notebook_id":"<url>","title":"..."}` | Returns env var or state file |
| `/query` | POST | WORKS | `{"notebook_url":"...","query":"..."}` | `{"answer":"..."}` | Main use case. Also accepts `notebook_id` key |
| `/list` | POST | WORKS | `{}` | `{"notebooks":[{"id":"...","title":"..."}]}` | Lists all accessible notebooks |
| `/create` | POST | 501 | `{"title":"..."}` | Error | notebooklm-mcp does not support this |
| `/add-url` | POST | 501 | `{"notebook_id":"...","url":"..."}` | Error | Add sources manually at notebooklm.google.com |
| `/add-text` | POST | 501 | `{"notebook_id":"...","title":"...","content":"..."}` | Error | Add sources manually at notebooklm.google.com |
| `/delete` | POST | 501 | `{"notebook_id":"..."}` | Error | notebooklm-mcp does not support this |

**Critical consequence**: Any pipeline that calls `/create`, `/add-url`, `/add-text`, or `/delete` will fail with HTTP 501. Specifically, `nexus-nlm-bridge.py` (DELPHI research synthesis) is **BROKEN** because it depends on `create` + `add_text`. Workaround: use the existing master notebook for queries; add sources manually.

---

## 3. Usage by Agent Type

### 3a. Python (DELPHI, research scripts, bridges)

**Client**: `~/.nexus/lib/nlm_client.py` — uses only `urllib` (zero external dependencies).

```python
import sys, os
sys.path.insert(0, os.path.expanduser("~/.nexus/lib"))
from nlm_client import nlm_query, nlm_health, nlm_list

DEFAULT_NOTEBOOK = os.environ.get(
    "NLM_DEFAULT_NOTEBOOK_URL",
    "https://notebooklm.google.com/notebook/cb9cf1fd-850d-4369-9434-607fa171636b"
)

# Step 1: Preflight health check
h = nlm_health()
if h.get("status") != "ok":
    print(f"NLM unavailable: {h}")
    # Graceful degradation — skip NLM, continue without it
    # Do NOT block the pipeline on NLM availability

# Step 2: Query
answer = nlm_query(DEFAULT_NOTEBOOK, "What is the recommended BPC-157 protocol?", timeout=90)
if answer:
    print(answer)
else:
    print("No answer received — check auth or try again")

# Step 3 (optional): List notebooks
notebooks = nlm_list(timeout=30)
for nb in notebooks:
    print(f"  {nb['title']}: {nb['id']}")
```

**Available functions** in `nlm_client.py`:

| Function | Signature | Returns | Status |
|----------|-----------|---------|--------|
| `nlm_health()` | `() -> dict` | `{"status": "ok\|unreachable"}` | WORKS |
| `nlm_query(notebook_id, query, timeout=120)` | `(str, str, int) -> Optional[str]` | Answer text or `None` | WORKS |
| `nlm_list(timeout=30)` | `(int) -> list` | `[{"id":..., "title":...}]` | WORKS |
| `nlm_create(title, timeout=30)` | `(str, int) -> Optional[str]` | — | BROKEN (501) |
| `nlm_delete(notebook_id, timeout=30)` | `(str, int) -> bool` | — | BROKEN (501) |
| `nlm_add_url(notebook_id, url, timeout=30)` | `(str, str, int) -> bool` | — | BROKEN (501) |
| `nlm_add_text(notebook_id, title, content, timeout=30)` | `(str, str, str, int) -> bool` | — | BROKEN (501) |
| `nlm_default_notebook(timeout=60)` | `(int) -> Optional[str]` | notebook URL | WORKS |

**Error handling**: All functions raise `RuntimeError` on failure (except `nlm_health` which returns `{"status": "unreachable"}`). Wrap in try/except for graceful degradation.

### 3b. TypeScript (Lis relay, Telegram bot)

**For auto-query RAG** (recommended — cached + rate-limited):

```typescript
import { queryNotebookCached } from "./notebooklm-handler";

// queryNotebookCached handles: cache lookup, rate limiting, AbortSignal, HTML stripping
const result = await queryNotebookCached(userMessage, signal);
if (result) {
  // Inject as context into Claude prompt
  // Result is plain text (HTML tags already stripped)
}
```

**For direct query** (returns formatted HTML for Telegram):

```typescript
import { queryNotebook, listNotebooks } from "./notebooklm-handler";

const html = await queryNotebook("What is the recommended BPC-157 protocol?");
// Returns: "🔍 <b>Rezultat:</b>\n\n..." or "❌ Nu am primit raspuns."

const listHtml = await listNotebooks();
// Returns: "📓 <b>Notebookurile tale (N)</b>\n\n..."
```

**For low-level access** (no caching, no rate limiting):

```typescript
import { nlmQuery, nlmHealth, nlmList } from "./nlm-client";

const health = await nlmHealth();          // { status: "ok" | "unreachable" }
const answer = await nlmQuery(nbUrl, question, 90_000, signal);  // string | null
const notebooks = await nlmList(30_000);   // Array<{ id, title }>
```

### 3c. Bash (scripts, cron jobs, one-off queries)

```bash
# Health check
curl -s http://127.0.0.1:18790/health | python3 -m json.tool

# Query
DEFAULT_NB="https://notebooklm.google.com/notebook/cb9cf1fd-850d-4369-9434-607fa171636b"

curl -s -X POST http://127.0.0.1:18790/query \
  -H "Content-Type: application/json" \
  -d "{\"notebook_url\": \"$DEFAULT_NB\", \"query\": \"What is TB-500?\"}" | \
  python3 -c "import json,sys; d=json.load(sys.stdin); print(d.get('answer','No answer'))"

# List notebooks
curl -s -X POST http://127.0.0.1:18790/list \
  -H "Content-Type: application/json" \
  -d '{}' | python3 -m json.tool

# Default notebook URL
curl -s http://127.0.0.1:18790/default-notebook | python3 -m json.tool
```

**Bash with timeout** (for cron jobs that must not hang):

```bash
curl -s --max-time 95 -X POST http://127.0.0.1:18790/query \
  -H "Content-Type: application/json" \
  -d "{\"notebook_url\": \"$DEFAULT_NB\", \"query\": \"$QUERY\"}" 2>/dev/null \
  || echo '{"answer": null, "error": "timeout"}'
```

---

## 4. Timeouts

| Layer | Operation | Timeout | Notes |
|-------|-----------|---------|-------|
| **nlm-service.py** | MCP initialize | 45s | First start or after crash |
| **nlm-service.py** | `/query` MCP call | 120s (default) | Overridable via `timeout` field in request body |
| **nlm-service.py** | `/list` MCP call | 30s | |
| **nlm_client.py** | `nlm_health()` | 5s | |
| **nlm_client.py** | `nlm_query()` | 120s (default) | Pass `timeout=90` for typical queries |
| **nlm_client.py** | `nlm_list()` | 30s | |
| **nlm-client.ts** | `nlmHealth()` | 5s | |
| **nlm-client.ts** | `nlmQuery()` | 120s (default) | Supports AbortSignal for external cancellation |
| **notebooklm-handler.ts** | `queryNotebookRaw()` | 90s | Internal timeout + external AbortSignal |
| **notebooklm-handler.ts** | `queryNotebook()` | 60s | Direct query (no cache) |
| **bash (curl)** | `--max-time` | 95s recommended | Set slightly above service timeout |

**First query after service start**: Expect 30-60s as Playwright launches Chrome and establishes session. Subsequent queries: 10-30s (Chrome session reused).

---

## 5. Rate Limits and Caching

### Rate Limits (TypeScript handler only)

| Parameter | Value | Scope |
|-----------|-------|-------|
| Max queries per window | 10 | Per 5-minute sliding window |
| Window duration | 5 minutes | Timestamps evicted on expiry |
| Behavior when limited | Returns `null` silently | Log: `[NB_AUTO] Rate limit hit` |

**Python and bash have no built-in rate limiting.** Be responsible — Google may throttle or block excessive automated access.

### Cache (TypeScript handler only)

| Parameter | Value |
|-----------|-------|
| Success TTL | 10 minutes |
| Failure TTL | 2 minutes (prevents retry storms) |
| Max entries | 200 (LRU eviction of oldest) |
| Key normalization | `trim().toLowerCase().replace(/\s+/g, " ")` |
| Scope | In-memory only, lost on relay restart |

---

## 6. Error Handling and Recovery

### Error Matrix

| Symptom | Health Status | Root Cause | Recovery |
|---------|--------------|------------|----------|
| All requests fail | `unreachable` | nlm-service.py not running | `launchctl kickstart -k user/$(id -u)/com.nexus.nlm-service` |
| Health OK, queries fail | `ok` | Chrome session expired | Open browser, log into notebooklm.google.com manually |
| HTTP 401 from `/query` | `ok` | Auth token expired | Run `notebooklm-mcp setup_auth` then restart service |
| HTTP 503 from `/query` | `mcp_down` | MCP process crashed | Service auto-restarts MCP; if persistent: restart service |
| HTTP 501 from any write | `ok` | Feature not implemented | Cannot fix — add sources manually at notebooklm.google.com |
| HTTP 400 from `/query` | `ok` | Missing `notebook_url` or `query` | Check request body format |
| `answer: null` or empty | `ok` | Query too vague or notebook has no relevant sources | Refine query; verify notebook has relevant sources |
| Timeout (no response) | varies | Chrome/Playwright hanging | Restart service: `launchctl kickstart -k user/$(id -u)/com.nexus.nlm-service` |

### Recovery Commands

```bash
# Check service status
launchctl list | grep nlm-service

# View logs
tail -50 /tmp/nlm-service.log 2>/dev/null || log show --predicate 'senderImagePath CONTAINS "nlm-service"' --last 10m

# Restart service (graceful)
launchctl kickstart -k user/$(id -u)/com.nexus.nlm-service

# Check MCP binary exists
ls -la /opt/homebrew/bin/notebooklm-mcp

# Re-authenticate (if auth expired)
/opt/homebrew/bin/notebooklm-mcp setup_auth

# Verify port is listening
lsof -i :18790
```

### Graceful Degradation Pattern

All agents MUST implement graceful degradation. NLM is a supplementary knowledge source, never a hard dependency.

```python
# Python pattern
try:
    h = nlm_health()
    if h.get("status") == "ok":
        answer = nlm_query(NOTEBOOK, question, timeout=90)
        if answer:
            context += f"\n[NotebookLM]: {answer}"
except Exception as e:
    logging.warning(f"NLM unavailable, continuing without: {e}")
    # Pipeline continues — NLM is optional enrichment
```

```typescript
// TypeScript pattern
const nlmContext = await queryNotebookCached(userMessage, signal).catch(() => null);
// nlmContext is null if anything fails — no exception propagation
```

---

## 7. Known Bugs

| # | Component | Bug | Impact | Workaround |
|---|-----------|-----|--------|------------|
| 1 | `nlm-client.ts` | Reads `NLM_SERVICE_URL` env var but `.env` defines `NLM_BRIDGE_URL` | Env var ignored; hardcoded fallback `127.0.0.1:18790` works | Rename env var to `NLM_SERVICE_URL` in `.env`, or leave as-is (fallback works) |
| 2 | `nlm_client.py` | Docstring references "Pafi Intel Feed" | Misleading — actual master notebook is Dr. Trevor Bachmeister health/longevity | Cosmetic; update docstring when touching the file |
| 3 | `nexus-nlm-bridge.py` | Calls `create` + `add_text` which return 501 | DELPHI research synthesis pipeline is BROKEN | Refactor to use existing master notebook for queries; add sources manually |
| 4 | `nlm_client.py` | `nlm_query` parameter named `notebook_id` but service expects `notebook_url` | Works because service accepts both keys (`notebook_url` or `notebook_id`) | No action needed; service is backwards-compatible |

---

## 8. Security

- **Port 18790 is bound to 127.0.0.1 ONLY** — never expose externally. This is enforced in `nlm-service.py`.
- **No authentication on the HTTP API** — any local process can query. This is acceptable because it is localhost-only.
- **Google auth session** is managed by Playwright/Chrome profile — credentials are not stored in NexusOS config files.
- **Query sanitization**: The TypeScript handler runs `sanitizeForPrompt()` on queries before sending to NLM (prevents prompt injection into the notebook query).

---

## 9. File Reference

| File | Path | Purpose |
|------|------|---------|
| HTTP Service | `~/.nexus/services/nlm-service.py` | Persistent daemon, MCP lifecycle management |
| Python Client | `~/.nexus/lib/nlm_client.py` | Zero-dep Python client library |
| TS Client | `~/repos/godagoo/claude-telegram-relay/src/nlm-client.ts` | TypeScript thin wrapper |
| TS Handler | `~/repos/godagoo/claude-telegram-relay/src/notebooklm-handler.ts` | Caching, rate limiting, state, Telegram formatting |
| LaunchAgent | `~/Library/LaunchAgents/com.nexus.nlm-service.plist` | Keeps service alive |
| State File | `~/.nexus/state/notebooklm-state.json` | Persisted notebook URL |
| DELPHI Bridge | `~/.nexus/bridges/nexus-nlm-bridge.py` | Research synthesis (BROKEN — uses unsupported write ops) |
| Env Config | `~/repos/godagoo/claude-telegram-relay/.env` | `NLM_DEFAULT_NOTEBOOK_URL`, `NLM_BRIDGE_URL` |

---

## 10. Decision Log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-03-17 | Single HTTP service for all agents | Eliminates auth fragmentation — one Chrome session shared by all callers |
| 2026-03-17 | Port 18790 localhost-only | Security — NLM has no auth layer on HTTP API |
| 2026-03-17 | Write operations return 501 | notebooklm-mcp upstream limitation — no programmatic source management |
| 2026-03-17 | Rate limit only in TS handler | Python/bash callers are internal scripts — trust the developer |
| 2026-03-17 | Graceful degradation mandatory | NLM depends on Chrome + Google auth — too fragile to be a hard dependency |
