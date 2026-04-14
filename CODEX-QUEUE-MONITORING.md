---
type: procedure
created: 2026-03-17
status: active
slug: codex-queue-monitoring
tags: [procedure, nexus]
---

# Codex Queue Monitoring — Procedura de Așteptare Activă

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: OPS-S-003
**Scope**: Cum gestionează Genie timpul între trimiterea task-urilor la Codex și primirea livrărilor

---

## 1. Problema

După trimiterea brief-urilor la Codex daemon (90s cycle), Genie trebuie să aștepte livrările. Fără procedură:
- Genie intră în sleep-poll loops (sleep 90s → check → sleep 120s → check) — blocând sesiunea
- Userul nu poate interacționa câte 5-10 min
- Tokens consumate pe polls goale fără valoare
- Task-uri Codex se termină mai repede decât estimat, dar Genie nu observă

Situații acoperite:
- 1-3 task-uri trimise — completion estimat <10 min
- 4-10 task-uri trimise — completion estimat 15-60 min
- Task-uri cu dependențe (m4-X depinde de m4-Y)
- User vrea să facă altceva între timp

---

## 2. Procedura

```
SUBMIT briefs → REPORT status → ASK user → WORK while waiting → CHECK on demand → AUDIT deliveries
```

### Pas 1: SUBMIT — Trimite brief-uri la Codex

Scrie în `~/.codex/genie-to-codex.md` cu format standard (Status: PENDING).
Notează: câte task-uri, ce modele, estimări de timp.

### Pas 2: REPORT — Anunță userul

Imediat după submit:
```
"N task-uri trimise la Codex (m4-XXX..m4-YYY). Daemon-ul le procesează secvențial (~90s/cycle).
Estimare: [scurt <10min | mediu 10-30min | lung >30min].
Vrei să așteptăm sau facem altceva?"
```

### Pas 3: ASK — Lasă userul să decidă

| User zice | Acțiune |
|-----------|---------|
| "Așteptăm" | → Pas 4 (work while waiting) |
| "Fă altceva" | → Treci la task-ul cerut, check Codex la cerere |
| "Verifică" | → Pas 5 (single check) |
| Nu răspunde | → Pas 4 implicit (work while waiting) |

### Pas 4: WORK — Lucrează între timp (NICIODATĂ idle)

Activități utile în așteptare:
1. Auditează task-uri deja completate (DEV-H-010)
2. Salvează skills din sesiune (MEM-H-002)
3. Răspunde la mesaje queue-uite de la user
4. Verifică documentație, cross-references
5. Updatează MEMORY.md / task files
6. Pregătește brief-uri pentru task-uri viitoare

**INTERDICȚII**:
- NU `sleep` > 30s inline
- NU loop de poll (sleep → check → sleep → check)
- NU bloca conversația cu "aștept..."

### Pas 5: CHECK — Verificare on-demand (max 1 per 5 min)

O singură comandă:
```bash
for t in m4-XXX m4-YYY; do
  grep -c "$t.*DONE" ~/.codex/codex-to-genie.md >/dev/null 2>&1 && echo "$t: DONE" || echo "$t: PENDING"
done
```

Dacă toate DONE → Pas 6.
Dacă nu → raportează progresul, continuă Pas 4.

### Pas 6: AUDIT — DEV-H-010 pe fiecare livrare

Per task completat:
1. Citește delivery report din `codex-to-genie.md`
2. Verifică fișiere create/modificate (exist? syntax OK?)
3. Verifică LaunchAgents (loaded?)
4. Spot-check conținut (head, grep key sections)
5. Raportează: PASS / PASS with notes / FAIL

---

## 3. Cortex Logging

```json
{
  "text": "[CODEX-QUEUE] Session {date}: {N} tasks submitted, {M} completed, {K} audited. Wait method: {active|passive}. User blocked: {yes|no}.",
  "collection": "procedures",
  "metadata": {
    "type": "session-log",
    "procedure": "CODEX-QUEUE-MONITORING",
    "rule_id": "OPS-S-003",
    "tasks_submitted": "{N}",
    "tasks_completed": "{M}",
    "wait_method": "active|passive",
    "tags": ["codex", "queue", "monitoring"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- La fiecare sesiune unde Genie trimite task-uri la Codex
- WISH Step H (Hand-off) → Codex queue

### WHEN
- Imediat după trimiterea oricărui brief la `genie-to-codex.md`
- La fiecare check de progres

### HOW (violation detection)
- `sleep` > 30s inline în așteptare Codex → violation
- Loop de poll (2+ checks fără muncă utilă între ele) → violation
- Sesiune blocată >5 min fără output util → violation
- User nu poate interacționa din cauza polling → violation
- Runner: self-check la fiecare sesiune, nightly-audit review

### CONNECT
- OPS-S-003 → regula principală
- DEV-H-010 → audit gate pe livrări
- DEV-H-011 → Codex signal check
- MEM-H-002 → skill saving în timpul așteptării
- `codex-daemon.sh` → daemon-ul care procesează queue
- `procedure-health.json` → entry pentru monitoring

---

## 5. Dependențe

| Componentă | Rol | Path |
|-----------|-----|------|
| codex-daemon.sh | Procesează queue | `~/.openclaw/scripts/codex-daemon.sh` |
| genie-to-codex.md | Queue file | `~/.codex/genie-to-codex.md` |
| codex-to-genie.md | Delivery file | `~/.codex/codex-to-genie.md` |
| pending-delivery.flag | Signal file | `~/.codex/pending-delivery.flag` |
| telegram-notify.sh | Notificări urgente | `~/.openclaw/scripts/telegram-notify.sh` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Session block time | Min blocate de polling | 0 min |
| Useful work during wait | Tasks done while waiting | >1 per wait |
| Poll frequency | Checks per 5 min | ≤1 |
| Audit coverage | % deliveries auditate | 100% |
| User interrupts | Câte ori userul a întrerupt polling | 0 |

---

## Checklist Pre-Publicare

- [x] Regulă asociată: OPS-S-003
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT
- [x] FORGE compliant: §1-§7
- [x] Incident real documentat (2026-02-27: 10+ min blocat în sleep loops)
- [x] Salvat în Cortex (add349de)
- [x] Entry în procedure-health.json
