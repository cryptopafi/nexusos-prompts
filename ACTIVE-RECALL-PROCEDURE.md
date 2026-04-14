---
type: procedure
created: 2026-03-17
status: active
slug: active-recall-procedure
tags: [procedure, nexus]
---

# ACTIVE-RECALL-PROCEDURE — SM-2 spaced repetition review at session start
**Rule**: TRAINING-H-001 | **Version**: 1.0 | **FORGE**: ✅ | **Audit**: 2026-03-04

## Problema
Without structured active recall, learned skills decay. SM-2 review states become stale,
confidence scores drift from reality, and Pafi has no visibility into which skills need
reinforcement. Skipping reviews breaks the spaced repetition curve — intervals lose meaning.

## Procedura

### Pas 1: Trigger
- **Automatic**: Session Start Checklist Step 6 — Genie checks for due reviews.
- **Manual**: User says `recall` to trigger on-demand review cycle.
- Max 3 reviews per trigger (do not block the user with excessive reviews).

### Pas 2: Fetch Due Skills from Cortex
- Query Cortex collection `training` for skills where `next_review <= today`:
  ```
  # Cortex e HTTP server, nu CLI — folosim curl
  curl -s -X POST "http://100.81.233.9:6400/api/search" \
    -H "Content-Type: application/json" \
    -d "{\"collection\":\"training\",\"filter\":{\"next_review\":{\"lte\":\"$(date +%Y-%m-%d)\"}},\"sort\":\"next_review\",\"limit\":3}"
  ```
- Each skill record contains:
  ```json
  {"title": "...", "bloom_level": "...", "confidence": N,
   "next_review": "YYYY-MM-DD", "interval_days": N,
   "ease_factor": 2.5, "review_count": N}
  ```
- If no skills are due: report "No reviews due today" and exit.
- **Cortex unavailable**: report "Cortex unavailable — recall skipped" and exit cleanly. Do NOT block session start.
- **Cortex returns malformed JSON**: treat as "no skills due" and log `[RECALL-WARN] Malformed Cortex response — recall skipped`. Do NOT crash or block session start.
- **Timeout**: entire recall cycle must complete within 60 seconds. If Cortex query takes >10s, skip recall for this session.

### Pas 3: Present Review Question
- For each due skill (max 3), present to Pafi:
  ```
  [RECALL 1/3] "Skill Title"
  Bloom level: Apply | Last reviewed: 2026-02-20 | Interval: 14d
  > Question: [Generate question matching bloom_level]
  ```
- **Question generation**: Genie generates the question freeform using skill `title` + `bloom_level` as context. No external model call needed.
- Question difficulty matches `bloom_level`:
  - Remember/Understand: definition or concept recall ("Ce înseamnă X?", "Descrie Y.")
  - Apply/Analyze: scenario-based ("Cum ai folosi X pentru a rezolva Y?")
  - Evaluate/Create: synthesis/judgment ("Compară X cu Y. Care e mai bun pentru Z?")
- If user types `skip`: move to next skill, do NOT update SM-2 state for skipped skill.
- Wait for Pafi's answer before proceeding.

### Pas 4: Evaluate Answer
- Genie judges the answer as one of:
  - **correct** (confidence +1, max 5): covers core concept AND applies or explains correctly. No significant gaps.
  - **partial** (confidence unchanged): core concept present but incomplete, minor error, or vague. Missing 1-2 key elements.
  - **wrong** (confidence reset to 1): fundamental misunderstanding, blank answer, or "nu știu".
- Provide brief feedback (1-2 sentences): what was right, what was missed.
- If user types `skip`: exit Pas 4 without updating SM-2 for this skill.

### Pas 5: Update SM-2 State
- Recalculate based on evaluation:
  | Result  | Confidence | Interval Formula                        |
  |---------|------------|-----------------------------------------|
  | correct | min(c+1,5) | interval * ease_factor                  |
  | partial | c (same)   | interval * 1.0 (no change)             |
  | wrong   | 1          | 1 day (reset)                           |
- SM-2 interval mapping for new confidence:
  - 1 → +1d | 2 → +3d | 3 → +7d | 4 → +14d | 5 → +30d
- Ease factor adjustment:
  - correct: `ease_factor + 0.1` (max 3.0)
  - partial: `ease_factor` (unchanged)
  - wrong: `max(ease_factor - 0.2, 1.3)`
- Calculate `next_review = today + new_interval_days`.
- Increment `review_count` by 1.

### Pas 6: Store Updated State to Cortex
- Update the skill record in Cortex collection `training`:
  ```
  curl -s -X POST "http://100.81.233.9:6400/api/store" \
    -H "Content-Type: application/json" \
    -d "{\"collection\":\"training\",\"text\":\"RECALL update for <skill_id>\",\"metadata\":{\"skill_id\":\"<skill_id>\",\"confidence\":N,\"next_review\":\"YYYY-MM-DD\",\"interval_days\":N,\"ease_factor\":N,\"review_count\":N,\"source\":\"active_recall\"}}"
  ```
- Verify write succeeded (Cortex returns updated record).
- **Write failure**: retry once. If still failing, log: `[RECALL-WARN] Write failed for "{title}" — state NOT updated. Review again next session.`

### Pas 7: Report Summary
- After all reviews (or if none due), output VK log lines:
  ```
  [RECALL] "CSS Grid Layout" | result: correct | new_interval: 14d | next: 2026-03-18
  [RECALL] "SQL Window Functions" | result: wrong | new_interval: 1d | next: 2026-03-05
  ```
- Final summary:
  ```
  [RECALL SUMMARY] Reviewed: 2 | Correct: 1 | Partial: 0 | Wrong: 1 | Next due: 2026-03-05
  ```

## Enforcement Loop
**WHERE**: Session start (Step 6 of checklist) or manual `recall` trigger
**WHEN**: Every session where `next_review <= today` for any skill in Cortex training collection
**HOW**: Session Start Checklist enforces Step 6; skipping recall is a checklist violation
**CONNECT**: TRAINING-MODE.md (SM-2 definitions), SKILL-DISCOVERY.md (new skill ingestion), AUDIT-IMPROVEMENT-CYCLE.md

## VK (Verification Keys)
- `VK-RECALL-001`: Due skills query returns only records where `next_review <= today`, sorted ascending by date
- `VK-RECALL-002`: Each question matches the skill's bloom_level tier (Remember→definition, Apply→scenario, Evaluate→judgment)
- `VK-RECALL-003`: SM-2 recalculation correct — interval * ease_factor for correct, reset to 1d for wrong, ease_factor bounded [1.3, 3.0]
- `VK-RECALL-004`: Cortex write confirmed — updated record returned with new next_review date
- `VK-RECALL-005`: Each reviewed skill emits exactly one log line: `[RECALL] "{title}" | result: X | new_interval: Nd | next: YYYY-MM-DD`

## VERIFY
[ ] Pas 1: Trigger fired (session start or manual)
[ ] Pas 2: Cortex query returned due skills (or confirmed none due)
[ ] Pas 3: Questions presented with bloom level and last review date
[ ] Pas 4: Answers evaluated as correct/partial/wrong with feedback
[ ] Pas 5: SM-2 state recalculated (interval, ease_factor, next_review)
[ ] Pas 6: Updated records stored to Cortex
[ ] Pas 7: Summary report printed with counts and next due date
