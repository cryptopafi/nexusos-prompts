---
type: procedure
created: 2026-03-17
status: active
slug: source-selection-framework
tags: [procedure, nexus]
---

# SOURCE-SELECTION-FRAMEWORK — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-02-28
**Versiune**: 1.0
**Regulă asociată**: REASON-H-001 (DARE Research Reasoning)
**Scope**: Selecția, prioritizarea și evaluarea surselor în orice research D2+.

---

## 1. Problema

Fără un framework formalizat de selecție a surselor:
- Research-ul tinde să consume primele N rezultate din search, indiferent de calitate
- Surse cu authority scăzut (YouTube transcripts, Reddit anecdote) primesc aceeași greutate ca documentația oficială
- Source routing-ul (I.3) și authority scoring-ul (I.4) din WISH pipeline sunt fragmentate — nu există o viziune unificată
- Ciclu 1 Prompting a identificat discrepanțe mari de valoare: Anthropic docs (Tier 1) vs YouTube (Tier 5) — dar asta nu era formalizat
- Riscul de "garbage in, garbage out": prompt-ul de research e bun (PromptForge FULL), dar sursele sunt slabe

Situații acoperite:
- Research D2+ (DARE obligatoriu) — orice subiect
- Delphi queries cu surse externe
- ECHELON channel research (curs evaluation, platform comparison)
- Radar source scoring (ce intră în monitoring)

---

## 2. Procedura

### Pas 1: CLASSIFY RESEARCH TYPE
Înainte de a căuta surse, clasifică tipul de research:

| Tip Research | Ce cauți | Surse prioritare |
|-------------|---------|-----------------|
| **Technical** | Cum funcționează X, implementare, API, config | Docs oficiale → GitHub repos → ArXiv |
| **Strategic** | Decizie arhitecturală, tradeoffs, comparații | ArXiv/papers → docs oficiale → GitHub issues → comunități |
| **Market/Trends** | Ce se întâmplă în domeniu, ce e nou | Comunități → Brave/Tavily → YouTube → blogs |
| **Audit/Security** | Vulnerabilități, compliance, best practices | Docs oficiale → CVE databases → ArXiv → GitHub security advisories |
| **Learning** | Curs, tutorial, skill acquisition | Docs oficiale → tutorial repos interactiv → cursuri structured → video |

### Pas 2: APPLY SOURCE TIERS
Evaluează fiecare sursă găsită pe 5 tiers:

| Tier | Tip sursă | Authority | Caracteristici | Exemple |
|------|----------|-----------|---------------|---------|
| **T1** | Documentație oficială + tutorial-uri interactive | 1.0 | Actualizate de maintainer, verificabile, structured | Anthropic docs, MDN, PostgreSQL docs, anthropics/prompt-eng-interactive-tutorial |
| **T2** | Repo-uri cu implementări practice + papers peer-reviewed | 0.85 | Cod executabil, reproducibil, citat | NirDiamant repos, ArXiv papers, GitHub repos cu >100 stars + recent commits |
| **T3** | Survey papers + curated lists | 0.7 | Coverage largă, nu depth | "The Prompt Report", awesome-X lists, benchmark comparisons |
| **T4** | Comunități + blogs tehnice | 0.5 | Punctual valoroase, mult noise, need verificare | Reddit (r/LocalLLaMA, r/PromptEngineering), HackerNews, technical blogs |
| **T5** | Video content + generalist | 0.3 | Density informațional scăzut, consum timp mare | YouTube transcripts, podcasts, medium articles generice |
| **AVOID** | Spam / low-value | 0.0 | Zero valoare, risc de misinformation | "Make money with AI", cursuri generice paid fără reviews, SEO spam |

### Pas 3: SOURCE BUDGET PER DEPTH
Limitează numărul de surse per depth level:

| Depth | Total surse | Min T1-T2 | Max T4-T5 | T5 solo permis? |
|-------|------------|-----------|-----------|-----------------|
| D1 Quick | 1-2 | 1 | 0 | Nu |
| D2 Standard | 3-5 | 2 | 1 | Nu |
| D3 Deep | 5-8 | 3 | 2 | Da, max 1 |
| D4 Exhaustive | 8+ | 5 | 3 | Da, max 2 |

**Regula de aur**: Niciodată mai multe surse T4-T5 decât T1-T2.

### Pas 4: EVALUATE SOURCE QUALITY
Per sursă individuală, verifică rapid (10 sec/sursă):

| Check | Ce verifici | Red flag |
|-------|-----------|----------|
| **Recency** | Când a fost publicat/updatat? | >12 luni pe fast-moving topic (AI, crypto) |
| **Authority** | Cine a scris? Organizație sau individual? | Anonim, fără credențiale vizibile |
| **Reproducibility** | Pot verifica informația? Cod executabil? | "Trust me bro" fără date/cod |
| **Bias** | Are autorul un conflict de interese? | Vendor promovând propriul produs |
| **Depth** | Cât de specific e conținutul? | Rămâne la suprafață, fără detalii actionable |

Dacă ≥2 red flags pe o sursă → downgrade cu un tier sau exclude.

### Pas 5: APPLY TO WISH PIPELINE
Integrează cu sub-steps existente:
- **I.3 Search DAG**: Source routing folosește tier-urile pentru a prioritiza canalele
- **I.4 Cross-Source Fusion**: Authority scores din tabel-ul de tiers (nu valori ad-hoc)
- **I.5 Radar Extraction**: Doar surse T1-T3 intră în Radar (T4-T5 sunt efemere)

Log: `[SOURCES] D{N} | T1:{n} T2:{n} T3:{n} T4:{n} T5:{n} | budget: OK/OVER`

**Exemplu real** (research affiliate marketing SMS, 2026-03-01, D3 Deep):
```
[SOURCES] D3 | T1:2 T2:3 T3:1 T4:2 T5:0 | budget: OK
Top T1: callloop.com/blog/sms-best-practices (official SMS platform docs, 2025)
Top T2: businessofapps.com/marketplace/affiliate-networks (catalog peer-curated 74 networks, 2025)
T3: ROIads.com/tier-guide (survey T1-T3 comparison, verified cu date reale)
T4 limitat: reddit-like forum STM (comunitate, verificat cross cu T1-T2)
Exclus: "make money with SMS" YouTube videos (AVOID — zero credentiale, clickbait)
Concluzie: T1+T2 = 63% din surse → regula de aur respectată ✅
```

---

## 3. Cortex Logging

```json
{
  "text": "Source Selection: {research_topic} | D{N} | T1:{n} T2:{n} T3:{n} T4:{n} T5:{n} | top_source: {name} (T{tier})",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SOURCE-SELECTION-FRAMEWORK",
    "rule_id": "REASON-H-001",
    "tags": ["source-selection", "research", "dare"],
    "research_depth": "D1|D2|D3|D4",
    "source_tier_distribution": {"T1": 0, "T2": 0, "T3": 0, "T4": 0, "T5": 0},
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step I (sub-steps I.3, I.4, I.5) — la orice research D2+
- Delphi queries cu surse externe
- ECHELON channel research

### WHEN
- La FIECARE research D2+ — zero excepții
- Implicit prin DARE flow (D→A→R→E) — Source Selection se aplică la R (Retrieve)

### HOW (violation detection)
- Research D2+ fără log `[SOURCES]` → violation
- Mai multe surse T4-T5 decât T1-T2 → violation (regula de aur)
- Sursă T5 ca singura dovadă pentru o concluzie → violation
- Research output citează sursă fără tier classification → warning
- Runner: Genie self-check la DARE E (Evaluate) — verifică source distribution înainte de sinteză

### CONNECT
- REASON-H-001 → Source Selection e sub-procedură a DARE R phase
- WISH I.3 (Search DAG) → source routing aliniată cu tier priorities
- WISH I.4 (Cross-Source Fusion) → authority scores unificate cu tier values
- WISH I.5 (Radar Extraction) → doar T1-T3 în Radar
- `wish-pipeline.md` → referință la acest framework
- `procedure-health.json` → entry pentru SOURCE-SELECTION-FRAMEWORK

### VERIFY (procedural checkpoint — emis ca VK în sesiune)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii 1-5 din §2)
- [ ] Output-ul satisface criteriile din §1? (surse prioritizate corect, budget respectat)
- [ ] VK emis în sesiune? (ambele linii de mai jos vizibile pentru Pafi)
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per procedură FORGE** (per VK-H-001):
1. **Execution VK** (confirmă pașii urmați):
   `✅ [PROC] SOURCE-SELECTION-FRAMEWORK | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK** (confirmă salvarea în Cortex):
   `✅ [CORTEX] "Source Selection {topic}" | FORGE ✓ | rule: REASON-H-001 | v1.0`

### MODEL ROUTING (cine execută ce pe această procedură)
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Source evaluation la D2 | Genie direct (Sonnet) | Parte din WISH pipeline, necesită session context |
| Source evaluation la D3-D4 | Genie orchestrează, subagents gather | Subagents caută, Genie evaluează tier-urile |
| Radar extraction post-evaluation | Genie direct | Necesită tier classification din acest framework |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| WISH Pipeline I.3-I.5 | Sub-steps care consumă acest framework | `memory/wish-pipeline.md` |
| DARE framework | Research reasoning care invocă Source Selection | CLAUDE.md §WISH → §I.0 |
| Radar | Destinația surselor T1-T3 | Radar configs |
| Cortex KB | Source patterns storage | `http://localhost:6400/api/store` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| T1-T2 ratio | Calitatea surselor folosite | ≥ 60% din total surse |
| T4-T5 ratio | Noise în research | ≤ 30% din total surse |
| Source budget compliance | Research nu depășește bugetul de surse | 100% |
| Red flag detection | Surse excluse pe baza quality checks | Track per sesiune |

---

## Checklist Pre-Publicare (verifică ÎNAINTE de a marca ACTIVE)

- [x] Regulă asociată există în rules-hard.md (REASON-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu cele 3 checks obligatorii
- [x] Procesul din WHERE a fost editat să referențieze această procedură (WISH I.3-I.5)
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
