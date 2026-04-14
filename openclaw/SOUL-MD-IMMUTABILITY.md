---
type: procedure
created: 2026-03-17
status: active
slug: soul-md-immutability
tags: [procedure, nexus]
---

# SOUL.md Immutability Enforcement — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regulă asociată**: SEC-H-006 (Richard Autonomy — Genie as Guardian), META-H-002 (Enforcement Loop Mandatory)
**Scope**: Garantarea că fișierele SOUL.md ale agenților OpenClaw rămân imutabile pentru agenți — modificabile exclusiv de Genie sau Pafi.

---

## 1. Problema

SOUL.md definește identitatea, valorile și limitele fiecărui agent. Dacă un agent își poate modifica propriul SOUL.md:
- Poate elimina restricții de securitate (ex. "nu accesez fișiere din afara workspace-ului meu")
- Poate modifica limitele de autonomie (ex. "pot executa cod fără aprobare")
- Poate deriva de la scopul său original (ex. Marketing agent care decide că e și Legal)
- Self-modification fără control = cel mai mare risc de comportament aberant pe termen lung

Regula: SOUL.md rămâne IMUTABIL pentru agenți. Agenții pot acumula lecții în LEARNINGS.md (additive), dar nu pot edita SOUL.md. Modificările SOUL.md sunt apanajul exclusiv al Genie/Pafi.

Situații acoperite:
- Tentativă agent de a edita propriul SOUL.md
- Tentativă agent de a edita SOUL.md-ul altui agent
- Audit periodic că SOUL.md nu s-a modificat neautorizat
- Procesul oficial de modificare SOUL.md prin Genie

---

## 2. Procedura

### Pas 1: Audit Integritate SOUL.md (periodic)
Verifică că niciun SOUL.md nu s-a modificat neautorizat:
- Pentru fiecare agent: compară hash-ul SOUL.md curent cu hash-ul din ultimul audit
- Surse de referință: git history (`~/.openclaw/workspaces/` dacă e sub git) sau hashes stocate în Cortex
- Dacă hash schimbat și nu e o modificare autorizată de Genie → INCIDENT

### Pas 2: Detectare Tentativă Modificare
Identifică dacă un agent a încercat să editeze un SOUL.md:
- Verifică logs de tool use ale agenților — există apeluri `write` sau `edit` pe `SOUL.md`?
- Verifică daily logs `memory/YYYY-MM-DD.md` pentru mențiuni de auto-modificare
- Richard este responsabil să semnaleze dacă observă un agent care menționează intenția de a-și modifica SOUL.md

### Pas 3: Procesul Oficial de Modificare SOUL.md
Când o modificare legitimă e necesară (ex. agent upgrade, nou scop, feedback de performanță):
1. Pafi sau Genie identifică nevoia de modificare
2. Genie redactează modificarea propusă + justificare
3. Pafi aprobă (Telegram sau sesiune directă)
4. Genie execută modificarea direct în fișier (nu agentul)
5. Versiunea veche arhivată în `~/.openclaw/workspaces/<agent>/SOUL.md.bak-{date}`
6. Agentul restartat pentru a încărca noul SOUL.md
7. Cortex logat: cine, ce, de ce, când

### Pas 4: Protecție Sistem de Fișiere (opțional)
Dacă securitatea e critică, SOUL.md poate fi setată read-only la nivel de OS:
- `chmod 444 ~/.openclaw/workspaces/<agent>/SOUL.md`
- Avantaj: agentul nu poate scrie fizic chiar dacă încearcă prin tool
- Dezavantaj: Genie trebuie să schimbe permisiunile la fiecare update legitim
- Decizie la setup: aplică sau nu în funcție de nivel risc perceput

### Pas 5: Response la Incident (modificare neautorizată detectată)
Dacă SOUL.md a fost modificat fără autorizare:
1. **STOP imediat** agentul respectiv (execută AGENT-RESTART-STATE-PRESERVATION.md cu forță)
2. **Restaurează** SOUL.md din backup sau git
3. **Auditează** ce a modificat agentul și de ce (session logs)
4. **Determină** cauza: bug OpenClaw, prompt injection, sau comportament emergent
5. **Reportează** Pafi prin Telegram cu toate detaliile
6. **Nu reporni** agentul până cauza e înțeleasă

---

## 3. Cortex Logging

**Audit clean**:
```json
{
  "text": "SOUL.md audit: toate 6 fișiere intacte. Hash-uri confirmate. Nicio modificare neautorizată.",
  "collection": "procedures",
  "metadata": {
    "type": "security_audit",
    "procedure": "SOUL-MD-IMMUTABILITY",
    "rule_id": "SEC-H-006",
    "result": "PASS",
    "tags": ["security", "soul-md", "immutability", "audit", "openclaw"]
  }
}
```

**Modificare legitimă executată**:
```json
{
  "text": "SOUL.md [agentId] modificat autorizat. Autor: Genie/Pafi. Motiv: [X]. Versiunea anterioară arhivată.",
  "collection": "procedures",
  "metadata": {
    "type": "config_change",
    "procedure": "SOUL-MD-IMMUTABILITY",
    "rule_id": "SEC-H-006",
    "authorized": true,
    "tags": ["soul-md", "modification", "authorized", "openclaw"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Setup inițial OpenClaw: după crearea fiecărui SOUL.md, hash stocat în Cortex
- Audit săptămânal (nightly-audit.sh sau Genie supervisor)
- Post-incident: orice comportament aberant al unui agent

### WHEN
- La fiecare setup agent nou (hash inițial înregistrat)
- Săptămânal ca audit standard
- Imediat după orice incident de drift sau comportament neașteptat

### HOW (violation detection)
- Hash SOUL.md schimbat fără commit autorizat în Cortex → INCIDENT imediat
- Agent care menționează editarea SOUL.md propriu în daily log → flag pentru review
- Tool call `write` sau `edit` pe fișier `SOUL.md` din sesiunea unui agent → violation critică
- Runner: comparare hash la fiecare startup prin OPENCLAW-STARTUP.md extins

### CONNECT
- `SEC-H-006` → regula sursă
- `OPENCLAW-STARTUP.md` → Pas 4 (verificare workspace) extins cu hash check SOUL.md
- `AGENT-RESTART-STATE-PRESERVATION.md` → apelat în incident response (Pas 5)
- `LEARNINGS.md` → alternativa legitimă pentru agent learning (additive, nu modificare SOUL.md)
- `procedure-health.json` → adaugă entry `"SOUL-MD-IMMUTABILITY": "active"`

### VERIFY
La finalul auditului, verifică:
- [ ] Hash-urile tuturor SOUL.md confirmate nemodificate?
- [ ] Procesul oficial de modificare respectat dacă s-a modificat ceva?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] SOUL-MD-IMMUTABILITY | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "SOUL.md Immutability Enforcement" | FORGE ✓ | rule: SEC-H-006 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Hash comparison audit | Script bash (determinist) | Verificare mecanică |
| Investigare incident modificare | Opus subagent | Reasoning profund, analiza cauzei |
| Redactare modificare legitimă SOUL.md | Opus subagent | Impact major = reasoning Opus obligatoriu |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| SOUL.md per agent | Fișier de protejat | `~/.openclaw/workspaces/<agent>/SOUL.md` |
| Cortex hash store | Referință pentru audit | `procedures` collection, tag `soul-md-hash` |
| Git history | Audit trail modificări | `~/.openclaw/` (dacă sub git) |
| LEARNINGS.md | Alternativa legitimă | `~/.openclaw/workspaces/<agent>/LEARNINGS.md` |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-006, META-H-002)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Hash-uri SOUL.md inițiale înregistrate în Cortex la setup
- [ ] Salvat în Cortex cu metadata FORGE
