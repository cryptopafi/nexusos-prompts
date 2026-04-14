---
type: procedure
created: 2026-03-17
status: active
slug: cortex-write-permissions
tags: [procedure, nexus]
---

# Cortex Collection Write Permission Control — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: SEC-H-006 (Richard Autonomy — Genie as Guardian)
**Scope**: Controlul și auditarea permisiunilor de scriere ale agenților OpenClaw în colecțiile Cortex — prevenirea poluării cross-agent a memoriei.

---

## 1. Problema

Cortex este shared knowledge base pentru toți agenții și Genie. Dacă un agent compromis sau cu comportament aberant poate scrie în orice colecție:
- Poate polua `procedures` collection cu lecții false → toți agenții le vor folosi
- Poate suprascrie `rules` collection → modificând reguli fundamentale
- Poate injecta "lecții" toxice în `shared_knowledge` → drift orchestrat al echipei
- Un agent cu SOUL.md deteriorat poate propagă corupția la ceilalți prin Cortex

Principiu aplicat: **least privilege** — fiecare agent scrie NUMAI în colecțiile proprii și în `shared_knowledge` cu flag explicit `shared: true`.

Situații acoperite:
- Setup inițial al permisiunilor la instalarea OpenClaw
- Audit periodic al permisiunilor
- Verificare după adăugarea unui agent nou
- Investigare incident de poluare Cortex

---

## 2. Procedura

### Pas 1: Maparea Permisiunilor per Agent
Definește matricea permisiunilor (READ/WRITE/NONE):

| Agent | procedures | shared_knowledge | rules | sessions | research | tasks |
|-------|-----------|-----------------|-------|----------|----------|-------|
| Richard | READ+WRITE | READ+WRITE | READ ONLY | READ+WRITE | READ | READ+WRITE |
| Finance | READ | READ+WRITE | READ ONLY | READ+WRITE | READ | READ |
| Marketing | READ | READ+WRITE | READ ONLY | READ+WRITE | READ | READ |
| Legal | READ | READ+WRITE | READ ONLY | READ+WRITE | READ | READ |
| Ops | READ | READ+WRITE | READ ONLY | READ+WRITE | READ | READ+WRITE |
| Insights | READ | READ+WRITE | READ ONLY | READ+WRITE | READ+WRITE | READ |
| Genie | READ+WRITE | READ+WRITE | READ+WRITE | READ+WRITE | READ+WRITE | READ+WRITE |

**Reguli cheie**:
- `rules` collection → READ ONLY pentru toți agenții (scrie doar Genie/Pafi)
- `procedures` collection → agenții pot propune (flag `pending_review`), doar Richard/Genie aprobă
- `shared_knowledge` → WRITE permis dar cu tag `agent_source` obligatoriu

### Pas 2: Implementare Permisiuni prin SKILL.md / MCP
Permisiunile se aplică la nivelul SKILL.md Cortex (sau MCP server Cortex):
- Fiecare agent primește un API token cu scope limitat
- Token Richard: scope `read:all, write:procedures, write:shared_knowledge, write:sessions`
- Token Insights: scope `read:all, write:research, write:shared_knowledge, write:sessions`
- Token restul: scope `read:all, write:shared_knowledge, write:sessions`
- Token Genie: scope `full` (admin)

**Notă (aspirational)**: Implementarea token-urilor cu scope per colecție depinde de Cortex API suportând acest feature. Verifică `GET /api/v1/auth/scopes` pe VPS înainte de a asuma că scope-urile funcționează. Dacă API-ul nu suportă scope-uri granulare → implementează restricțiile la nivelul MCP server (validation layer în `cortex-mcp-server/`).

### Pas 3: Audit Periodic Permisiuni
Verifică lunar că permisiunile nu s-au schimbat:
- Fiecare token Cortex per agent are scope-ul definit în Pas 2?
- Există token-uri expirate sau revocate care nu au fost regenerate?
- Există agenți care au scris în colecții interzise lor? (check Cortex audit log)

**Queries concrete de audit** (rulează pe VPS sau prin Cortex API):
```
# Intruziuni în rules collection (trebuie să fie ZERO)
GET /api/v1/collections/rules/entries?filter[agent_source][ne]=genie&filter[agent_source][ne]=pafi

# Proceduri fără pending_review de la agenți non-Richard
GET /api/v1/collections/procedures/entries?filter[metadata.pending_review]=true&filter[agent_source][ne]=richard

# Entries în colecții interzise per agent (exemplu Finance în procedures)
GET /api/v1/collections/procedures/entries?filter[agent_source]=finance
```

### Pas 4: Investigare Poluare Cortex
Dacă sunt suspiciuni că un agent a scris date neautorizate:
1. Caută în Cortex după `agent_source: <agentId>` în colecțiile interzise
2. Revizuiește timestamp-urile: ce s-a scris în ultima perioadă?
3. Dacă poluare confirmată → șterge entries suspecte + auditează SOUL.md agentului
4. Notifică Pafi prin Telegram

### Pas 5: Rotire Token-uri după Incident
Dacă un agent a fost compromis sau a avut comportament aberant:
- Revocă token-ul Cortex al agentului imediat
- Generează token nou cu aceleași permisiuni
- Actualizează SKILL.md sau MCP config al agentului
- Verifică că agentul nu mai poate accesa Cortex cu token-ul vechi

---

## 3. Cortex Logging

```json
{
  "text": "Audit permisiuni Cortex completat. Toți agenții au scope corect. Nicio intruziune detectată.",
  "collection": "procedures",
  "metadata": {
    "type": "security_audit",
    "procedure": "CORTEX-WRITE-PERMISSIONS",
    "rule_id": "SEC-H-001",
    "result": "PASS",
    "tags": ["security", "cortex", "permissions", "audit", "openclaw"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Setup inițial integrare Cortex-OpenClaw (Procedura #91 CORTEX-MCP-SERVER-DEPLOY)
- Audit lunar (nightly-audit.sh extins)
- Post-incident (după orice comportament aberant al unui agent)

### WHEN
- La fiecare adăugare agent nou în OpenClaw
- Lunar ca audit standard
- Imediat după incident de drift/comportament aberant al unui agent

### HOW (violation detection)
- Agent cu scope greșit pentru token Cortex → violation SEC-H-001
- Intrări în `rules` collection cu `agent_source` altul decât Genie/Pafi → INCIDENT critic
- Proceduri în `procedures` collection fără `pending_review` flag de la un agent non-Richard → violation
- Runner: verificare lunară manuală Genie + Cortex audit log queries

### CONNECT
- `SEC-H-001` → principiu least privilege pe API tokens
- `SEC-H-006` → Richard are scope extins față de restul (rol de orchestrator)
- `CORTEX-MCP-SERVER-DEPLOY.md` → la deploy se aplică această matrice de permisiuni
- `SOUL-MD-IMMUTABILITY.md` → perechea: dacă SOUL.md e imutabil, și regulile în Cortex trebuie protejate
- `procedure-health.json` → adaugă entry `"CORTEX-WRITE-PERMISSIONS": "active"`

### VERIFY
La finalul auditului, verifică:
- [ ] Toți agenții au token-uri cu scope-urile din matricea Pas 1?
- [ ] Nicio intrare în `rules` collection de la agenți non-Genie/Pafi?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] CORTEX-WRITE-PERMISSIONS | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Cortex Collection Write Permissions" | FORGE ✓ | rule: SEC-H-006 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Audit permisiuni | Genie (Sonnet) | Query simplu Cortex |
| Investigare poluare | Opus subagent | Reasoning complex, pattern detection |
| Token rotation | Genie direct | Acțiune simplă cu impact de securitate |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Cortex API | Knowledge base de protejat | `localhost:6400` (SSH tunnel) |
| Token management | Scope-uri per agent | Cortex admin API |
| SOUL.md per agent | Context de audit | `~/.openclaw/workspaces/<agent>/SOUL.md` |
| nightly-audit.sh | Verificare lunară automată | `/Users/pafi/.openclaw/nightly-audit.sh` |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-001, SEC-H-006)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [x] Matrice permisiuni validată cu Pafi (2026-03-03)
- [ ] Salvat în Cortex cu metadata FORGE
