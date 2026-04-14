---
type: procedure
created: 2026-03-17
status: active
slug: codex-error-detection
tags: [procedure, nexus]
---

# CODEX ERROR DETECTION — Procedură Standard

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: OPS-H-002
**Scope**: Niciun ERROR Codex nu trece nedetectat de Genie

---

## 1. Problema

Daemon-ul Codex poate produce ERROR din 3 cauze:
- **Timeout** (task depășește 1800s) — cel mai frecvent
- **Codex crash** (model fail, OOM, network)
- **Script error** (bug în prompt file sau sandbox)

Daemon-ul scrie ERROR în `codex-to-genie.md` și `~/.codex/errors/`, trimite Telegram.
Dar **Genie CLI nu avea mecanism de detecție** — ERROR-urile rămâneau invizibile.

### Root Cause (2026-02-27)
Delivery monitor (`codex-delivery-monitor.sh`) avea regex incompatibil:
- Căuta: `codex-task-NNN (DONE|FAILED|INCOMPLETE)`
- Daemon scria: `m4-NNN ERROR model=... duration=...`
- Pattern mismatch total → zero detecții

### Ce s-a pierdut
- m4-141: ERROR timeout — task era de fapt DONE (false positive)
- m4-146: ERROR timeout — task era de fapt DONE (false positive)
Ambele descoperite manual de Pafi, nu de Genie.

---

## 2. Soluția (3 layere)

### Layer 1: Delivery Monitor (FIXED)
**Fișier**: `~/.openclaw/scripts/codex-delivery-monitor.sh`
- Regex actualizat: prinde atât `codex-task-NNN` cât și `m4-NNN`
- Statusuri: `DONE|ERROR|FAILED|INCOMPLETE`
- La ERROR/FAILED: scrie `~/.codex/error.flag` (pe lângă `pending-delivery.flag`)

### Layer 2: Genie CLI Detection (OPS-H-002)
La FIECARE răspuns, Genie verifică:
```bash
cat ~/.codex/error.flag 2>/dev/null
```
Dacă flag există:
1. Citește task ID + status din flag
2. Citește detalii din `codex-to-genie.md` (grep pe task ID)
3. Verifică `~/.codex/errors/{task_id}.error.json` pentru detalii
4. **Triaj**:
   - Timeout + DONE deja în delivery → **false positive**, marchează DONE, șterge flag
   - Timeout + task incomplet → **re-queue** cu timeout mărit sau model mai mic
   - Crash real → **alertează Pafi** + queue retry
5. Șterge flag după procesare

### Layer 3: Session Start (COM-H-001 extension)
La session start, DUPĂ citirea `codex-to-genie.md`:
```bash
grep "ERROR" ~/.codex/codex-to-genie.md | tail -5
ls ~/.codex/errors/*.error.json 2>/dev/null | tail -5
```
Include orice ERROR neremediat în briefing.

---

## 3. Signal Files

| File | Scris de | Citit de | Când |
|------|----------|----------|------|
| `~/.codex/pending-delivery.flag` | Delivery monitor | Genie CLI (per-response) | Orice livrare (DONE/ERROR) |
| `~/.codex/error.flag` | Delivery monitor | Genie CLI (per-response) | Doar ERROR/FAILED |
| `~/.codex/errors/{id}.error.json` | Daemon direct | Genie (on-demand) | Doar ERROR |

---

## 4. Triaj Decision Tree

```
ERROR detectat
    ├── Verifică: task are și DONE în codex-to-genie.md?
    │   ├── DA → False positive (daemon timeout DUPĂ finalizare)
    │   │   ├── Marchează DONE în queue
    │   │   ├── Verifică fișierele pe disc (syntax check)
    │   │   └── Șterge error.flag + error.json
    │   └── NU → Error real
    │       ├── error_type == "timeout"?
    │       │   ├── DA → Re-queue cu timeout +50% sau model mai mic
    │       │   └── NU → Codex crash / script error
    │       │       ├── Citește error.json pentru stack trace
    │       │       ├── Fix prompt dacă posibil
    │       │       └── Alertează Pafi dacă nu se rezolvă
    └── Șterge error.flag după procesare
```

---

## 5. Enforcement Loop

```
┌──────────────────────────────────────────────────┐
│     CODEX ERROR DETECTION ENFORCEMENT            │
│                                                  │
│  PER-RĂSPUNS (real-time):                       │
│    □ cat ~/.codex/error.flag                    │
│    □ Dacă există → triaj (§4)                   │
│    □ Rezolvă sau escalează                      │
│    □ Șterge flag după procesare                 │
│                                                  │
│  SESSION START:                                  │
│    □ grep ERROR codex-to-genie.md | tail -5     │
│    □ ls ~/.codex/errors/*.error.json            │
│    □ Include în briefing                        │
│    □ Triaj pe orice ERROR nesoluționat          │
│                                                  │
│  DETECTION:                                      │
│    - error.flag ignorat >1 răspuns = violation  │
│    - ERROR descoperit de Pafi (nu Genie) =      │
│      violation gravă + review procedură         │
│    - Daemon cu 3+ ERROR consecutive = review    │
│      timeout/model config                       │
│                                                  │
│  CONNECTS TO:                                    │
│    - DEV-H-011 (delivery signal check)          │
│    - COM-H-001 (session start checklist)        │
│    - ERR-H-001 (error protocol)                 │
│    - OPS-H-001 (technical autonomy)             │
│                                                  │
└──────────────────────────────────────────────────┘
```

---

## 6. Metrici

- **MTTR (Mean Time To Resolve)**: Cât durează de la ERROR la rezolvare/re-queue
  - Target: <1 Genie response (dacă sesiune activă), <1 session start (dacă offline)
- **False Positive Rate**: ERROR-uri care sunt de fapt DONE
  - Target: 0% (fix daemon timeout detection)
- **Detection Miss Rate**: ERROR-uri descoperite de Pafi, nu de Genie
  - Target: 0% — NICIODATĂ

---

## 7. Prevenție Timeout False Positives

Daemon timeout = 1800s. Dar Codex poate termina treaba și scrie DONE *înainte* ca daemon-ul să timeout-eze runner-ul. Pattern:
1. Codex termină la 1750s → scrie DONE în delivery
2. Runner-ul nu se oprește imediat (cleanup)
3. La 1800s daemon-ul kill-uiește runner-ul → scrie ERROR

**Fix pe termen lung**: Daemon-ul ar trebui să verifice dacă task-ul are DONE în delivery ÎNAINTE de a scrie ERROR. Brief Codex pentru asta.

## Enforcement Loop (META-H-002)

### WHERE
- La fiecare Codex delivery (codex-to-genie.md scan) - check for ERROR status
- Codex daemon cycle - 90s interval

### WHEN
- Automatic: fiecare delivery scan
- Manual: la audit sesiune

### HOW
- ERROR delivery necitita >1h -> violation
- ERROR fara retry/brief -> violation
- Runner: pending-delivery.flag + codex-to-genie.md scan

### CONNECT
- OPS-H-002 -> error detection rule
- DEV-H-010 -> audit gate
- codex-daemon.sh -> execution engine
- procedure-health.json -> monitoring
