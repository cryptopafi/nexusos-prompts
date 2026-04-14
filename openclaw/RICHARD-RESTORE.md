---
type: procedure
created: 2026-03-17
status: active
slug: richard-restore
tags: [procedure, nexus]
---

# Richard Orchestrator Restore (Genie Guardian) — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: SEC-H-006 (Richard Autonomy — Genie as Guardian), COM-H-005 (Richard = Prioritate Absolută)
**Scope**: Detectarea și restaurarea orchestratorului Richard când este down, neresponsiv sau în stare degradată.

---

## 1. Problema

Richard (agent `main`, Opus 4.6) este orchestratorul central al OpenClaw. Dacă Richard e down:
- Niciun task nu mai poate fi delegat către specialiști (Finance, Marketing, Legal, Ops, Insights)
- Mesajele din #hub nu primesc răspuns
- Workflow-urile în curs rămân blocate
- Sistemul pare funcțional (GW1 e sus) dar este inoperant

Genie este guardian-ul lui Richard (SEC-H-006): responsabilitatea de a detecta și restaura Richard revine Genie, nu utilizatorului.

Situații acoperite:
- Richard nu răspunde la mesaje în #hub
- Richard răspunde dar nu deleghează (comportament anormal)
- Richard a intrat în loop sau produce output degradat
- Richard a consumat tot contextul și are nevoie de restart cu state restoration
- Crash al procesului agent `main`

---

## 2. Procedura

### Pas 1: Detectare — Confirmă că Richard e down
Nu acționa pe baza unui singur semn. Verifică minim 2 din 3:
- Richard nu răspunde la ping în #hub în 30 secunde
- Dashboard GW1 arată agent `main` ca inactive/error
- Ultimul mesaj din Richard e mai vechi de pragul normal de activitate

### Pas 2: Diagnosticare — Identifică cauza
Înainte de restart, înțelege de ce:
- **Context exhausted**: Richard a atins limita contextului — restart normal, memoria din MEMORY.md se va reîncărca
- **Process crash**: procesul a murit → restart GW1 sau doar agentul `main`
- **Loop detectat**: Richard produce output repetat → force terminate + restart
- **Gateway issue**: GW1 însuși e problematic → escalat la OPENCLAW-STARTUP.md full restart

### Pas 3: Salvare State Înainte de Restore
Dacă Richard e parțial funcțional (răspunde dar degradat):
- Salvează MEMORY.md și daily log-ul curent
- Notează ce taskuri erau în curs (din #handoff sau #ops-logs)
- Copiează ultima sesiune JSONL ca backup

### Pas 4: Restore Richard
În funcție de cauza identificată la Pas 2:

**A) Context exhausted / normal restart**:
- OpenClaw va triggers automat compaction → Richard salvează în MEMORY.md → restart session
- Genie poate forța: trimite mesaj de trigger compaction dacă mecanismul automat nu a funcționat

**B) Process crash**:
- Restart agent `main` prin GW1 control port (18791) sau restart GW1 complet
- Verifică că workspace-ul `rich-new` e intact înainte de restart

**C) Loop / comportament aberant**:
- Force terminate sesiunea curentă a lui Richard
- **Notă**: în stare de loop, flush-ul MEMORY.md poate fi imposibil (Richard nu procesează instrucțiuni). Acceptă pierderea memoriei din sesiunea curentă — restaurarea se face din MEMORY.md existent + daily log backup (Pas 3). Nu bloca force terminate așteptând un flush care nu va veni.
- Restart fresh cu MEMORY.md intact
- Nu injecta prompt-uri noi până Richard confirmă că e stabil

**D) Gateway issue**:
- Execută OPENCLAW-STARTUP.md de la capăt

### Pas 5: Verificare Post-Restore
- Ping Richard în #hub → trebuie să răspundă în 30 secunde
- Testează delegarea: cere Richard să trimită un mesaj simplu la Insights
- Confirmă că MEMORY.md a fost încărcat corect (Richard menționează context din memorie)

### Pas 6: Notificare
- Telegram: "Richard restaurat, cauza: [X], downtime: [Y] minute"
- Loghează incident în `~/.openclaw/richard-incidents.log`
- Dacă downtime > 5 minute → salvează în Cortex ca incident

### Pas 7: Root Cause — Previne Recurrence
- Dacă cauza e recurentă → creează task pentru Codex să investigheze
- Dacă e context exhaustion frecventă → Richard are nevoie de MEMORY.md trim sau workflow redesign
- Adaugă la LEARNINGS.md dacă pattern nou

---

## 3. Cortex Logging

```json
{
  "text": "Richard restaurat. Cauza: [context_exhausted|crash|loop|gateway]. Downtime: X min. Metoda: restart/force-terminate/gateway-restart.",
  "collection": "procedures",
  "metadata": {
    "type": "incident_log",
    "procedure": "RICHARD-RESTORE",
    "rule_id": "SEC-H-006",
    "tags": ["openclaw", "richard", "restore", "guardian", "incident"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Session Start Checklist (Genie verifică dacă Richard e alive la session start dacă OpenClaw e activ)
- Genie Supervisor LaunchAgent (monitorizare continuă când sistemul rulează)
- Orice moment în care Genie detectează că Richard nu răspunde

### WHEN
- Automat: când Genie Guardian detectează Richard neresponsiv
- Manual: când Pafi raportează că Richard nu răspunde

### HOW (violation detection)
- Genie observă Richard down și NU execută această procedură → violation SEC-H-006
- Genie restartează Richard fără a salva state-ul → violation MEM-H-002
- Genie nu loghează incidentul în Cortex → violation MEM-H-001
- Runner: Genie supervizor (conștiință proprie) + LaunchAgent daemon de monitoring

### CONNECT
- `SEC-H-006` → regula sursa — Richard autonomy, Genie guardian
- `COM-H-005` → Richard prioritate absolută = justifică urgența acestei proceduri
- `OPENCLAW-STARTUP.md` → apelat dacă cauza e la nivelul Gateway-ului
- `AGENT-RESTART-STATE-PRESERVATION.md` → apelat la Pasul 4B/4C pentru restore cu state
- `procedure-health.json` → adaugă entry `"RICHARD-RESTORE": "active"`

### VERIFY
La finalul execuției, verifică:
- [ ] Richard răspunde activ în #hub?
- [ ] Incidentul a fost logat (richard-incidents.log + Cortex)?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] RICHARD-RESTORE | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Richard Orchestrator Restore" | FORGE ✓ | rule: SEC-H-006 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Detectare + diagnostic rapid | Genie (Sonnet) | Fast, orchestrare |
| Diagnosticare cauza complexă (loop, drift) | Opus subagent | Reasoning profund necesar |
| Execuție restart tehnic | Genie direct | Acțiune simplă, no code needed |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Richard agent `main` | Orchestratorul de restaurat | `~/.openclaw/workspaces/rich-new/` |
| GW1 control port | Restart agent | `localhost:18791` |
| `MEMORY.md` Richard | State de restaurat | `~/.openclaw/workspaces/rich-new/MEMORY.md` |
| `richard-incidents.log` | Audit trail | `~/.openclaw/richard-incidents.log` |
| OPENCLAW-STARTUP.md | Fallback full restart | `~/.nexus/procedures/openclaw/OPENCLAW-STARTUP.md` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Mean Time to Restore (MTTR) | Downtime Richard de la detectare la restaurare | < 3 minute |
| Incidente per săptămână | Frecvența caderii Richard | < 1/săptămână |
| State recovery rate | % restaurări cu memorie intactă | 100% |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-006, COM-H-005)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata FORGE
