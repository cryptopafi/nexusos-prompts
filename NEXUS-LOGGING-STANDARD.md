---
type: procedure
created: 2026-03-17
status: active
slug: nexus-logging-standard
tags: [procedure, nexus]
---

# NEXUS-LOGGING-STANDARD — Centralized Logging Convention v1.0

## Overview
Standard log format for all NexusOS agent and service logs.
Enables cross-agent correlation, unified search, and observability.

**See also**: NEXUS-MESSAGING-PROTOCOL.md — the `correlation_id` field in message envelopes is the same ID used in log lines, linking inter-agent messages to their log entries.

## Log Line Format

```
YYYY-MM-DDTHH:MM:SSZ [LEVEL] [AGENT] [CORRELATION_ID] message key=value key=value
```

### Fields:
- **Timestamp**: ISO 8601 UTC, always ending in `Z`
- **LEVEL**: `DEBUG` | `INFO` | `SKIP` | `WARN` | `ERROR` | `CRITICAL`
- **AGENT**: `genie` | `iris` | `mercury` | `tech` | `sentinel` | `relay` | `echelon`
- **CORRELATION_ID**: Task ID or message envelope correlation_id (use `-` if none)
- **message**: Free-form text, brief
- **key=value pairs**: Optional structured data (no spaces in values, quote if needed)

### Examples:
```
2026-03-10T14:30:00Z [INFO] [iris] [research-042] heartbeat START
2026-03-10T14:30:01Z [SKIP] [iris] [research-042] no work pending
2026-03-10T14:32:15Z [INFO] [iris] [research-042] source-gather complete sources=4 epr=16
2026-03-10T14:33:00Z [WARN] [genie] [research-042] cost warning spent_usd=1.65 budget_usd=2.00
2026-03-10T14:35:00Z [ERROR] [sentinel] [-] cortex unreachable port=6400 retry=1
2026-03-10T14:35:30Z [INFO] [sentinel] [-] cortex retry succeeded
2026-03-10T14:40:00Z [INFO] [relay] [research-042] approval GO responded_by=pafi
```

## Log File Locations

All agent logs MUST write to `~/.nexus/logs/`:

| Agent | Log File | Writer |
|-------|----------|--------|
| GENIE | `genie-heartbeat.log` | heartbeat.sh |
| IRIS | `iris-heartbeat.log` | heartbeat.sh |
| SENTINEL | `sentinel-health.log` | health-check.sh |
| TECH | `tech-activity.log` | codex-medic.sh + manual |
| RELAY | `relay.log` | relay.ts (via appendFile) |
| ECHELON | `echelon-{source}.log` | enricher scripts |

## Bash Helper

Add to any bash script:
```bash
nexus_log() {
    local level="$1" agent="$2" corr_id="$3" msg="$4"
    echo "$(date -u +%FT%TZ) [$level] [$agent] [$corr_id] $msg" >> "$HOME/.nexus/logs/${agent}-${SCRIPT_NAME:-activity}.log"
}
```

Usage:
```bash
SCRIPT_NAME="heartbeat"
nexus_log "INFO" "iris" "research-042" "heartbeat START"
nexus_log "SKIP" "iris" "-" "no work pending"
```

## Level Guidelines

| Level | Severity | When |
|-------|----------|------|
| DEBUG | 0 | Verbose detail (normally off, enable for investigation) |
| SKIP | 1 | Heartbeat ran but had nothing to do (no-op cycle) |
| INFO | 2 | Normal operation milestones (start, complete, key decisions) |
| WARN | 3 | Non-critical issue (approaching threshold, retry succeeded) |
| ERROR | 4 | Operation failed but agent continues |
| CRITICAL | 5 | Agent halted, requires human intervention |

Hierarchy: `DEBUG < SKIP < INFO < WARN < ERROR < CRITICAL`
`--level INFO` shows INFO + WARN + ERROR + CRITICAL (hides DEBUG and SKIP).

## Correlation Rules
- Heartbeat cycles: use task's `correlation_id` from PROGRESS.md if executing, else `-`
- Approval flow: use `task_id` from APPROVAL-RESPONSE.md
- Health checks: use `-` (no specific task context)
- Multi-step tasks: same `correlation_id` across all agents involved

## Unified Log Viewer
Use `~/.nexus/scripts/nexus-logs.sh` to search across all logs:
```bash
# All logs from last hour
nexus-logs.sh --since 1h

# All ERROR+ for a specific task
nexus-logs.sh --level ERROR --corr research-042

# All iris activity today
nexus-logs.sh --agent iris --since today
```

## Retention
- Active logs: keep 7 days in `~/.nexus/logs/`
- Archive: rotate weekly to `~/.nexus/logs/archive/` (gzip)
- ECHELON DB (`nexus.db`): permanent (SQLite, 22MB)

## Migration
Existing scripts should adopt the format gradually:
1. New scripts: use `nexus_log()` from day one
2. Existing heartbeat.sh: update `echo "$(date ...) START"` to structured format
3. Python services: update logging format string
4. Relay: add file appender alongside console.log
