---
type: procedure
created: 2026-03-17
status: active
slug: openclaw-shutdown
tags: [procedure, nexus]
---

# OpenClaw Graceful Shutdown — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.2
**Regulă asociată**: OPS-H-001 (Technical Autonomy), MEM-H-002 (Save-As-You-Go), COM-H-005 (Richard = Prioritate Absolută)
**Scope**: Oprirea ordonată a OpenClaw pe MacIntel cu preservarea completă a stării agenților.

---

## 1. Problema

**Context**: OpenClaw rulează 6 agenți persistenți pe MacIntel cu stare distribuită în SQLite + JSONL + MEMORY.md. Un shutdown brusc distruge starea nesalvată și necesită recovery manual la restart — riscul principal este Richard abandonat în mijlocul unui workflow critic fără checkpoint.

Un shutdown brusc al OpenClaw (kill -9, power off, crash) poate cauza:
- Pierdere de date din MEMORY.md (dacă nu s-a salvat înainte de compaction)
- Sesiuni JSONL incomplete sau corupte
- SQLite WAL file neflushuit → index inconsistent la restart
- Tasks în curs de execuție abandonate fără notificare
- Richard în mijlocul unui workflow → stare nedefinită la restart

**Situații concrete acoperite**:
- **Shutdown planificat** (seară, weekend): timp suficient pentru Pas 1–8 complet (~2–3 min)
- **Shutdown înainte de update** OpenClaw/MacIntel: obligatoriu — update-ul poate suprascrie procesele active
- **Shutdown pentru troubleshooting**: folosit când GW1/GW2 sunt blocate sau unresponsive
- **Shutdown de urgență** (forțat de sistem, kernel panic, atac): skip la Pas 1–2, intră direct pe emergency path

---

## 2. Procedura

> **Emergency shutdown shortcut**: Dacă sistemul e instabil (crash iminent, loop, atac) și Pas 1-2 nu pot fi executate — oprește direct GW1 (SIGTERM pe procesul `openclaw`) → GW2 → loghează incident. Pierderea de memorie e acceptabilă în situații de urgență. Documentează în `~/.openclaw/shutdown.log` cu flag `EMERGENCY`.

### Pas 1: Notifică Richard
Înainte de orice altceva, anunță Richard că sistemul se oprește:
- Deschide conversația cu Richard în OpenClaw UI sau trimite prin CLI:
  ```
  openclaw send --agent richard --channel hub "shutdown iminent în 3 minute — suspendă taskurile active"
  ```
- Alternativ din dashboard: Richard → #hub → mesaj manual cu textul de mai sus
- Richard trebuie să confirme cu "acknowledged" sau să suspende taskurile active
- Agenții care au taskuri în curs vor primi mesaj de abort din partea lui Richard
- **Nu continua la Pas 2 fără confirmarea lui Richard** (sau timeout 60 secunde)

### Pas 2: Flush MEMORY.md per Agent
Triggherează salvarea forțată a memoriei pentru fiecare agent:
- Trimite instrucțiunea de flush la fiecare agent activ:
  ```
  openclaw send --agent <agentId> "FLUSH_MEMORY — scrie starea curentă în MEMORY.md acum"
  ```
  Repetă pentru fiecare agent activ (Richard, Genie, și ceilalți 4).
- Verifică că fiecare agent a confirmat scrierea (răspuns "MEMORY flushed" sau echivalent)
- Alternativ rapid: verifică timestamp-ul ultimei scrieri în daily log:
  ```bash
  ls -la ~/.openclaw/memory/$(date +%Y-%m-%d).md
  ```
  Dacă timestamp e < 5 minute → memoria e fresh, ok să continui

**Fallback Pas 2 (agent nu confirmă):**
- Agent neresponsiv la instrucțiunea flush: verifică `memory/YYYY-MM-DD.md` manual — dacă ultimul entry e < 5 minute, memoria e fresh → ok
- Dacă daily log e mai vechi de 5 minute: acceptă pierdere minimă, documentează în incident log cu timestamp
- Nu bloca shutdown-ul pentru un agent neresponsiv — continuă la Pas 3 cu nota de pierdere acceptată

### Pas 3: Închide Sesiunile Active
Permite sesiunilor JSONL să se finalizeze curat:
- Nu forța kill pe procese cu sesiuni active
- Așteptă confirmare că sessions sunt în stare `completed` sau `idle`
- Timeout: dacă o sesiune nu se închide în 30 secunde → kill forțat + log incident

### Pas 4: Oprește Gateway 1 (port 18789)
Oprește GW1 prin metoda oficială (SIGTERM — NU SIGKILL):
```bash
# Metoda 1: SIGTERM graceful
kill -15 $(pgrep -f "openclaw.*gw1") 2>/dev/null || kill -15 $(lsof -ti:18789)

# Sau prin dashboard: OpenClaw → Gateway Manager → GW1 → Stop

# Verifică că porturile sunt libere:
lsof -i:18789 -i:18791 -i:18792
# Rezultat așteptat: niciun output (porturi libere)
```
- Nu forța SIGKILL decât dacă SIGTERM nu funcționează în 15 secunde (risc de corupție)

### Pas 5: Oprește Gateway 2 (port 19001) — Watchdog
Oprește GW2 DUPĂ GW1 (watchdog nu mai are ce monitoriza):
```bash
kill -15 $(pgrep -f "openclaw.*gw2") 2>/dev/null || kill -15 $(lsof -ti:19001)

# Verifică porturile libere:
lsof -i:19001 -i:19003 -i:19004
# Rezultat așteptat: niciun output
```

### Pas 6: Verifică Integritatea SQLite
Confirmă că WAL file-urile s-au flushuit:
```bash
# Verifică existența WAL files negoale:
ls -la ~/.openclaw/memory/*.sqlite-wal 2>/dev/null

# Dacă există WAL neflushuit → rulează checkpoint manual:
sqlite3 ~/.openclaw/memory/<agentId>.sqlite "PRAGMA wal_checkpoint(TRUNCATE);"

# Verificare finală — toate WAL trebuie să fie 0 bytes sau absente:
find ~/.openclaw/memory/ -name "*.sqlite-wal" -size +0c
# Rezultat așteptat: niciun output
```

### Pas 7: Închide SSH Tunnel Cortex
Oprește tunelul `localhost:6400` dacă nu mai e nevoie:
```bash
# Dacă tunelul e manual (nu LaunchAgent):
kill $(lsof -ti:6400) 2>/dev/null && echo "Tunel SSH Cortex închis"

# Dacă rulează ca LaunchAgent → poate rămâne activ, skip:
launchctl list | grep cortex-tunnel
# Dacă apare în output → lăsa-l activ (ok)
```

### Pas 8: Log + Telegram
```bash
# Loghează timestamp în shutdown.log:
echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) GRACEFUL shutdown complet — GW1+GW2 oprite, WAL clean" >> ~/.openclaw/shutdown.log
```
- Notifică prin @Claudeautomationbot (MacM4): "OpenClaw shutdown complet, sistem oprit"
- Mesajul confirmă că sistemul e safe de oprit/rebooted

---

## 3. Cortex Logging

```json
{
  "text": "OpenClaw shutdown graceful executat. GW1+GW2 oprite. Memory flush confirmat. SQLite WAL clean.",
  "collection": "procedures",
  "metadata": {
    "type": "execution_log",
    "procedure": "OPENCLAW-SHUTDOWN",
    "rule_id": "OPS-H-001",
    "tags": ["openclaw", "shutdown", "operational", "graceful"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (Flux A) înainte de orice acțiune destructivă pe MacIntel (update, reboot)
- Manual când Pafi decide să oprească sistemul

### WHEN
- La orice shutdown planificat al OpenClaw
- Obligatoriu înaintea reboot-ului MacIntel
- Obligatoriu înaintea update-ului OpenClaw

### HOW (violation detection)
- MacIntel reboot fără a rula această procedură → violation OPS-H-001
- MEMORY.md neflushuit înainte de oprire → potențială pierdere de date → violation MEM-H-002
- GW1 oprit fără notificarea Richard → violation COM-H-005
- Runner: Genie verifică la Session Start dacă shutdown-ul anterior a fost graceful (verifică shutdown.log timestamp)

### CONNECT
- `MEM-H-002` → flush MEMORY.md = Save-As-You-Go aplicat la shutdown
- `COM-H-005` → Richard notificat obligatoriu (Pas 1)
- `OPENCLAW-STARTUP.md` → perechea directă a acestei proceduri
- `procedure-health.json` → adaugă entry `"OPENCLAW-SHUTDOWN": "active"`

### VERIFY
La finalul execuției, verifică toate cele 8 checkpointuri:
- [ ] Richard notificat și taskurile active suspendate (Pas 1)?
- [ ] MEMORY.md flushed pentru toți agenții activi (Pas 2)?
- [ ] Sesiunile JSONL în stare `completed` sau `idle` (Pas 3)?
- [ ] Porturile 18789, 18791, 18792 libere — GW1 oprit (Pas 4)?
- [ ] Porturile 19001, 19003, 19004 libere — GW2 oprit (Pas 5)?
- [ ] SQLite WAL files goale sau absente (Pas 6)?
- [ ] SSH Tunnel Cortex gestionat (Pas 7)?
- [ ] Entry scris în `~/.openclaw/shutdown.log` + Telegram notificat (Pas 8)?
- [ ] Dacă oricare = NU → procedura NU e completă, documentează în shutdown.log cu flag `INCOMPLETE`

**VK-uri obligatorii**:
1. `✅ [PROC] OPENCLAW-SHUTDOWN | §1✓ §2✓ §3✓ §4✓ §5✓ §6✓ §7✓ §8✓ VER✓ | complete`
2. `✅ [CORTEX] "OpenClaw Graceful Shutdown" | FORGE ✓ | rule: OPS-H-001 | v1.2`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție shutdown | Genie (Sonnet) | Orchestrare pași secvențiali |
| Diagnosticare WAL neflushuit | Opus subagent | Risc de corupție SQLite |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| OpenClaw GW1 | Proces de oprit | port 18789 (porturi secundare: 18791, 18792) |
| OpenClaw GW2 | Watchdog de oprit | port 19001 (porturi secundare: 19003, 19004) |
| SQLite WAL files | Integritate index | `~/.openclaw/memory/*.sqlite-wal` |
| Daily memory log | Verificare flush agent | `~/.openclaw/memory/YYYY-MM-DD.md` |
| `shutdown.log` | Audit trail + metrics source | `~/.openclaw/shutdown.log` |
| @Claudeautomationbot | Notificare finală | MacM4 relay (`com.claude.telegram-relay`) |
| SSH Tunnel Cortex | Index vector Cortex | `localhost:6400` → VPS 89.116.229.189 |
| `OPENCLAW-STARTUP.md` | Procedura pereche (restart) | `~/.nexus/procedures/openclaw/OPENCLAW-STARTUP.md` |
| `procedure-health.json` | Registry proceduri active | `~/.nexus/procedures/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Cum se măsoară | Review |
|---------|-----------|--------|----------------|--------|
| Time to graceful shutdown | De la Pas 1 la Pas 8 complet | < 3 minute (normal) | Diff timestamp Pas 1 vs Pas 8 în `shutdown.log` | La fiecare execuție |
| Memory flush success rate | % agenți cu MEMORY.md flushed la shutdown | > 95% | Count confirmări flush / count agenți activi din `shutdown.log` | Săptămânal |
| Emergency shutdowns / lună | Frecvența bypass-ului Pas 1-2 | < 2 / lună | Count linii cu flag `EMERGENCY` în `shutdown.log` (`grep EMERGENCY ~/.openclaw/shutdown.log | wc -l`) | Lunar, prima zi din lună |
| WAL checkpoint failures | % shutdown-uri cu WAL neflushuit la Pas 6 | 0% | Count linii cu `WAL_FAIL` în `shutdown.log` | La fiecare execuție |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, MEM-H-002, COM-H-005)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu toate cele 8 pași acoperite
- [x] Pași executabili cu comenzi concrete (nu doar descriere)
- [x] VK format specificat (v1.2)
- [x] Metrics cu target-uri specifice + metodă de măsurare + frecvență review
- [x] Dependencies complete cu path-uri exacte (9 componente)
- [x] Changelog prezent
- [ ] Entry adăugat în `procedure-health.json` (acțiune runtime, nu pre-publicare)
- [ ] Salvat în Cortex cu metadata FORGE (acțiune runtime, executat la prima rulare)

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|------------|
| 1.0 | 2026-03-03 | Creare inițială procedură |
| 1.1 | 2026-03-03 | Adăugat emergency shortcut, fallback Pas 2, model routing |
| 1.2 | 2026-03-04 | FORGE-AUDIT: comenzi concrete la Pas 1–8, VERIFY extins (8 checkpointuri), dependencies completate (9 componente), metrics cu metodă de măsurare, D1 context adăugat, changelog creat |
