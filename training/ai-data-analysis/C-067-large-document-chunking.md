---
type: procedure
created: 2026-03-22
status: active
slug: c-067-large-document-chunking
tags: [procedure, nexus]
---

# Handle Large Documents with AI (Chunking and Indexing) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Procesarea documentelor care depășesc context window-ul unui model AI prin strategii de chunking, indexare și sinteză cross-chunk, menținând coerența semantică a output-ului final.

---

## 1. Problema

Documentele mari (rapoarte anuale, contracte, cărți, baze de cunoaștere) nu pot fi procesate integral de un model AI cu context window limitat. Truncherea naivă distruge coerența semantică. Această procedură definește o metodologie sistematică de chunking care păstrează integritatea informațională și permite sinteze complete și coerente.

Situații acoperite:
- Analiza contractelor sau documentelor legale de sute de pagini
- Sumarizarea rapoartelor financiare sau de cercetare extensive
- Extragerea de informații din baze de cunoaștere mari
- Procesarea corespondențelor sau log-urilor voluminoase

---

## 2. Procedura

### Pas 1: Assess — Evaluează dimensiunea și structura documentului
Măsoară: număr de caractere/tokeni, structura documentului (capitole, secțiuni, paragrafe), tipul de conținut (narativ/tabelar/mixt). Compară cu context window-ul modelului ales (rezervă 20% pentru prompt + output). Determină dacă e necesar chunking sau documentul încape integral.

### Pas 2: Strategy — Alege strategia de chunking potrivită
Selectează în funcție de tipul documentului: (a) **Semantic** — chunkuri pe secțiuni/capitole naturale, ideal pentru documente structurate; (b) **Fixed-size** — chunkuri de dimensiune fixă cu overlap de 10-15%, pentru documente nestructurate; (c) **Sliding window** — fereastră mobilă cu overlap mare, pentru documente narative dense unde contextul se propagă. Documentează alegerea și justificarea.

### Pas 3: Chunk — Creează chunk-urile cu metadata
Generează chunk-urile conform strategiei alese. Pentru fiecare chunk adaugă metadata: {chunk_id, document_id, position (N/Total), section_title, key_entities[], summary_sentence, prev_chunk_summary}. Stochează în format structurat (JSON/CSV) alături de textul original.

### Pas 4: Process — Procesează chunk-urile cu AI
Rulează AI pe fiecare chunk cu un prompt consistent care include: contextul general al documentului, summary-ul chunk-ului anterior (din metadata), instrucțiunea de procesare specifică (sumarizare/extracție/clasificare). Colectează output-urile în ordine. Loghează prompt-urile pentru reproductibilitate.

### Pas 5: Index — Construiește indexul cross-chunk
Creează un index de referință încrucișată: entități cheie → chunk-urile unde apar, teme principale → distribuția lor, decizii/cifre importante → localizare exactă. Acest index permite căutarea și sinteza fără re-procesare.

### Pas 6: Synthesize — Sintetizează output-urile chunk-urilor
Folosind indexul și output-urile individuale, construiește sinteza finală. Prompt-ul de sinteză trebuie să includă toate output-urile chunk sau un rezumat intermediar al lor. Verifică că sinteza acoperă toate temele majore (compară cu indexul). Identifică și rezolvă contradicțiile inter-chunk.

---

## 3. Cortex Logging

```json
{
  "text": "Large Document Chunking pentru [document]. Dimensiune: [X tokeni]. Strategie: [semantic/fixed/sliding]. Chunks: [N]. Index creat: [da/nu]. Sinteză finală: [tip output].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-067",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["chunking", "large-documents", "indexing", "context-window", "document-processing", "synthesis"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La procesarea oricărui document care depășește 70% din context window-ul modelului ales

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Chunking fără metadata (Pasul 3) → violation — index imposibil de construit
- Sinteza finală fără verificare acoperire (Pasul 6) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] C-067 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Handle Large Documents with AI (Chunking and Indexing)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Document sursă | Input pentru chunking |
| Script de chunking (Python/tool) | Automatizare Pasul 3 |
| Model AI cu context window definit | Procesare Pasul 4 |
| Index storage (JSON/vector DB) | Persistența indexului Pasul 5 |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Chunks cu metadata completă | 100% |
| Teme majore acoperite în sinteză | ≥ 95% din index |
