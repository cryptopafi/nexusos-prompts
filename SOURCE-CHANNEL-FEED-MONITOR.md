---
type: procedure
created: 2026-03-18
status: active
slug: source-channel-feed-monitor
tags: [procedure, nexus]
---

# SOURCE-CHANNEL-FEED-MONITOR — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-15
**Versiune**: 1.0
**Regulă asociată**: INGEST-H-001 (orice conținut ingerat TREBUIE să înregistreze sursa părinte)
**Scope**: Adaugă câmpul `source_channel` în toate cele 3 baze de date ale Intel Hub și monitorizează automat sursele cunoscute pentru conținut nou.
**Gemini Audit**: CONDITIONAL 2.71/4.0 (4 findings rezolvate în această versiune)
**Opus Audit**: CONDITIONAL 2.75/4.0 (1 CRITICAL + 1 HIGH rezolvate în v1.1 — YouTube RSS 404, yt-dlp adapter)
**Versiune**: 1.1

---

## 1. Problema

Conținutul este ingerat o singură dată în Intel Hub. După ingestare, sursa părinte (canal YouTube, repo GitHub, subreddit, domeniu de știri, comunitate Skool) este pierdută permanent. Nu există niciun mecanism automat pentru descoperirea conținutului nou de la surse deja cunoscute.

**Consecință**: Daily digest și raportul BI pot afișa doar ce a fost explicit ingerat manual. Videoclipuri noi, articole noi, postări noi de la surse monitorizate nu sunt niciodată descoperite autonom.

**Gap confirmat în 3 baze de date**:
1. **Cortex** (vector DB, `localhost:6400`) — metadata fără `source_channel`
2. **`monitoring-db.json`** (`~/.nexus/intel/monitoring-db.json`) — schema fără câmpul `source_channel`
3. **Notion Intel Hub** (DB `327d31d1-1b7d-819a-972a-c10f82224289`) — fără proprietatea `SourceChannel`

**`subscriptions.json`** (`~/.nexus/inbox/subscriptions.json`) există cu schema corectă dar este permanent goală — niciun script nu scrie sau citește din ea.

**`source-hook.sh`** și **`source-discover.py`** există dar nu sunt apelate din niciun pipeline de ingestare — cod mort.

---

## 2. Procedura

### Pas 1: Clasificarea tipului de sursă și câmpul `source_channel`

La ingestarea oricărui item, se derivă `source_channel` din URL-ul de conținut astfel:

| Tip conținut | URL conținut (exemplu) | `source_channel` (ce înregistrăm) | `feed_url` (pentru polling) |
|---|---|---|---|
| **YouTube video** | `youtube.com/watch?v=xxx` | `https://youtube.com/@ChannelName/videos` (din yt-dlp `uploader_url` + `/videos`) | N/A — YouTube RSS returnează 404 pe canale noi; polling via **yt-dlp** (vezi Pas 5b) |
| **YouTube Shorts** | `youtube.com/shorts/xxx` | `https://youtube.com/@ChannelName/videos` | același yt-dlp adapter ca canalul |
| **GitHub repo/release** | `github.com/user/repo/releases/...` | `https://github.com/user/repo` | `https://github.com/user/repo/releases.atom` |
| **GitHub commit/article** | `github.com/user/repo/commit/...` | `https://github.com/user/repo` | `https://github.com/user/repo/commits.atom` |
| **Reddit post** | `reddit.com/r/sub/comments/...` | `https://reddit.com/r/sub` | `https://reddit.com/r/sub/.rss?limit=25` |
| **Articol de știri** | `techcrunch.com/2026/03/15/...` | `https://techcrunch.com` (domeniu root) | autodetect: caută `<link rel="alternate" type="application/rss+xml">` în `<head>` |
| **Newsletter/Substack** | `substack.com/p/article` | `https://author.substack.com` | `https://author.substack.com/feed` |
| **HackerNews post** | `news.ycombinator.com/item?id=xxx` | `https://news.ycombinator.com` | `https://news.ycombinator.com/rss` |
| **Skool post/articol** | `skool.com/community/post/xxx` | `https://skool.com/community` (slug comunitate) | N/A — Skool nu are RSS public; monitorizare via ECHELON scraper dedicat |
| **Skool mesaj/DM** | Telegram forward din Skool | sursa Telegram (`source_channel: "skool:{community_slug}"`) | N/A |
| **Tweet / X post** | `x.com/user/status/xxx` | `https://x.com/user` | N/A — Twitter/X fără RSS public; monitorizare via ECHELON X scraper |
| **LinkedIn articol** | `linkedin.com/pulse/...` | `https://linkedin.com/in/user` sau `linkedin.com/company/name` | N/A |
| **Generic web** | orice alt URL | domeniu root (schema + netloc) | încearcă autodescoperire RSS; dacă nu există → `feed_url: ""` |

**Regula de derivare** (în ordine de prioritate):
1. yt-dlp JSON → `channel_url` + `channel_id` (YouTube)
2. Parsare URL → path components (GitHub, Reddit, Skool slug)
3. HTTP `<head>` → `<link rel="alternate" type="application/rss+xml">` (știri generice)
4. Fallback: domeniu root (schema + netloc)

---

### Pas 2: Modifică `ingest.py:item_to_schema()` — schema canonică

Adaugă în dict-ul returnat de `item_to_schema()` (linia ~278, `~/.nexus/intel/ingest.py`):

```python
source_channel = clamp(str(
    pick_first(item, [
        "source_channel", "channel_url", "parent_url",
        "uploader_url", "feed_url", "subreddit_url"
    ]) or ""
), 500)
channel_id = str(pick_first(item, ["channel_id", "uploader_id"]) or "").strip()

# returnează în dict:
"source_channel": source_channel,
"channel_id": channel_id,
```

După scrierea noilor items în `monitoring-db.json`, apelează `register_subscriptions(new_items)` (Pas 4).

---

### Pas 3: Modifică sursele upstream (ECHELON scrapers + ingest-25)

**ECHELON YouTube scraper** — adaugă la fiecare item din `echelon-youtube-signals.json`:
```json
{
  "channel_url": "<din yt-dlp channel_url>",
  "channel_id": "<din yt-dlp channel_id>",
  "source_channel": "<din yt-dlp channel_url>"
}
```
Câmpurile sunt disponibile gratis din `yt-dlp --dump-json` (fields: `channel_url`, `channel_id`, `uploader_url`).

**`ingest-25-targeted.py:store_cortex()`** — adaugă în metadata:
```python
"source_channel": channel_url,  # din yt-dlp
"channel_id": channel_id,
```

**`nexus-unified.sh:seed_cortex()`** — adaugă parametru `source_channel` derivat din `uploader_url` (disponibil deja la linia ~292 din JSON-ul yt-dlp parsat).

---

### Pas 4: Auto-populează `subscriptions.json` la ingestare

Funcție `register_subscriptions(new_items)` adăugată în `ingest.py`:

**Schema unui entry în `subscriptions.json`**:
```json
{
  "id": "youtube_channel:UCuAXFkgsw1L7xaCfnd5JJOw",
  "type": "youtube_channel",
  "name": "Fireship",
  "source_url": "https://www.youtube.com/@Fireship",
  "feed_url": "https://www.youtube.com/feeds/videos.xml?channel_id=UCuAXFkgsw1L7xaCfnd5JJOw",
  "last_checked": null,
  "last_item_url": "https://youtube.com/watch?v=xxx",
  "check_interval_hours": 24,
  "active": true,
  "added_at": "2026-03-15T07:00:00Z",
  "added_from": "ECHELON"
}
```

**Tipuri suportate și `check_interval_hours` defaults**:

| type | interval default | note |
|---|---|---|
| `youtube_channel` | 24h | **yt-dlp adapter** (nu RSS — RSS returnează 404 pe canale handle-based). `source_url` = `@handle/videos`. `feed_url` = `""` (yt-dlp folosit direct). |
| `github_repo` | 24h | Atom gratuit, fără auth |
| `reddit_subreddit` | 12h | RSS cu User-Agent header |
| `newsletter_substack` | 24h | RSS gratuit |
| `hackernews` | 6h | RSS gratuit |
| `skool_community` | N/A | `active: false`, `feed_url: ""` — monitorizare via ECHELON |
| `twitter_x` | N/A | `active: false`, `feed_url: ""` — monitorizare via ECHELON X scraper |
| `generic_web` | 24h | doar dacă `feed_url` autodetectat; altfel `active: false` |
| `news_site` | 12h | RSS autodetectat din `<head>` sau path standard `/feed`, `/rss`, `/rss.xml` |

**Regulă de upsert**: dacă `id` există deja → skip (nu suprascrie `last_checked` sau `active`). Utilizatorul poate edita manual `subscriptions.json` (adaugă/dezactivează surse).

**Scriere atomică** (obligatoriu — Gemini HIGH finding):
```python
SUBSCRIPTIONS_FILE.parent.mkdir(parents=True, exist_ok=True)  # mandatory
tmp = SUBSCRIPTIONS_FILE.with_suffix(".tmp")
tmp.write_text(json.dumps(state, indent=2, ensure_ascii=False), encoding="utf-8")
os.replace(tmp, SUBSCRIPTIONS_FILE)
```

---

### Pas 5: `feed-monitor.py` — polling automat

Fișier nou: `~/.nexus/scripts/feed-monitor.py`

**Pas 5b: YouTube-specific adapter** (CRITICAL — RSS nu funcționează pe canale handle-based):

```
dacă subscription.type == "youtube_channel":
  # yt-dlp --flat-playlist --playlist-items 1-{MAX_NEW} "{source_url}"
  # source_url = "@handle/videos" (ex: "https://youtube.com/@buildnpublic/videos")
  items = run_yt_dlp_flat(subscription["source_url"], max_items=MAX_NEW_PER_FEED)
  # fiecare item: {"url": "https://youtube.com/watch?v=xxx", "title": "...", "channel_id": "..."}
  # dedup cu last_item_url → break-on-known logic (identic cu RSS flow)
  # dispatch nexus-unified.sh pentru fiecare item nou
  # NU se apelează urlopen(feed_url) — feed_url este "" pentru youtube_channel
```

Cost: yt-dlp spawn per canal per run — lightweight (metadata only, no download). Rulează `yt-dlp --flat-playlist -J --playlist-items 1-3` (sub 5 secunde per canal).

---

**Logica principală RSS** (cu fix-ul CRITICAL de la Gemini — list comprehension corect):

```
pentru fiecare subscription activă cu feed_url nenul:
  dacă last_checked + check_interval_hours > now → skip

  fetch RSS/Atom feed (timeout=15s, User-Agent header)

  # CORECT: iterează și oprește la elementul cunoscut
  new_items = []
  pentru fiecare item din feed (newest first):
    dacă item.url == last_item_url: STOP
    adaugă la new_items

  new_items = new_items[:MAX_NEW_PER_FEED]  # cost guard: max 3/feed/run

  pentru fiecare new_item:
    dispatch non-blocking: Popen(["bash", nexus-unified.sh, "--input", url, "--depth", "standard"])
    log succesul

  actualizează last_checked = now (UTC ISO format cu "+00:00")
  dacă new_items: actualizează last_item_url = new_items[0].url

  salvează subscriptions.json atomic
```

**Error handling** (Gemini MEDIUM finding — fără silent except):
- Erori de fetch → log `[ERROR]` + continuă cu next subscription (non-blocking)
- Erori de parse XML → log `[WARN]` + marchează `last_checked` (nu retry imediat)
- Dispatch fail → log `[ERROR]` cu returncode; nu bloca feed-monitor
- Niciun `except Exception: pass` fără log anterior

**Constante**:
```python
MAX_NEW_PER_FEED = 3       # cost guard
FETCH_TIMEOUT = 15         # secunde, aplicat explicit la urlopen()
MAX_FEEDS_PER_RUN = 50     # evită run-uri prea lungi
```

**LaunchAgent**: `com.nexus.feed-monitor.plist`
- Rulează la: 06:00, 12:00, 18:00, 00:00 (StartCalendarInterval × 4)
- `CORTEX_LOCAL_URL=http://localhost:6400`
- Log: `~/.nexus/logs/feed-monitor.log`

---

### Pas 6: Modifică `notion-sync.py:build_page_properties()`

Adaugă `SourceChannel` URL property în Notion (după fix Gemini MEDIUM — validare URL):

```python
source_channel = (item.get("source_channel") or "").strip()
if source_channel:
    try:
        from urllib.parse import urlparse
        parsed = urlparse(source_channel)
        if parsed.scheme in ("http", "https") and parsed.netloc:
            props["SourceChannel"] = {"url": source_channel}
    except (ValueError, AttributeError) as exc:
        log(f"[WARN] source_channel URL invalid: {source_channel!r} — {exc}")
```

**Notion DB schema**: adaugă manual proprietatea `SourceChannel` de tip `URL` în Notion Intel Hub DB (1 click în interfața Notion). Nu necesită cod.

---

### Pas 7: Backfill și coexistență

- **Items istorice** în `monitoring-db.json`: câmpul `source_channel` va fi `""` — acceptabil, nu se migrează retroactiv
- **Subscriptions manuale**: se pot adăuga direct în `subscriptions.json` fără să fi ingerat vreun item din acea sursă
- **Surse fără RSS** (Skool, X, LinkedIn): rămân `active: false` în subscriptions; sunt monitorizate exclusiv prin ECHELON scrapers dedicați; `source_channel` se populează la ingestare dar fără polling automat

---

### Test Cases

1. **Normal flow YouTube**: ECHELON ingestează videoclip Fireship → `echelon-youtube-signals.json` conține `channel_url: "https://youtube.com/@Fireship"` → `ingest.py` scrie `source_channel` în `monitoring-db.json` → `subscriptions.json` primește entry `youtube_channel:UCuAXFkgsw1L7xaCfnd5JJOw` → `feed-monitor.py` la run următor descoperă videoclip nou → dispatch `nexus-unified.sh --input {new_url} --depth standard`

2. **Edge case — surse fără RSS** (Skool): item Skool ingerat → `source_channel: "skool:community-slug"` stocat în monitoring-db → entry în subscriptions cu `active: false`, `feed_url: ""` → `feed-monitor.py` skip → nu se face polling, doar tracking

3. **Edge case — re-run idempotent**: `feed-monitor.py` rulat de 2 ori consecutiv → la al doilea run, `last_checked` este recent (< interval) → toate sursele skip → zero dispatche duplicate

4. **Failure case — feed indisponibil**: URL RSS returnează 404 sau timeout → log `[ERROR]` → `last_checked` actualizat (evită retry imediat) → continuă cu next subscription

5. **Edge case — subscriptions.json lipsă**: `feed-monitor.py` rulat prima oară → `~/.nexus/inbox/` creat automat → `subscriptions.json` inițializat cu `{"subscriptions": []}` → exit normal

---

## 3. Cortex Logging

La fiecare run `feed-monitor.py`:
```json
{
  "text": "feed-monitor run: {N} feeds checked, {M} new items dispatched",
  "collection": "intelligence",
  "metadata": {
    "type": "feed_monitor_run",
    "procedure": "SOURCE-CHANNEL-FEED-MONITOR",
    "rule_id": "INGEST-H-001",
    "feeds_checked": N,
    "new_items_dispatched": M,
    "run_at": "ISO timestamp",
    "has_enforcement_loop": true,
    "forge_version": "1.4"
  }
}
```

La fiecare item nou descoperit și dispatched:
```json
{
  "text": "New content discovered from {source_channel}: {title}",
  "collection": "intelligence",
  "metadata": {
    "type": "feed_discovery",
    "source_channel": "{url}",
    "content_url": "{url}",
    "subscription_id": "{id}",
    "discovered_at": "ISO timestamp"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- **INGEST-H-001 gate** — `ingest.py:item_to_schema()` este punctul de intrare canonical. Orice item care trece prin această funcție trebuie să aibă `source_channel` derivat (chiar dacă e `""`)
- **ECHELON scraper gate** — înainte de scrierea în `echelon-{platform}-signals.json`, extrage `source_channel` din metadata platformei
- **Daily smoke test** — verifică că `subscriptions.json` nu este gol după prima zi de operare

### WHEN
- La fiecare ingestare de conținut nou (ECHELON, ingest manual, nexus-unified.sh)
- La fiecare run `feed-monitor.py` (06:00, 12:00, 18:00, 00:00)
- La adăugarea manuală a unui URL în subscriptions.json

### HOW (violation detection)
- `ingest.py` — dacă un item ajunge în `monitoring-db.json` fără câmpul `source_channel`: **violation INGEST-H-001**
- `subscriptions.json` — dacă după 48h de operare rămâne goală: **violation** (ingestarea nu a înregistrat sursele)
- `feed-monitor.log` — dacă nu conține niciun run în ultimele 12h: **LaunchAgent eșuat**
- `notion-sync.py` — dacă iteme noi din Notion nu au `SourceChannel` populat: **violation**

### CONNECT
- **INGEST-H-001** (regulă nouă) → enforcement la `item_to_schema()` + ECHELON scrapers
- **`procedure-health.json`** → entry pentru această procedură
- **`feed-monitor.log`** → monitorizat de daily smoke test
- **ECHELON HEARTBEAT** → verifică că `subscriptions.json` crește după fiecare run ECHELON

### VERIFY
- [ ] Procedura executată complet? `subscriptions.json` conține entries după primul run ECHELON
- [ ] Output satisface §1? Videoclipuri noi de la canale cunoscute apar în daily digest fără trigger manual
- [ ] VK emis? ✅ după fiecare modificare a schemei
- [ ] Gemini CRITICAL fix aplicat? (`new_items` list comprehension corect — break la `last_item_url`)
- [ ] Gemini HIGH fix aplicat? (`mkdir(parents=True, exist_ok=True)` înainte de scriere)
- [ ] Gemini MEDIUM fix aplicat? (no silent except, dispatch non-blocking via Popen)

### MODEL ROUTING
- `feed-monitor.py` — no LLM (RSS parsing + dispatch, zero cost)
- `ingest.py:register_subscriptions()` — no LLM (pure Python logic)
- `nexus-unified.sh` dispatch per item nou — Haiku (depth: standard, cost: minimal)
- Notion sync — no LLM

---

## Referințe

- Gemini Audit: `~/.nexus/workspace/output/forge/audit-gemini-20260315-075121.md` (CONDITIONAL 2.71/4.0, 4 findings)
- Proposal original: `/tmp/source-channel-proposal.md`
- Fișiere afectate: `~/.nexus/intel/ingest.py`, `~/.nexus/intel/notion-sync.py`, `~/.nexus/echelon/nexus-unified.sh`, `~/.nexus/scripts/ingest-25-targeted.py`, `~/.nexus/inbox/subscriptions.json`
- Fișiere noi: `~/.nexus/scripts/feed-monitor.py`, `~/Library/LaunchAgents/com.nexus.feed-monitor.plist`
