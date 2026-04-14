---
type: procedure
created: 2026-03-17
status: active
slug: cortex-procedure-retrieval
tags: [procedure, nexus]
---

# Cortex Procedure Retrieval for Agents — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: PROC-H-001 (Procedure-First Execution Gate), DEV-H-008 (Check Cortex Before Starting Any Project)
**Scope**: Protocolul prin care agenții OpenClaw caută și aplică proceduri din Cortex înainte de a executa orice task — evitând reinventarea roții și asigurând consistența.

---

## 1. Problema

Cortex conține 297 proceduri FORGE'd + toate lecțiile acumulate. Fără un protocol clar de retrieval, agenții:
- Rezolvă taskuri ad-hoc, ignorând proceduri existente
- Produc output inconsistent față de standardele documentate
- Repetă greșeli deja rezolvate în alte proceduri
- Reinventează abordări pe care sistemul le-a rafinat deja

PROC-H-001 impune **procedure-first**: înainte de orice acțiune, caută dacă există o procedură.

Situații acoperite:
- Orice task nou primit de un agent
- Delegare de la Richard — include referința la procedura relevantă
- Task recurent — verifica dacă procedura s-a updatat
- Situație de eroare — cauta error_fix în Cortex

---

## 2. Procedura

### Pas 1: Task Classification la Primire
Când un agent primește un task, clasifică-l înainte de orice altceva:
- **Categorie**: research | analysis | campaign | legal | technical | financial | operational
- **Cuvinte cheie**: extrage 3-5 termeni principali din task
- **Tip output**: document | raport | acțiune | decizie | cod

### Pas 2: Cortex Search pentru Proceduri
Caută în Cortex proceduri relevante folosind MCP tool:
```
cortex_search(
  query: "<categorie> <cuvinte cheie>",
  collection: "procedures",
  limit: 5
)
```
Filtre utile:
- `type: "task_pattern"` pentru pattern-uri de abordare
- `type: "procedure"` pentru proceduri standard complete
- `tags: ["<categorie>"]` pentru proceduri specifice domeniului

### Pas 3: Evaluare Rezultate
Pentru fiecare rezultat returnat de Cortex:
- **Relevanță > 0.7** (Cortex score): citește procedura
- **Relevanță 0.5-0.7**: citește summary, decide dacă se aplică
- **Relevanță < 0.5**: ignoră

**Notă calibrare (experimental)**: Pragurile 0.7 / 0.5 sunt inițiale și netestate empiric. Revizuiește după 30 zile de utilizare și ajustează în funcție de calitatea procedurilor returnate (prea multe false positive → ridică pragul; prea multe missed → coboară).

Dacă sunt proceduri relevante găsite:
- Urmează procedura ca framework de execuție
- Nu ignora pașii din procedură fără justificare explicită
- Dacă procedura e outdated (referințe vechi, context schimbat) → notează pentru update

### Pas 4: Caută Error Fixes (dacă task implică debugging)
Separat de proceduri generale, caută fix-uri cunoscute:
```
cortex_error_match(error_text: "<eroare sau problemă specifică>")
```
Rezultatele din error_fix sunt soluții confirmate anterior — aplica-le direct.

### Pas 5: Caută Lessons (LEARNINGS.md context)
Adaugă context din lecțiile acumulate:
```
cortex_agent_learnings(agent_id: "<propriu agent ID>")
```
Lecțiile relevante pentru tipul de task se injectează în contextul de execuție.

### Pas 6: Execuție cu Referință la Procedură
Execută taskul cu procedura găsită ca ghid:
- Menționează în răspuns / handoff: "Am folosit procedura X din Cortex"
- Dacă deviezi de la procedură: explică de ce
- Dacă nu există procedură: execută cel mai bun judgment și salvează ca lecție nouă

**Escalation când nu există procedură** (relevanță < 0.5 pe toate rezultatele):
- Dacă task e recurent (l-ai mai văzut): escalat la Richard — probabil există o procedură neindexată sau outdated
- Dacă task e nou și complex: execută cu cel mai bun judgment, apoi notifică Richard că un gap a fost detectat în Cortex (potențial procedură nouă de creat)

### Pas 7: Feedback Post-Execuție
Dacă procedura a fost aplicată, oferă feedback Cortex:
```
cortex_feedback(
  id: "<id procedura>",
  success: true/false,
  notes: "ce a funcționat / ce nu"
)
```
Feedback-ul îmbunătățește ranking-ul procedurii pentru viitoare căutări.

**Failure handling Pas 7**: Dacă `cortex_feedback` eșuează (tool error, timeout, connection lost) → skip silențios. Loghează în daily log agentului: `[FEEDBACK-SKIP] procedure <id>, motiv: <eroare>`. Nu bloca flow-ul principal pentru un feedback eșuat — scopul primar (execuția taskului) deja e completat.

---

## 3. Cortex Logging

```json
{
  "text": "Procedură aplicată: [ID/titlu]. Task: [descriere scurtă]. Succes: DA/NU. Agent: [agentId].",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_application",
    "procedure": "CORTEX-PROCEDURE-RETRIEVAL",
    "rule_id": "PROC-H-001",
    "applied_procedure_id": "<id>",
    "tags": ["procedure-retrieval", "openclaw", "proc-h-001"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Fiecare task primit de un agent — execuția trebuie să înceapă cu Pasul 1-3
- SOUL.md al fiecărui agent va conține instrucțiunea de procedure-first
- Richard la delegare: verifică Cortex și include referința la procedură relevantă în mesajul de delegare

### WHEN
- La FIECARE task nou, fără excepție
- La orice situație de eroare (Pasul 4 obligatoriu)
- La delegarea de la Richard (Pasul 2 executat de Richard înainte de delegare)

### HOW (violation detection)
- Agent care produce output fără a menționa nicio procedură sau lecție din Cortex pe un task recurent → sub-optim, flagged
- Agent care ignoră o procedură cu relevanță > 0.8 → violation PROC-H-001
- Agent care nu oferă feedback post-execuție pe proceduri aplicate → lecțiile nu se îmbunătățesc
- Runner: Richard poate audita retrospectiv dacă agenții referențiază proceduri; Genie audit săptămânal

### CONNECT
- `PROC-H-001` → regula sursă
- `DEV-H-008` → Check Cortex before starting
- `CORTEX-MCP-SERVER-DEPLOY.md` → prerequisite (fără MCP server, tool-urile nu sunt disponibile)
- `CORTEX-HEALTH-CHECK-OPENCLAW.md` → apelat dacă `cortex_search` returnează eroare
- `procedure-health.json` → adaugă entry `"CORTEX-PROCEDURE-RETRIEVAL": "active"`

### VERIFY
La finalul execuției unui task, verifică:
- [ ] Cortex căutat înainte de execuție?
- [ ] Proceduri relevante aplicate sau devierile justificate?
- [ ] Feedback dat pentru procedura aplicată?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] CORTEX-PROCEDURE-RETRIEVAL | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Cortex Procedure Retrieval for Agents" | FORGE ✓ | rule: PROC-H-001 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție retrieval (cortex_search) | Agent curent (orice model) | MCP tool call simplu |
| Evaluare relevanță proceduri multiple | Richard sau Genie | Judgment de orchestrator |
| Update procedură outdated găsită | Opus subagent | Modificare procedură = impact system-wide |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Cortex MCP Server | Tool `cortex_search` disponibil | stdio transport, mcp-adapter |
| Cortex `procedures` collection | 297+ proceduri indexate | via `cortex_search` |
| SOUL.md per agent | Instrucțiunea procedure-first | `~/.openclaw/workspaces/<agent>/SOUL.md` |
| Richard | Orchestrator care include proceduri la delegare | agent `main` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Procedure retrieval rate | % taskuri cu Cortex search executat | > 80% după 30 zile |
| Procedure application rate | % căutări cu procedură relevantă găsită | > 60% după 30 zile |
| First-try success rate | % taskuri fără retry când procedura e aplicată | > 70% (vs 50% baseline) |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (PROC-H-001, DEV-H-008)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [ ] SOUL.md al fiecărui agent updatat cu instrucțiunea procedure-first
- [ ] Salvat în Cortex cu metadata FORGE
