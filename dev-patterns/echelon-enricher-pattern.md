---
type: procedure
created: 2026-03-17
status: active
slug: echelon-enricher-pattern
tags: [procedure, nexus]
---

# ECHELON Enricher Pattern — Dev Pattern Collection

**Status**: ACTIVE
**Creat**: 2026-03-08
**Versiune**: 1.0
**Regulă asociată**: DEV-H-013 (TECH scrie brief/pattern, nu cod direct >50 linii)
**Scope**: Template pentru orice enricher ECHELON Layer 2 (Python scraper → JSON → Cortex)
**Extras din**: m4-288, m4-289, m4-290, m4-291, m4-292, m4-302, m4-311, m4-329

---

## 1. Problema

ECHELON Layer 2 enrichers sunt scripturi Python care:
- Scrapează/parsează un canal specific (Newsletter, HackerNews, GitHub, Reddit, Instagram, Bluesky, LinkedIn, TikTok)
- Îmbogățesc datele cu metadata structurată
- Salvează JSON output standardizat
- Se integrează cu Cortex pentru persistență

Fără pattern: fiecare enricher e scris de la zero → inconsistențe, null safety bugs, retry logic lipsă.

---

## 2. Pattern Template

### Structura fișier
```
~/.nexus/echelon/enrichers/{channel}-enricher.py
```

### Schema obligatorie per enricher
```python
# 1. IMPORTS — standard set
import json, os, sys, time, logging
from pathlib import Path
from datetime import datetime

# 2. CONFIG — nu hardcodat
CONFIG = {
    "channel": "{channel_name}",
    "output_dir": Path.home() / ".nexus/echelon/data/{channel}/",
    "max_retries": 3,
    "retry_backoff": [30, 120, 300],  # exponential
    "timeout": 30,
}

# 3. LOGGING — structured
logging.basicConfig(level=logging.INFO, format='%(asctime)s %(levelname)s %(message)s')
logger = logging.getLogger(CONFIG["channel"])

# 4. MAIN FUNCTION — try/except SPECIFIC
def enrich(item: dict) -> dict:
    """Enrich a single item. Returns enriched dict or None on failure."""
    try:
        # Channel-specific logic here
        enriched = {
            "id": item.get("id"),  # ALWAYS .get(), never item["id"]
            "title": item.get("title", ""),
            "url": item.get("url", ""),
            "enriched_at": datetime.utcnow().isoformat(),
            "channel": CONFIG["channel"],
            # ... channel-specific fields
        }
        return enriched
    except requests.exceptions.RequestException as e:
        logger.error(f"Network error enriching {item.get('id', 'unknown')}: {e}")
        return None
    except (KeyError, TypeError, ValueError) as e:
        logger.error(f"Data error enriching {item.get('id', 'unknown')}: {e}")
        return None

# 5. BATCH PROCESSING — with atomic write
def process_batch(items: list, output_path: Path) -> dict:
    """Process batch with atomic write. Returns stats."""
    results = []
    stats = {"total": len(items), "success": 0, "failed": 0, "skipped": 0}

    for item in items:
        result = enrich(item)
        if result:
            results.append(result)
            stats["success"] += 1
        else:
            stats["failed"] += 1

    # ATOMIC WRITE — tmp + rename
    tmp_path = output_path.with_suffix('.tmp')
    with open(tmp_path, 'w') as f:
        json.dump(results, f, indent=2, ensure_ascii=False)
    os.replace(str(tmp_path), str(output_path))  # atomic on POSIX

    logger.info(f"Batch complete: {stats}")
    return stats

# 6. ENTRY POINT
if __name__ == "__main__":
    # Parse args, load input, call process_batch
    pass
```

### Null Safety Checklist (OBLIGATORIU — top motiv resubmit)
- [ ] Folosește `item.get("key")` nu `item["key"]`
- [ ] Verifică `if value is not None` înainte de `.split()`, `.strip()`, `.lower()`
- [ ] Verifică `if response and response.status_code == 200` (nu doar status_code)
- [ ] Lists: `items = data.get("items", [])` nu `data["items"]`
- [ ] Nested: `data.get("meta", {}).get("count", 0)` nu `data["meta"]["count"]`

### Retry Pattern (OBLIGATORIU)
```python
for attempt, backoff in enumerate(CONFIG["retry_backoff"]):
    try:
        response = requests.get(url, timeout=CONFIG["timeout"])
        response.raise_for_status()
        break
    except requests.exceptions.RequestException as e:
        logger.warning(f"Attempt {attempt+1} failed: {e}")
        if attempt < len(CONFIG["retry_backoff"]) - 1:
            time.sleep(backoff)
        else:
            logger.error(f"All retries exhausted for {url}")
            return None
```

---

## 3. Cortex Logging

```json
{
  "text": "Dev pattern: ECHELON enricher template v1.0 — Python scraper with null safety, retry, atomic write",
  "collection": "procedures",
  "metadata": {
    "type": "dev-pattern",
    "procedure": "echelon-enricher-pattern",
    "rule_id": "DEV-H-013",
    "domain": "echelon",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["python", "scraper", "enricher", "null-safety", "atomic-write", "retry"]
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- TECH workflow Step 3a (pattern-select) — caută acest pattern la orice task ECHELON enricher

### WHEN
- La fiecare task Codex cu domain=ECHELON și tip=enricher/scraper

### HOW
- Brief Codex fără null safety checklist → violation
- Delivery fără atomic write → FORGE-AUDIT FAIL
- Delivery fără retry logic → FORGE-AUDIT CONDITIONAL

### CONNECT
- FORGE-AUDIT procedură → verifică null safety + atomic + retry
- Codex brief template → include referință la acest pattern

### VERIFY
- [ ] Pattern aplicat sau explicit justificat de ce nu
- [ ] Null safety checklist complet
- [ ] Atomic write prezent
