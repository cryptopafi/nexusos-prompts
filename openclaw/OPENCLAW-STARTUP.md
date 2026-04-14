---
type: procedure
created: 2026-03-17
status: active
slug: openclaw-startup
tags: [procedure, nexus]
---

# OpenClaw Daily Startup Sequence — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: OPS-H-001 (Technical Autonomy), COM-H-005 (Richard = Prioritate Absolută)
**Scope**: Pornirea ordonată și verificată a sistemului OpenClaw pe MacIntel — Gateway 1, Gateway 2, toți agenții.

---

## 1. Problema

Fără o secvență de startup documentată, fiecare restart al OpenClaw depinde de cunoștințe tribale. Riscuri:
- Agenți porniți fără workspace inițializat → memoria incompletă
- GW1 pornit fără GW2 watchdog → nimeni nu detectează dacă GW1 cade
- Richard pornit fără fallback chain verificat → orchestrare nefuncțională
- SSH tunnel Cortex uitat → agenții nu pot accesa knowledge base

Situații acoperite:
- Startup după shutdown planificat (seară → dimineață)
- Startup după crash neașteptat al MacIntel
- Startup după update OpenClaw
- Startup inițial după fresh install

---

## 2. Procedura

### Pas 1: Pre-flight Check (MacIntel)
Verifică că MacIntel este disponibil și procesele vechi sunt oprite:
- Confirma că nu rulează instanțe OpenClaw din sesiune anterioară (port 18789 și 19001 libere)
- Verifică că Tailscale VPN este conectat (necesar pentru SSH tunnel VPS)
- Confirma spațiu disc disponibil (minim 1GB liber pentru logs + sessions)

**Error recovery Pas 1:**
- Port 18789 ocupat: identifică procesul (`lsof -i :18789`), oprește-l dacă e o instanță veche; dacă e alt proces → escalat la Pafi
- Port 19001 ocupat: același flow ca 18789
- Tailscale deconectat: reconectează (`tailscale up`) și re-verifică înainte de a continua
- Disc insuficient: curăță logs vechi din `~/.openclaw/logs/` (> 30 zile) și re-verifică
- Referință: `PORT-18789-LOCALHOST-ENFORCEMENT.md` pentru detalii complete audit porturi

### Pas 2: SSH Tunnel Cortex
Deschide tunelul persistent către VPS pentru accesul agenților la Cortex:
- Tunnel: `localhost:6400` → `VPS:6400` (Qdrant + Cortex API)
- Verifică că tunelul răspunde: Cortex health check trebuie să returneze OK
- Dacă tunelul e deja activ din LaunchAgent — confirmă că e funcțional

**Error recovery Pas 2:**
- Tunnel nu răspunde: re-pornește tunelul manual (SSH `pafi@89.116.229.189 -L 6400:localhost:6400`)
- SSH connection refused: verifică că VPS e up (ping/SSH direct), că Qdrant rulează pe VPS (`systemctl status qdrant`)
- Timeout după 3 retry-uri (15 secunde fiecare): loghează ca warning și continuă fără Cortex — agenții vor opera în fallback mode (fără `cortex_search`)
- LaunchAgent expirat sau eșuat: restart LaunchAgent (`launchctl kickstart -k gui/$UID/com.cortex.tunnel`)

### Pas 3: Pornire Gateway 1 (port 18789)
Lansează Gateway-ul principal cu profilul de producție:
- Profil: `research` (sau `main` — conf. `~/.openclaw/openclaw.json`)
- Port: 18789 (data), 18791 (control), 18792 (CDP)
- Confirmă că toți agenții bound sunt vizibili în dashboard: Richard, Finance, Marketing, Legal, Ops, Insights

**Error recovery Pas 3:**
- GW1 nu pornește (config error): verifică `~/.openclaw/openclaw.json` pentru syntax errors; rollback la ultima versiune funcțională din git
- Un agent din 6 lipsește din dashboard: verifică workspace-ul agentului (Pas 4 avansat); restart agent individual prin control port 18791
- GW1 pornește dar crashes imediat: verifică logs `~/.openclaw/logs/gw1-*.log` pentru eroare specifică; escalat la Opus subagent dacă cauza nu e evidentă în 2 minute

### Pas 4: Verificare per-agent Workspace
Pentru fiecare agent din GW1, confirmă că:
- Workspace-ul există și MEMORY.md e accesibil
- Ultima sesiune JSONL din `~/.openclaw/agents/<agentId>/sessions/` e intactă
- SQLite index (`~/.openclaw/memory/<agentId>.sqlite`) e prezent și necorupt

### Pas 5: Pornire Gateway 2 (port 19001) — Watchdog
Lansează GW2 pe profil `watchdog`:
- Port: 19001 (base), 19003 (control), 19004 (CDP)
- Confirmă că watchdog poate accesa GW1 health endpoint
- GW2 trebuie pornit DUPĂ GW1 (verifică GW1 în viață)

### Pas 6: Ping Richard
Trimite un mesaj de test lui Richard prin #hub:
- Mesaj: "startup check" sau similar
- Așteptați confirmare că Richard răspunde și poate delega
- Dacă Richard nu răspunde în 30 secunde → execută RICHARD-RESTORE.md

### Pas 7: Notificare Telegram
Trimite confirmare de startup prin @Claudeautomationbot:
- Conținut: sistem pornit, număr agenți activi, status Cortex tunnel, timestamp

---

## 3. Cortex Logging

```json
{
  "text": "OpenClaw startup completat: GW1 (18789) + GW2 (19001) active. 6 agenți online. Cortex tunnel OK.",
  "collection": "procedures",
  "metadata": {
    "type": "execution_log",
    "procedure": "OPENCLAW-STARTUP",
    "rule_id": "OPS-H-001",
    "tags": ["openclaw", "startup", "operational", "gateway"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Session Start Checklist Step 6 (dacă OpenClaw trebuie să fie activ pentru sesiune)
- WISH Step H (Flux A) când Pafi cere taskuri care necesită OpenClaw

### WHEN
- La orice pornire a OpenClaw — voluntară sau după crash
- Zilnic dimineața dacă OpenClaw e folosit în acea zi

### HOW (violation detection)
- GW1 pornit fără verificarea workspace-urilor per agent → violation
- GW2 pornit înaintea sau fără GW1 → violation
- Richard nu răspunde la ping și nu s-a executat RICHARD-RESTORE.md → violation
- Cortex tunnel neactivat înainte de pornirea agenților → violation
- Runner: Genie verifică manual la fiecare startup; watchdog GW2 verifică health continuu

### CONNECT
- `COM-H-005` → Richard prioritate absolută — Pas 6 ping este mandatory
- `SEC-H-002` → port 18789 NEVER public — verificat implicit la Pas 1
- `PORT-18789-LOCALHOST-ENFORCEMENT.md` → audit complet porturi dacă Pas 1 detectează anomalie
- `RICHARD-RESTORE.md` → executat dacă Pasul 6 eșuează
- `OPENCLAW-SHUTDOWN.md` → perechea inversă a acestei proceduri
- `CORTEX-HEALTH-CHECK-OPENCLAW.md` → apelat dacă SSH tunnel Cortex (Pas 2) eșuează
- `procedure-health.json` → adaugă entry `"OPENCLAW-STARTUP": "active"`

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii 1-7 executați?
- [ ] Richard răspunde activ în #hub?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] OPENCLAW-STARTUP | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "OpenClaw Daily Startup" | FORGE ✓ | rule: OPS-H-001 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție startup | Genie (Sonnet) | Orchestrare pași simpli |
| Diagnosticare probleme la startup | Opus subagent | Reasoning complex pt. erori neașteptate |
| Verificare automată health | Watchdog GW2 | Continuu, fără LLM |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| OpenClaw binary | Runtime GW1 + GW2 | `openclaw` (în PATH) |
| SSH tunnel | Acces Cortex | `localhost:6400` → VPS |
| `openclaw.json` | Config GW1 + GW2 | `~/.openclaw/openclaw.json` |
| Workspaces | Memorie per-agent | `~/.openclaw/workspaces/<agent>/` |
| Tailscale | VPN pentru VPS | activ pe MacIntel |
| @Claudeautomationbot | Notificare | MacM4 relay |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Time to first Richard response | Cât durează startup complet | < 60 secunde |
| Agenți online la startup | Câți din 6 sunt funcționali | 6/6 |
| Cortex tunnel latency | Răspuns health check | < 200ms |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, COM-H-005)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu cele 3 checks obligatorii
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata FORGE
