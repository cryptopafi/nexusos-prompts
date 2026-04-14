---
type: procedure
created: 2026-03-17
status: active
slug: nexus-pipeline-pattern
tags: [procedure, nexus]
---

# NEXUS Pipeline Pattern — Dev Pattern Collection

**Status**: ACTIVE
**Creat**: 2026-03-08
**Versiune**: 1.0
**Regulă asociată**: DEV-H-013
**Scope**: Template pentru componente NEXUS Research pipeline (TypeScript relay + Python orchestration)
**Extras din**: m4-296, m4-297, m4-298, m4-299, m4-303, m4-309, m4-310, m4-318, m4-320

---

## 1. Problema

NEXUS pipeline are 2 runtime-uri (TypeScript/Bun pentru relay + Python pentru orchestration).
Componentele trebuie: model routing corect, EPR scoring, safety guards, și integrare Cortex.
Fără pattern: model routing inconsistent, timeout-uri necontrolate, iteration loops infinite.

---

## 2. Pattern Template

### TypeScript Component (relay.ts additions)
```typescript
// PATTERN: Command handler în relay.ts
// Location: ~/repos/godagoo/claude-telegram-relay/relay.ts

// 1. COMMAND REGISTRATION — în switch/case existent
case '/nexus-{command}':
  await handle{Command}(ctx, args);
  break;

// 2. HANDLER FUNCTION
async function handle{Command}(ctx: Context, args: string[]): Promise<void> {
  // TIMEOUT GUARD — obligatoriu
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), 5 * 60 * 1000); // 5min max

  try {
    // ITERATION CAP — obligatoriu (m4-320 lesson)
    const MAX_ITERATIONS = 10;
    let iteration = 0;

    while (iteration < MAX_ITERATIONS) {
      // ... logic
      iteration++;
    }

    if (iteration >= MAX_ITERATIONS) {
      logger.warn(`Iteration cap reached for ${command}`);
    }

  } catch (error) {
    if (error.name === 'AbortError') {
      await ctx.reply('⏱ Timeout — task cancelled.');
    } else {
      // LOG STDERR — obligatoriu (m4-320 lesson)
      logger.error(`handle${Command} error:`, error);
      await ctx.reply(`❌ Error: ${error.message}`);
    }
  } finally {
    clearTimeout(timeout);
  }
}
```

### Python Orchestration Component (nexus-run.sh / scripts)
```python
# PATTERN: NEXUS Python orchestration script
# Location: ~/.nexus/scripts/

import subprocess
import json
import sys
import logging
from pathlib import Path

logger = logging.getLogger("nexus")

# MODEL ROUTING — obligatoriu
MODEL_ROUTING = {
    "triage": "claude-haiku-4-5-20251001",    # clasificare rapidă
    "research": "claude-sonnet-4-6",           # research standard
    "synthesis": "claude-opus-4-6",            # synthesis deep
    "fallback": "claude-sonnet-4-6",           # când opus fail
}

# EPR SCORING — integrat în output
def calculate_epr(sources: list, confidence: float, coverage: float) -> int:
    """EPR score 0-20. Min 14 standard, 16 critic."""
    source_score = min(len(sources) * 2, 8)        # max 8 pts (4+ surse)
    confidence_score = int(confidence * 6)          # max 6 pts
    coverage_score = int(coverage * 6)              # max 6 pts
    return source_score + confidence_score + coverage_score

# SUBPROCESS EXECUTION — cu timeout + stderr capture
def run_subprocess(cmd: list, timeout: int = 300) -> dict:
    """Run subprocess with timeout. Returns {success, stdout, stderr}."""
    try:
        result = subprocess.run(
            cmd,
            capture_output=True,
            text=True,
            timeout=timeout
        )
        return {
            "success": result.returncode == 0,
            "stdout": result.stdout,
            "stderr": result.stderr,
        }
    except subprocess.TimeoutExpired:
        logger.error(f"Timeout ({timeout}s) for: {' '.join(cmd)}")
        return {"success": False, "stdout": "", "stderr": f"Timeout after {timeout}s"}
    except FileNotFoundError as e:
        logger.error(f"Command not found: {cmd[0]}")
        return {"success": False, "stdout": "", "stderr": str(e)}
```

### Safety Guards Checklist
- [ ] Timeout pe ORICE subprocess call (max 5 min default)
- [ ] Iteration cap pe ORICE loop (max 10 default)
- [ ] stderr capture pe ORICE subprocess (nu pierde error info)
- [ ] Model routing explicit (nu hardcodat în cod — citit din config)
- [ ] EPR score calculat la output (nu opțional)
- [ ] Fallback model specificat (când primary fail)

---

## 3. Cortex Logging

```json
{
  "text": "Dev pattern: NEXUS pipeline template v1.0 — TS relay + Python orchestration with safety guards",
  "collection": "procedures",
  "metadata": {
    "type": "dev-pattern",
    "procedure": "nexus-pipeline-pattern",
    "rule_id": "DEV-H-013",
    "domain": "nexus",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["typescript", "python", "pipeline", "timeout", "iteration-cap", "model-routing", "epr"]
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- TECH workflow Step 3a/3b — la orice task NEXUS pipeline

### WHEN
- La fiecare task cu domain=NEXUS și tip=pipeline/orchestration/command

### HOW
- Cod fără timeout guard → FORGE-AUDIT FAIL
- Cod fără iteration cap → FORGE-AUDIT FAIL (m4-320 was a real incident)
- Subprocess fără stderr capture → FORGE-AUDIT CONDITIONAL

### VERIFY
- [ ] Timeout guard prezent pe subprocess + async ops
- [ ] Iteration cap pe orice loop
- [ ] Model routing din config, nu hardcodat
