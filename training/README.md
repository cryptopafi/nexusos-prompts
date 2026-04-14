---
type: procedure
created: 2026-03-17
status: active
slug: readme
tags: [procedure, nexus]
---

# Training Procedure Tracking

This folder is tracked by the PRISM Training Collection system.

## Components
- Notion DB bootstrap/populate: `~/.nexus/sync/notion-populate-training-procedures.py`
- Cortex <-> Notion progress sync: `~/.nexus/scripts/training-progress-sync.sh`
- CLI dashboard: `~/.nexus/scripts/training-status.sh [domain]`

## Workflow
1. Create/find and populate Notion database:
   - `python3 ~/.nexus/sync/notion-populate-training-procedures.py`
2. Sync progress records between Notion and Cortex:
   - `bash ~/.nexus/scripts/training-progress-sync.sh both`
3. Check progress by domain:
   - `bash ~/.nexus/scripts/training-status.sh`
   - `bash ~/.nexus/scripts/training-status.sh finance`

## Current count
- Local training procedure files detected from `~/.nexus/procedures/training/*/*.md` (excluding `INDEX.md`/`README.md`): **260**.
- If target count differs (e.g. 297 expected by older brief), refresh source set before re-running populate.

## Phase 4+5 Integrations

### Phase 4 — ECHELON Link Hook
- Script: `~/.nexus/scripts/training-echelon-link.sh`
- Purpose: map ECHELON-enriched content to a training procedure domain and mark `relevant_content_available=true` in the training Notion DB.
- Manual test:
  - `bash ~/.nexus/scripts/training-echelon-link.sh --domain finance --title "AI valuation framework" --tags "finance,valuation" --text "market signals and financial modeling" --dry-run`

### Phase 5 — Active Recall Scheduler
- Script: `~/.nexus/scripts/training-recall-scheduler.sh`
- Logic: pulls `training_progress` records from Cortex, selects entries with `confidence < 3` and `last_reviewed` older than 14 days, appends to `~/.nexus/inbox/active-recall-queue.txt`.
- LaunchAgent: `~/Library/LaunchAgents/com.genie.training-recall-scheduler.plist`
  - schedule: daily 08:00
- Manual test:
  - `bash ~/.nexus/scripts/training-recall-scheduler.sh`
