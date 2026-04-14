---
type: procedure
created: 2026-03-17
status: active
slug: infra-safety-pattern
tags: [procedure, nexus]
---

# Infrastructure Safety Pattern — Dev Pattern Collection

**Status**: ACTIVE
**Creat**: 2026-03-08
**Versiune**: 1.0
**Regulă asociată**: DEV-H-013
**Scope**: Template pentru infrastructure scripts (circuit breakers, LaunchAgents, monitoring, alerts)
**Extras din**: m4-294, m4-295, m4-301, m4-316, m4-319, m4-320, m4-339

---

## 1. Problema

Infrastructure scripts gestionează monitoring, sync, alerting — sisteme critice care trebuie să fie resilient.
Fără pattern: silent failures, log rotation lipsă, Telegram alerts cu formatting spart.

---

## 2. Pattern Template

### Circuit Breaker (Python — m4-316 pattern)
```python
# PATTERN: Circuit breaker cu Telegram alerting
# Location: ~/.nexus/scripts/notion_circuit.py (referință reală)

import json
import time
import logging
from pathlib import Path

logger = logging.getLogger("circuit")

CIRCUIT_FILE = Path.home() / ".nexus/sync/circuit-state.json"

class CircuitBreaker:
    """3-state circuit breaker: CLOSED (normal) → OPEN (failing) → HALF_OPEN (testing)."""

    def __init__(self, name: str, failure_threshold: int = 3, reset_timeout: int = 300):
        self.name = name
        self.failure_threshold = failure_threshold
        self.reset_timeout = reset_timeout
        self.state = self._load_state()

    def _load_state(self) -> dict:
        try:
            data = json.loads(CIRCUIT_FILE.read_text())
            return data.get(self.name, {"state": "CLOSED", "failures": 0, "last_failure": 0})
        except (FileNotFoundError, json.JSONDecodeError):
            return {"state": "CLOSED", "failures": 0, "last_failure": 0}

    def _save_state(self):
        try:
            data = json.loads(CIRCUIT_FILE.read_text())
        except (FileNotFoundError, json.JSONDecodeError):
            data = {}
        data[self.name] = self.state
        # ATOMIC WRITE
        tmp = CIRCUIT_FILE.with_suffix('.tmp')
        tmp.write_text(json.dumps(data, indent=2))
        tmp.replace(CIRCUIT_FILE)

    def can_execute(self) -> bool:
        if self.state["state"] == "CLOSED":
            return True
        if self.state["state"] == "OPEN":
            if time.time() - self.state["last_failure"] > self.reset_timeout:
                self.state["state"] = "HALF_OPEN"
                self._save_state()
                return True
            return False
        return True  # HALF_OPEN → try once

    def record_success(self):
        self.state = {"state": "CLOSED", "failures": 0, "last_failure": 0}
        self._save_state()

    def record_failure(self):
        self.state["failures"] += 1
        self.state["last_failure"] = time.time()
        if self.state["failures"] >= self.failure_threshold:
            self.state["state"] = "OPEN"
            logger.error(f"Circuit {self.name} OPEN after {self.failure_threshold} failures")
        self._save_state()
```

### LaunchAgent plist Template
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.nexus.{service-name}</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>-c</string>
        <string>{SCRIPT_PATH}</string>
    </array>
    <key>StartInterval</key>
    <integer>{INTERVAL_SECONDS}</integer>
    <key>StandardOutPath</key>
    <string>/tmp/nexus-{service-name}.stdout.log</string>
    <key>StandardErrorPath</key>
    <string>/tmp/nexus-{service-name}.stderr.log</string>
    <key>EnvironmentVariables</key>
    <dict>
        <key>PATH</key>
        <string>/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin</string>
        <key>HOME</key>
        <string>/Users/pafi</string>
    </dict>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

### Telegram Alert Formatting (m4-319 lesson)
```bash
# WRONG — literal \n in message
MSG="Line 1\nLine 2"
curl -s "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" -d "chat_id=${CHAT_ID}&text=${MSG}"

# CORRECT — use printf for real newlines
MSG=$(printf "Line 1\nLine 2")
curl -s "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
  -d "chat_id=${CHAT_ID}" \
  --data-urlencode "text=${MSG}" \
  -d "parse_mode=Markdown"
```

### Log Rotation Pattern
```bash
# PATTERN: rotate_log_tail — keep last N lines, atomic
rotate_log_tail() {
    local file="$1"
    local max_lines="${2:-1000}"

    if [ ! -f "$file" ]; then return 0; fi

    local current_lines=$(wc -l < "$file")
    if [ "$current_lines" -le "$max_lines" ]; then return 0; fi

    # ATOMIC — tmp + mv
    local tmp="${file}.tmp"
    tail -n "$max_lines" "$file" > "$tmp"
    mv "$tmp" "$file"
}
```

### Checklist
- [ ] Circuit breaker pe orice API call extern care poate fail
- [ ] Atomic writes pe orice shared state file
- [ ] LaunchAgent cu StandardErrorPath (nu pierde errors)
- [ ] PATH explicit în LaunchAgent (homebrew nu e default)
- [ ] Telegram: printf + --data-urlencode (nu literal \n)
- [ ] Log rotation pe fișiere care cresc (>1000 linii)
- [ ] OSError guard pe file operations (disk full, permission denied)

---

## 3. Cortex Logging

```json
{
  "text": "Dev pattern: Infrastructure safety v1.0 — circuit breaker, LaunchAgent, Telegram alerts, log rotation",
  "collection": "procedures",
  "metadata": {
    "type": "dev-pattern",
    "procedure": "infra-safety-pattern",
    "rule_id": "DEV-H-013",
    "domain": "infrastructure",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["circuit-breaker", "launchagent", "telegram", "log-rotation", "atomic-write"]
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- TECH workflow Step 3a/3b — la orice task infrastructure/monitoring/alerting

### WHEN
- La task-uri cu domain=infra și tip=script/daemon/sync/alert

### HOW
- Script fără circuit breaker pe API calls → FORGE-AUDIT CONDITIONAL
- LaunchAgent fără StandardErrorPath → FORGE-AUDIT FAIL
- Telegram cu literal \n → FORGE-AUDIT FAIL (known bug pattern)
- Shared file write fără atomic → FORGE-AUDIT FAIL

### VERIFY
- [ ] Circuit breaker pe API calls
- [ ] Atomic writes pe shared state
- [ ] LaunchAgent stderr logging
- [ ] Telegram formatting corect
