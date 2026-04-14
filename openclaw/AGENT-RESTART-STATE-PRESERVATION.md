---
type: procedure
created: 2026-03-17
status: active
slug: agent-restart-state-preservation
tags: [procedure, nexus]
---

# Agent Restart with State Preservation — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: MEM-H-002 (Save-As-You-Go), OPS-H-001 (Technical Autonomy)
**Scope**: Restartul oricărui agent OpenClaw (Richard sau specialist) cu păstrarea completă a memoriei și stării anterioare.

---

## 1. Problema

Un restart al unui agent fără preservarea stării duce la:
- Pierdere de memorie din sesiunea curentă (daily log neflushuit)
- Agent care "uită" taskurile în curs
- Context window resetat → agent reconfigurat de la zero din MEMORY.md (bine) dar pierde ce nu s-a scris încă acolo (rău)
- Alte agenți care așteptau un handoff rămân blocați

Situații acoperite:
- Restart unui agent care a exhausted context window
- Restart după eroare de tool sau timeout
- Restart forțat pentru modificare config/SOUL.md
- Restart după update de model
- Restart ca parte din RICHARD-RESTORE.md sau OPENCLAW-STARTUP.md

---

## 2. Procedura

### Pas 1: Identifică Agentul și Starea Curentă
Înainte de restart, colectează:
- **agentId**: `main` | `finance` | `marketing` | `legal` | `ops` | `insights`
- **Workspace path**: `~/.openclaw/workspaces/<agent>/`
- **Task curent**: verifică #handoff și #ops-logs pentru taskuri în progres
- **Sesiune activă**: există JSONL deschis în `~/.openclaw/agents/<agentId>/sessions/`?

### Pas 2: Flush Memory (înainte de restart)
Salvează tot ce nu e încă în MEMORY.md:
- Dacă agentul e parțial responsiv: trimite instrucțiunea "write current state to MEMORY.md now"
- Dacă agentul e complet mort: verifică daily log `memory/YYYY-MM-DD.md` — e up to date?
- Dacă daily log e incomplet: accepta pierderea minimă și continuă

### Pas 3: Snapshot Sesiune Curentă
Backup al sesiunii JSONL active:
- Copiază `~/.openclaw/agents/<agentId>/sessions/main.json` → `main.json.bak-{timestamp}`
- Acest snapshot e folosit dacă vrei să auditi ce s-a pierdut

### Pas 4: Notifică Agenții Dependenți
Dacă alți agenți așteptau output de la agentul care se restartează:
- Trimite mesaj în #handoff: "agent X restart iminent, taskul Y suspendat temporar"
- Richard trebuie notificat întotdeauna (indiferent de care agent se restartează)

### Pas 5: Execuție Restart
Restartul propriu-zis:
- **Metodă preferată**: restart prin GW1 control port (18791) — graceful session end + reload
- **Dacă nu merge**: stop agent → verifică workspace → start agent din nou cu profilul său
- **Fallback**: restart complet GW1 (include toți agenții) — folosit doar dacă agent individual nu poate fi restartat

**⚠ Blast radius warning — restart GW1**: Oprirea GW1 afectează TOȚI cei 6 agenți simultan (Richard + Finance + Marketing + Legal + Ops + Insights). Toate sesiunile active sunt terminate. Folosește fallback GW1 DOAR dacă metodele individuale au eșuat complet.
**Escalation dacă GW1 restart eșuează**: → execută OPENCLAW-STARTUP.md de la zero (full restart) → dacă și acela eșuează → escalat la Pafi, sistemul necesită intervenție manuală

### Pas 6: Verificare Post-Restart
Confirmă că starea a fost restaurată:
- Agentul a încărcat MEMORY.md (trimite un mesaj și verifică că răspunde cu context din memorie)
- SOUL.md a fost reîncărcat (agentul se comportă conform personalității definite)
- Agentul poate lua taskuri noi (delegate de Richard sau direct)

### Pas 7: Resume Taskuri Suspendate
Dacă erau taskuri în curs la momentul restartului:
- Informează agentul despre taskul suspendat (cu contextul relevant din Cortex dacă disponibil)
- Agentul poate relua din snapshot-ul menționat în notificarea din Pas 4

---

## 3. Cortex Logging

```json
{
  "text": "Agent [agentId] restartat cu state preservation. Memoria intactă: [DA/NU]. Task suspendat: [task sau none]. Cauza restartului: [motiv].",
  "collection": "procedures",
  "metadata": {
    "type": "execution_log",
    "procedure": "AGENT-RESTART-STATE-PRESERVATION",
    "rule_id": "MEM-H-002",
    "tags": ["openclaw", "agent", "restart", "state", "memory"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Orice restart de agent în OpenClaw — indiferent de cauză
- Apelat explicit din RICHARD-RESTORE.md (Pasul 4B/4C)
- Apelat din OPENCLAW-STARTUP.md dacă un agent nu pornește corect

### WHEN
- La orice restart individual de agent
- Obligatoriu înainte de modificarea SOUL.md unui agent activ

### HOW (violation detection)
- Agent restartat fără flush de MEMORY.md → violation MEM-H-002
- Restart fără notificarea Richard → violation COM-H-005
- Restart fără snapshot sesiune JSONL → risc MEM-H-001 (potențiala pierdere de date)
- Runner: Genie verifică după fiecare restart că agentul a încărcat MEMORY.md

### CONNECT
- `MEM-H-002` → flush memory = save-as-you-go aplicat la restart
- `COM-H-005` → Richard notificat la Pasul 4
- `RICHARD-RESTORE.md` → apelant pentru cazul specific Richard
- `OPENCLAW-STARTUP.md` → fallback dacă restart individual eșuează
- `procedure-health.json` → adaugă entry `"AGENT-RESTART-STATE-PRESERVATION": "active"`

### VERIFY
La finalul execuției, verifică:
- [ ] Agentul a încărcat MEMORY.md și răspunde cu context?
- [ ] Taskurile suspendate au fost notificate și pot fi reluate?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] AGENT-RESTART-STATE-PRESERVATION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Agent Restart State Preservation" | FORGE ✓ | rule: MEM-H-002 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție procedură | Genie (Sonnet) | Orchestrare pași clari |
| Investigare de ce agentul pică repetat | Opus subagent | Pattern analysis |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Agent workspace | Memorie de restaurat | `~/.openclaw/workspaces/<agent>/` |
| MEMORY.md | Long-term memory | `<workspace>/MEMORY.md` |
| Sessions JSONL | Short-term state | `~/.openclaw/agents/<agentId>/sessions/main.json` |
| GW1 control port | Restart graceful | `localhost:18791` |
| RICHARD-RESTORE.md | Procedură specializată Richard | `~/.nexus/procedures/openclaw/RICHARD-RESTORE.md` |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (MEM-H-002, OPS-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata FORGE
