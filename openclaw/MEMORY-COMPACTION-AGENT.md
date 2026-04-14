---
type: procedure
created: 2026-03-17
status: active
slug: memory-compaction-agent
tags: [procedure, nexus]
---

# Memory Compaction Agent — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.1
**Regulă asociată**: OPS-H-001 (Technical Autonomy), MEM-H-001 (Everything to Cortex)
**Scope**: Compactarea MEMORY.md pentru agenții OpenClaw când dimensiunea depășește 200 linii — prevenirea degradării contextului și a costurilor excesive de token.

---

## 1. Problema

Fiecare agent OpenClaw menține un `MEMORY.md` în workspace-ul propriu. Fără compactare:
- MEMORY.md crește necontrolat (detectat de NIGHTLY-AUDIT Pas 2: `wc -l > 200`)
- Context window al agentului e poluat cu informații vechi, redundante sau contradictorii
- Cost tokenuri crescut per sesiune
- Lecții contradictorii ("always use X" + "never use X") rămân nedezambiguizate

**Situații acoperite**:
- MEMORY.md > 200 linii (trigger automat din NIGHTLY-AUDIT)
- Lecții contradictorii detectate (trigger din NIGHTLY-AUDIT Pas 3 — Genie judgment)
- Compactare manuală la cererea Pafi (oricând, indiferent de dimensiune)

---

## 2. Procedura

### Pas 1: Identificare candidați pentru compactare
```bash
# Verifică dimensiunea MEMORY.md per agent
for agent in rich-new finance marketing legal ops insights; do
  LINES=$(wc -l < ~/.openclaw/workspaces/$agent/MEMORY.md 2>/dev/null || echo 0)
  echo "$LINES linii: $agent"
done | sort -rn
```

Trigger: orice agent cu > 200 linii → candidat pentru compactare.

### Pas 2: Backup MEMORY.md înainte de compactare
```bash
AGENT="rich-new"  # sau alt agent
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
BACKUP_DIR="$HOME/.openclaw/backups/memory-compaction"
mkdir -p "$BACKUP_DIR"

cp ~/.openclaw/workspaces/$AGENT/MEMORY.md \
   $BACKUP_DIR/${AGENT}-MEMORY-${TIMESTAMP}.md

echo "Backup: $BACKUP_DIR/${AGENT}-MEMORY-${TIMESTAMP}.md"
```

### Pas 3: Analiză conținut (Genie — Sonnet)
Citește MEMORY.md și identifică:
- **Duplicate** — același fapt enunțat de mai multe ori
- **Contradicții** — "always X" vs "never X" pentru același subiect
- **Informații expirate** — referințe la proiecte terminate, configurații vechi
- **Core lessons** — insighturi fundamentale care trebuie păstrate

Format analiză:
```
DUPLICATE: [entry 1] = [entry 2] → keep: [care]
CONTRADICȚIE: "[text A]" vs "[text B]" → rezolvare: [care e corectă și de ce]
EXPIRAT: "[entry]" → motiv expirare: [...]
CORE (păstrează nemodificat): "[entry]"
```

### Pas 4: Compactare (Opus subagent — judecată necesară)
Delegă unui subagent Opus cu task-ul explicit:

```
Compactează MEMORY.md pentru agentul [AGENT].
Fișier: ~/.openclaw/workspaces/[AGENT]/MEMORY.md
Backup existent la: [path backup]

Reguli de compactare:
1. Păstrează TOATE core lessons (pilon central al memoriei agentului)
2. Rezolvă contradicțiile — keep cea mai recentă sau mai specifică
3. Elimină duplicate — păstrează formularea cea mai clară
4. Elimină informații expirate (proiecte terminate, configurații vechi)
5. Structurează în secțiuni clare cu headings
6. Target: < 150 linii (cu 25% buffer față de limita de 200)
7. Nu inventa informații noi — doar reorganizează și elimină

Output: fișier compactat complet, gata de scriere.
Salvează output-ul în: /tmp/${AGENT}-MEMORY-compacted.md
```

Opus subagent TREBUIE să scrie rezultatul în `/tmp/${AGENT}-MEMORY-compacted.md` — acesta e fișierul validat la Pas 5.

### Pas 5: Validare diff înainte de aplicare
```bash
AGENT="rich-new"
diff $BACKUP_DIR/${AGENT}-MEMORY-${TIMESTAMP}.md \
     /tmp/${AGENT}-MEMORY-compacted.md | head -50
```

Verifică că:
- Nu s-au pierdut informații critice (core lessons prezente)
- Contradicțiile sunt rezolvate consistent
- Dimensiunea e < 150 linii

### Pas 6: Aplicare
```bash
AGENT="rich-new"
cp /tmp/${AGENT}-MEMORY-compacted.md \
   ~/.openclaw/workspaces/$AGENT/MEMORY.md

echo "Compactare aplicată. Linii noi: $(wc -l < ~/.openclaw/workspaces/$AGENT/MEMORY.md)"
```

### Pas 7: Logging Cortex
Salvează în Cortex folosind structura din §3:
```bash
curl -s http://100.81.233.9:3200/save -X POST -H "Content-Type: application/json" \
  -d '{
    "text": "Memory compaction agent '"$AGENT"': [N_INAINTE] → [N_DUPA] linii. Duplicate eliminate: [N]. Contradictii rezolvate: [N]. Expirate sterse: [N]. Backup: '"$BACKUP_DIR/${AGENT}-MEMORY-${TIMESTAMP}.md"'.",
    "collection": "sessions",
    "metadata": {
      "source_agent": "genie",
      "type": "memory_compaction",
      "procedure": "MEMORY-COMPACTION-AGENT",
      "rule_id": "OPS-H-001",
      "tags": ["memory", "compaction", "openclaw", "agent", "optimization"]
    }
  }'
```
Înlocuiește `[N_INAINTE]`, `[N_DUPA]`, `[N]` cu valorile efective din sesiune.

---

## 3. Cortex Logging

```json
{
  "text": "Memory compaction agent [AGENT]: [N_INAINTE] → [N_DUPA] linii. Duplicate eliminate: [N]. Contradictii rezolvate: [N]. Expirate sterse: [N]. Backup: memory-backups/[FILE].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "memory_compaction",
    "procedure": "MEMORY-COMPACTION-AGENT",
    "rule_id": "OPS-H-001",
    "tags": ["memory", "compaction", "openclaw", "agent", "optimization"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Declanșat de NIGHTLY-AUDIT Pas 2 (detectare MEMORY.md > 200 linii)
- Executat de Genie la 09:00 în cadrul NIGHTLY-AUDIT Pas 3
- Manual: la cererea Pafi sau la detectare contradicții

### WHEN
- **Automat**: NIGHTLY-AUDIT detectează > 200 linii → marcat pentru compactare → Genie execută la 09:00
- **Manual**: la sesiunea Genie dacă raportul nocturn include candidați de compactare
- **Nu există limita de frecvență** — compactarea se face ori de câte ori e necesar

### HOW (violation detection)
- MEMORY.md > 200 linii > 48h fără compactare → violation MEM-H-001 (calitate memorie degradată)
- Compactare executată fără backup → violation (informații pierdute irecuperabil)
- Pas 4 delegat la Sonnet în loc de Opus → risc pierdere nuanțe → violation recomandare
- Runner: NIGHTLY-AUDIT Pas 2 detectează + Genie execută la 09:00

### CONNECT
- `OPS-H-001` → autonomie: compactarea e automată, fără intervenție Pafi
- `MEM-H-001` → calitate memorie: MEMORY.md compact = context mai bun pentru agent
- `NIGHTLY-AUDIT-OPENCLAW.md` → Pas 2 detectează, Pas 3 execută compactare
- `BACKUP-AUTOMATION-OPENCLAW.md` → backup workspace existent, dar compactare face backup dedicat
- `procedure-health.json` → entry `"MEMORY-COMPACTION-AGENT": "active"`

### VERIFY
- [ ] MEMORY.md < 200 linii post-compactare?
- [ ] Backup pre-compactare creat în `memory-backups/`?
- [ ] Validare diff executată (core lessons prezente)?
- [ ] Logging Cortex completat cu metrici?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → compactare incompletă

**VK-uri obligatorii**:
1. `✅ [PROC] MEMORY-COMPACTION-AGENT | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Memory Compaction Agent" | FORGE ✓ | rule: OPS-H-001 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Detecție candidați (Pas 1) | Script bash | `wc -l`, zero cost |
| Analiză conținut (Pas 3) | Genie (Sonnet) | Pattern recognition, cost moderat |
| Compactare efectivă (Pas 4) | **Opus subagent** | Judecată nuanțată — riscul de a pierde lecții critice |
| Validare diff (Pas 5) | Genie (Sonnet) | Verificare rapidă |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `~/.openclaw/workspaces/*/MEMORY.md` | Fișiere țintă | 6 agenți |
| `~/.openclaw/backups/memory-compaction/` | Backup pre-compactare | Creat de procedură |
| `NIGHTLY-AUDIT-OPENCLAW.md` | Trigger automat | Pas 2 (detecție) + Pas 3 (execuție) |
| Opus subagent | Compactare cu judgment | Model routing obligatoriu |
| Cortex API (`http://100.81.233.9:3200/save`) | Logging post-compactare | Pas 7 |
| `~/.nexus/monitoring/procedure-health.json` | Status procedură | Entry `"MEMORY-COMPACTION-AGENT": "active"` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| MEMORY.md size post-compaction | Linii rămase după compactare | < 150 linii |
| Reduction ratio | % reducere față de pre-compactare | > 25% |
| Core lesson retention | % lecții esențiale păstrate | 100% |
| Time-to-compact | De la detecție la aplicare | < 24 ore (next 09:00) |
| Contradiction resolution rate | % contradicții rezolvate per compactare | 100% |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — formalizează compactarea MEMORY.md referită în NIGHTLY-AUDIT Pas 2/3 |
| 1.1 | 2026-03-04 | FORGE-AUDIT: Pas 4 — output path explicit (/tmp/), Pas 7 — comanda Cortex inline, Dependencies — Cortex API + procedure-health.json adăugate |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, MEM-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Opus routing explicitat (judecată critică la Pas 4)
- [x] Salvat în Cortex cu metadata FORGE
