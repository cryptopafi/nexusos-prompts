# Prompt Library — Master Index

**Location**: `~/.nexus/prompt-library/`
**Updated**: 2026-03-19 (consolidated)
**Total local prompts**: 153 files centralizate (symlinks inverse în locațiile originale)
**Total Cortex prompts**: ~620+ entries (collection `technical`, tag `prompting-library`)

## Consolidare 2026-03-19 — Symlinks active

| Symlink | Target |
|---------|--------|
| `memory/agent-prompts` → | `prompt-library/agents/` (dir) |
| `agents/genie/SOUL.md` → | `prompt-library/souls/genie.md` |
| `agents/tech/SOUL.md` → | `prompt-library/souls/tech.md` |
| `agents/iris/SOUL.md` → | `prompt-library/souls/iris.md` |
| `agents/mercury/SOUL.md` → | `prompt-library/souls/mercury.md` |
| `agents/sentinel/SOUL.md` → | `prompt-library/souls/sentinel.md` |
| `agents/tech/sub-agents/builder/SOUL.md` → | `prompt-library/souls/builder.md` |
| `agents/tech/sub-agents/fixer/SOUL.md` → | `prompt-library/souls/fixer.md` |
| `agents/tech/sub-agents/integrator/SOUL.md` → | `prompt-library/souls/integrator.md` |
| `agents/tech/sub-agents/pipeliner/SOUL.md` → | `prompt-library/souls/pipeliner.md` |
| `procedures/training/ai-prompt-engineering` → | `prompt-library/techniques/` (dir) |
| `memory/promptforge.md` → | `prompt-library/methodology/promptforge.md` |
| `memory/promptforge-v3.md` → | `prompt-library/methodology/promptforge-v3.md` |
| `memory/promptforge-training.md` → | `prompt-library/methodology/promptforge-training.md` |
| `memory/lis-system-prompt-v2.md` → | `prompt-library/bots/lis-system-prompt.md` |
| `memory/openclaw-configs/souls/*.md` → | `prompt-library/openclaw/*.md` (8 files) |
| `relay/src/prompts/system.xml` → | `prompt-library/bots/lis-system.xml` |
| `kara-telegram-relay/src/prompts/system.xml` → | `prompt-library/bots/kara-system.xml` |

---

## Structure

```
~/.nexus/prompt-library/
  INDEX.md                    <- this file
  agents/                     <- 12 OpenClaw agent role prompts (from memory/agent-prompts/)
  souls/
    nexus/                    <- 9 NexusOS agent SOULs (genie, iris, mercury, sentinel, tech + 4 sub-agents)
    openclaw/                 <- 8 OpenClaw agent SOULs (afy, dessie, mark, ressie, rich, sage, scottie, tech)
  system-prompts/             <- 5 Telegram bot prompts (lis, kara, health, luna + lis reference)
  adapters/                   <- 7 pipeline adapter prompts (.txt)
  pe-techniques/              <- 116 PE technique procedures (C-0XX, PR-0XX, ND-0XX) via symlink
  promptforge/                <- 4 PromptForge system docs (v3.6, PROMPTING v1.7, SOL v1.4, training plan)
  sol-state/                  <- manifest.json + queue.json (SOL runtime state)
  patterns/                   <- empty, to be populated with extracted patterns P1-P24+
```

---

## Cortex Entries (NOT on disk — query Cortex to access)

| Batch | Count | Tags | Date |
|-------|-------|------|------|
| Original external | 160 | `prompting-library, external-source` | 2026-03-06 |
| Web design | 62 | `prompting-library, web-design, 2026-03-19` | 2026-03-19 |
| GitHub repos | ~270 | `prompting-library, batch-2026-03-19, github` | 2026-03-19 |
| Marketplaces | ~350-400 | `prompting-library, batch-2026-03-19` | 2026-03-19 |
| **Total Cortex** | **~620+** | `prompting-library` | — |

Query: `mcp__cortex__cortex_search(query="...", collection="technical")` — filter by tag `prompting-library`.

---

## Embedded Prompts (in scripts, cannot extract — INDEX ONLY)

16 Python scripts in `~/.nexus/scripts/` contain embedded LLM prompts:
`calendar-intelligence.py`, `comm-intel-audit.py`, `comms-insight.py`, `financial-insight.py`, `financial-monitor.py`, `inbox-triage-v2.py`, `ingest-25-targeted.py`, `knowledge-capture.py`, `liked-content-ingestion.py`, `morning-brief.py`, `prompt-genie-experiment.py`, `prompt-optimizer-discover.py`, `radar-light-digest.py`, `synthesize-25-opus.py`, `task-extraction.py`, `triage-feedback.py`

---

## External Reference Libraries (bulk, NOT native — INDEX ONLY)

`~/.nexus/research/github-references/awesome-chatgpt-prompts/` — 71K+ prompts, 210MB
Plus 4 marketing/SEO repos (~4MB combined).

---

## Training Skills (active plugin — INDEX ONLY)

`~/.claude/plugins/training-library/skills/` — 47 skill directories

---

## Known Issues

1. **Stale copy**: `~/.claude/agent-prompts/` has outdated versions of the 12 agent prompts — DELETE recommended (primary is `memory/agent-prompts/`)
2. **Forge SOUL missing**: `~/.nexus/agents/forge/SOUL.md` does not exist
3. **Patterns dir empty**: `patterns/` has no P1-P24 files yet — populate from BRAIN-DUMP.md
4. **SOL manifest incomplete**: 10 prompts not tracked (5 TECH sub-agents + 4 insight scripts + sdr.md)
5. **12 REVIEW prompts in Cortex**: leaked system prompts marked for review, reference only

---

## SOL Audit Status

| Category | Tracked | Audited | Avg Score |
|----------|---------|---------|-----------|
| Agent prompts | 12/12 | 12 (9 changed) | 80.2 |
| NexusOS SOULs | 5/9 | 1 (forge=88) | — |
| Production scripts | 4/8 | 2 | 72.0 |
| System prompts | 0/5 | 1 (Lis FORGE) | — |
| Cortex library | 620+/620+ | 0 (security audit only) | — |
