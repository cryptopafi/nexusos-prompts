---
type: procedure
created: 2026-03-17
status: active
slug: skool-extraction-procedure
tags: [procedure, nexus]
---

# SKOOL-EXTRACTION-PROCEDURE — Automated Skool community post extraction pipeline
**Rule**: ECHELON-S-001 | **Version**: 1.0 | **FORGE**: ✅ | **Audit**: 2026-03-04

## 1. Problema
Without a structured extraction pipeline, Skool community posts are lost or manually copied.
Duplicate ingestion wastes Cortex storage. Missing author/category/engagement metadata
breaks downstream ENRICH cross-referencing and AUDIT-IMPROVEMENT-CYCLE checks (AUD-05).

## 2. Procedura

### Pas 1: Trigger
- **Scheduled**: Daily cron at 09:00 via `launchctl` on MacM4.
- **Manual**: Run `cd ~/repos/skool-extractor && node skool-reader.js`.
- Before any run, confirm `communities.json` lists target communities.

### Pas 2: Auth Check
- Verify Keychain credentials exist:
  ```bash
  security find-generic-password -s SKOOL_EMAIL -w >/dev/null 2>&1 && \
  security find-generic-password -s SKOOL_PASSWORD -w >/dev/null 2>&1 && \
  echo "AUTH OK" || echo "AUTH MISSING — add credentials to Keychain first"
  ```
- If AUTH MISSING: STOP. Do not proceed without valid credentials.

### Pas 3: Dry-Run Validation
- Run with dry-run flag first:
  ```bash
  node skool-reader.js --dry-run
  ```
- Verify output includes: post URLs, authors, categories, engagement (likes, comments).
- If parsing errors appear, check fixes m4-158 / m4-159 (author/category/engagement fields).
- Only proceed to live run if dry-run output is clean.

### Pas 4: Live Scrape (Playwright)
- Run full extraction:
  ```bash
  node skool-reader.js
  ```
- Playwright launches headless Chromium, authenticates, scrapes configured communities.
- Output: array of post objects `{url, title, author, category, likes, comments, body, date}`.
- **Timeout**: 60s per page. Retry max 2x on transient failure.
- **On persistent Playwright failure**: log error for that community, skip it, continue with remaining communities. Do NOT abort entire run.

### Pas 5: Classify via Ollama
- Each post body is sent to `qwen2.5:7b` for topic classification.
- Model assigns: `topic_primary`, `topic_secondary`, `relevance_score` (0-10).
- Posts with `relevance_score < 3` are flagged as low-value (still stored, tagged `low-relevance`).
- **Ollama unavailable**: If qwen2.5:7b is not responding, store posts with `topic_primary: "unclassified"`, `relevance_score: -1`. Flag for re-classification in next run. Do NOT block pipeline.

### Pas 6: Dedup Check via Cortex
- For each post URL, search Cortex collection `skool`:
  ```
  cortex search --collection skool --field url --exact "<post_url>"
  ```
- If URL exists: skip (increment `duplicates_skipped` counter).
- If URL is new: proceed to store.

### Pas 7: Store to Cortex
- Save each new post to Cortex collection `skool` with full metadata:
  `{url, title, author, category, likes, comments, body, date, topic_primary, topic_secondary, relevance_score, ingested_at}`.
- Increment `new_posts_stored` counter.
- **Partial-run safety**: If crash occurs mid-storage, next run is safe to re-run — Pas 6 dedup check prevents double ingestion of already-stored posts.

### Pas 8: Cross-Reference with ENRICH Pipeline
- After storage, trigger ENRICH-PROCEDURE Mode A (auto-enrich new entries).
- Pass list of newly stored post IDs to ENRICH for entity extraction and linking.
- See: `ENRICH-PROCEDURE.md` — CALL ENRICH Mode A/B.

### Pas 9: Report
- Print summary to stdout and log to `~/.nexus/logs/skool-extraction.log`:
  ```
  [SKOOL-EXTRACT] 2026-03-04 09:03 | New: 12 | Duplicates: 3 | Low-relevance: 2 | Errors: 0
  ```
- If errors > 0, flag for manual review.

## 3. Cortex Logging

```json
{
  "text": "SKOOL-EXTRACTION: {N} new posts | {N} duplicates skipped | {N} errors | communities: {list}",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SKOOL-EXTRACTION-PROCEDURE",
    "rule_id": "ECHELON-S-001",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["skool", "extraction", "ingestion", "automation"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- MacM4, daily cron job + manual trigger

### WHEN
- Daily 09:00 UTC or manual invocation

### HOW (violation detection)
- AUDIT-IMPROVEMENT-CYCLE AUD-05 checks Skool Reader output daily post-run; missing runs trigger alert
- Extraction fără dedup check (Pas 6 skipped) → violation
- ENRICH pipeline not triggered after storage → warning

### CONNECT
- `ECHELON-S-001` → regula sursă
- `ENRICH-PROCEDURE.md` (Mode A/B) → post-storage enrichment
- `AUDIT-IMPROVEMENT-CYCLE.md` (AUD-05) → monitoring runs
- `INGESTION-DUPLICATE-CHECK.md` → dedup pattern
- `procedure-health.json` → entry `SKOOL-EXTRACTION-PROCEDURE`

### VK (Verification Keys — pipeline)
- `VK-SKOOL-001`: Keychain credentials SKOOL_EMAIL and SKOOL_PASSWORD are present
- `VK-SKOOL-002`: Dry-run completes without parsing errors (author, category, engagement all populated)
- `VK-SKOOL-003`: Dedup prevents duplicate URLs from being stored in Cortex
- `VK-SKOOL-004`: Report line logged with correct counts (new + duplicates + errors = total scraped)
- `VK-SKOOL-005`: ENRICH pipeline triggered for all newly stored posts

### VERIFY
- [ ] Pas 1: Trigger confirmed (cron or manual)
- [ ] Pas 2: Auth check passed
- [ ] Pas 3: Dry-run output clean
- [ ] Pas 4: Live scrape completed
- [ ] Pas 5: Classification assigned to all posts
- [ ] Pas 6: Dedup check ran against Cortex
- [ ] Pas 7: New posts stored to Cortex collection skool
- [ ] Pas 8: ENRICH pipeline triggered
- [ ] Pas 9: Report logged with correct counts
- [ ] VK emis in sesiune

**VK-uri obligatorii (FORGE)**:
1. `✅ [PROC] SKOOL-EXTRACTION-PROCEDURE | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Skool Extraction" | FORGE ✓ | rule: ECHELON-S-001 | v1.0`
