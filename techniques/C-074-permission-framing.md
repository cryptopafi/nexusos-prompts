---
id: C-074
title: Permission Framing
domain: ai-prompt-engineering
version: 1.0
forge_audit: PASS
created: 2026-03-06
source: Reddit r/PromptEngineering (64 upvotes "be wrong if you need to") | Reddit r/PromptEngineering OpenAI Guide top 3 (165 upvotes) | PR-019 Self-Calibration | ND-011 Negative Prompting (anti-pattern confirmat) | PE INSIGHT EPR 16/20
---

# C-074 Permission Framing — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regula asociata**: PROMPT-H-002
**Scope**: Micro-tehnica empirica pentru reducerea confident hallucinations prin eliberarea modelului de presiunea de competenta aparenta si conversia instructiunilor negative in instructiuni pozitive.

---

## §1 — Problema

Fara Permission Framing, modelele LLM au tendinta de a:
- Genera raspunsuri care par sigure si corecte chiar si atunci cand sunt incerte — **confident hallucinations**
- Urma instructiuni de tip "don't X" mai slab decat instructiunile pozitive echivalente (confirmat empiric, ND-011, 165 upvotes Reddit)
- Omite semnalele de incertitudine in output-ul final pentru a parea mai competente

Situatii acoperite:
- Task-uri cu risc de over-confidence (cercetare, fact-checking, analiza de date, recomandari medicale/legale/financiare)
- Prompt-uri care contin instructiuni de tip "don't X" sau "avoid Y"
- Decizii high-stakes unde calibrarea incertitudinii e critica

---

## §2 — Procedura

### Pas 1: Audit instructiunile negative
Citeste prompt-ul curent si identifica toate instructiunile de forma:
- "Don't be verbose"
- "Avoid using technical jargon"
- "Don't repeat yourself"
- "Never say I don't know"

Converteste FIECARE in echivalentul pozitiv:
- "Don't be verbose" → "Use concise, direct sentences. Maximum 3 sentences per point."
- "Avoid technical jargon" → "Use plain language accessible to a non-technical reader."
- "Don't repeat yourself" → "Each sentence must add new information not present in previous sentences."
- "Never say I don't know" → "When uncertain, describe what you DO know and where the gap is."

Regula: instructiunile negative sunt rezervate STRICT pentru safety/refusal, nu pentru stil sau comportament general.

### Pas 2: Evalueaza riscul de over-confidence
Determina clasa task-ului:

| Clasa | Caracteristici | Actiune |
|-------|---------------|---------|
| HIGH-STAKES | Sanatate, legal, financiar, cod critic, date factuale verificabile | Permission + Self-Calibration |
| MEDIUM | Analiza, comparatii, sinteze complexe | Permission framing simplu |
| LOW / TRIVIAL | Creative, one-shot, follow-ups in context activ | Nu aplica permission framing |

### Pas 3: Adauga Permission Framing (pentru HIGH si MEDIUM)

Pentru MEDIUM — adauga la finalul system prompt-ului sau al instructiunii principale:
```
If you're uncertain about any claim, say so directly. It's better to acknowledge
uncertainty than to sound confident and be wrong.
```

Pentru HIGH-STAKES — adauga ambele:
```
If you're uncertain about any claim, say so directly. It's better to acknowledge
uncertainty than to sound confident and be wrong.

For each major claim or recommendation, rate your confidence (0-100%) and briefly
note what evidence supports it and what evidence might contradict it.
```

### Pas 4: Aplica Self-Calibration trigger (doar HIGH-STAKES)
Dupa ce modelul genereaza output-ul, verifica daca a respectat calibrarea:
- Output-ul contine scoruri de incredere? (0-100% per claim)
- Output-ul diferentiaza intre fapte sigure si inferente?
- Daca NU: adauga la re-prompt "Please review each claim and add your confidence level (0-100%) and the basis for that confidence."

### Test Cases

1. **Normal flow** — Prompt de analiza medicala: "Ce tratamente sunt recomandate pentru X?"
   - Aplica: MEDIUM/HIGH → adauga permission framing + Self-Calibration
   - Output asteptat: raspuns cu afirmatii calificate ("Studies suggest... [85% confidence]") si recunoastere explicita a limitelor ("I am not a medical professional...")

2. **Edge case** — Prompt creativ: "Scrie un poem despre toamna."
   - Nu aplica permission framing — incertitudinea e inutila in context creativ
   - Output asteptat: poem fara calificari de incredere

3. **Failure case** — Prompt simplu de follow-up in context activ: "Ce ai spus mai devreme?"
   - Nu aplica — context activ, risc zero de over-confidence pe recap
   - Comportament asteptat: ignorare permission framing, raspuns direct

---

## §3 — Cortex Logging

```json
{
  "text": "C-074 Permission Framing aplicat: task=[descriere scurta] | clasa=[HIGH/MEDIUM/LOW] | conversii negative→pozitiv=[N] | self-calibration=[YES/NO]",
  "collection": "technical",
  "metadata": {
    "type": "procedure_artifact",
    "procedure": "C-074",
    "rule_id": "PROMPT-H-002",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["permission-framing", "hallucination-reduction", "self-calibration", "positive-instruction", "ai-prompt-engineering"]
  }
}
```

---

## §4 — Enforcement Loop (META-H-002)

### WHERE
- PromptForge v2.3 Phase 2 (Optimize) — dupa aplicarea tehnicii principale, inainte de finalizare
- Orice task cu risc de over-confidence detectat in WISH Step A (Scope)
- Post-H Skill extraction — cand output-ul contine afirmatii factuale nequalificate

### WHEN
- La FIECARE prompt care trece prin PromptForge cu clasa HIGH sau MEDIUM
- La detectia de instructiuni "don't X" in orice prompt curent
- La task-uri cu domeniu: medical, legal, financiar, stiintific, date factuale

### HOW (violation detection)
- Prompt finalizat contine instructiuni "don't X" neconvertite → violation
- Task HIGH-STAKES fara Self-Calibration trigger → violation
- Output final cu afirmatii factuale fara nicio calificare de incertitudine pe task HIGH → violation
- Runner: checklist manual pre-output pe PromptForge Phase 2; audit Opus la review periodic

### CONNECT
- PROMPT-H-002 (hard rule) → Permission Framing e parte din optimizarea obligatorie
- PR-019 Self-Calibration → furnizeaza mecanismul de scoring per claim (High-Stakes path)
- ND-011 Negative Prompting → anti-pattern confirmat, aceasta procedura il rezolva
- C-065-self-calibration.md → procedura extinsa pentru Self-Calibration (referentiaza)
- `procedure-health.json` → adauga entry C-074

### VERIFY
- [ ] Toate instructiunile "don't X" au fost convertite in "do Y"?
- [ ] Clasa task-ului a fost evaluata (HIGH/MEDIUM/LOW/TRIVIAL)?
- [ ] Permission framing adaugat pentru HIGH si MEDIUM?
- [ ] Self-Calibration adaugat pentru HIGH-STAKES?
- [ ] Daca oricare = NU → procedura NU e completa

**VK-uri obligatorii** (per VK-H-001):
1. `[C-074] Permission Framing | negatives-converted=[N] | class=[HIGH/MEDIUM/LOW] | self-cal=[YES/NO] | DONE`
2. `[CORTEX] "C-074 Permission Framing aplicat" | collection: technical | type: procedure_artifact | SAVED`

### MODEL ROUTING

| Activitate | Model | Motivul |
|-----------|-------|---------|
| Aplicare procedura per task | Sonnet 4.6 (Genie orchestrator) | Conversie rapida, reguli clare |
| Audit periodic al aplicarii | Opus 4.6 subagent | Detectare gap-uri subtile in calibrare |
| Creare procedura | Sonnet 4.6 | FORGE template asigura structura |

---

## §4b — Note pentru Proceduri care Genereaza Prompturi

Aceasta procedura produce modificari la prompturi existente. Cand prompturile modificate sunt deployate in productie (API calls repetate):
- Referentiaza C-073 (Promptware Engineering) pentru versioning si eval gate
- Versioning: adauga "-pf" suffix la versiunea promptului dupa aplicarea Permission Framing (ex: v1.0 → v1.0-pf)
- Rollback criterion: daca calibrarea incertitudinii creste latenta perceputa fara a imbunatati acuratetea → revert

---

## §5 — Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| PR-019 Self-Calibration | Mecanism scoring per claim | `~/.nexus/procedures/training/ai-prompt-engineering/C-065-self-calibration.md` |
| ND-011 Negative Prompting | Anti-pattern sursa (confirma conversia) | Cortex: collection technical, tag ND-011 |
| PromptForge v2.3 | Procesul gazda (Phase 2 Optimize) | `~/.nexus/procedures/PROMPTING.md` |
| C-073 Promptware Engineering | Versioning dupa aplicare | `~/.nexus/procedures/training/ai-prompt-engineering/C-073-promptware-engineering.md` |

---

## §6 — Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Negative-to-positive conversion rate | % din instructiuni "don't X" convertite corect | 100% per prompt procesat |
| Self-calibration trigger rate | % task-uri HIGH cu Self-Calibration adaugat | 100% |
| False positive rate | % task-uri TRIVIAL/creative cu permission framing adaugat gresit | 0% |

---

## Checklist Pre-Publicare

- [x] Regula asociata: PROMPT-H-002 (exista in rules-hard.md)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu checks obligatorii
- [x] Doua VK-uri specificate (per VK-H-001)
- [x] Test cases documentate (3: normal, edge, failure)
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] Cortex metadata: rule_id, type: procedure_artifact, has_enforcement_loop: true, forge_version: "1.4"
- [x] §4b note pentru prompturi in productie

---

## Changelog

| Versiune | Data | Modificari |
|---------|------|-----------|
| 1.0 | 2026-03-06 | Versiune initiala. Sursa: PE INSIGHT EPR 16/20, Reddit r/PromptEngineering (64+165 upvotes), PR-019, ND-011. |
