---
type: procedure
created: 2026-03-17
status: active
slug: superdelphi-refine-bridge
tags: [procedure, nexus]
---

# Nexus Research ↔ INSIGHT REFINE Bridge

> **Naming 2026-03-07**: "SuperDelphi" = `superdelphi.py` — scriptul din **Nexus Research** (ex-Delphi project).
> Umbrella: **Nexus OS**. Referințele la "SuperDelphi" din acest document = referințe la scriptul `superdelphi.py`, nu la produsul Delphi.

**Status**: ACTIVE
**Creat**: 2026-02-27 | **Updated**: 2026-03-07
**Versiune**: 2.3

> **Document structure**: §1-5 = Academic pipeline (superdelphi.py → INSIGHT REFINE). §6-10 = EPR pipeline (External Practical Research, tool/product investigation). Citește §6.1 pentru a determina care pipeline se aplică topic-ului tău.
**Regulă asociată**: RESEARCH-S-001 (Standard — rules-standard.md), RESEARCH-S-002 (Standard — rules-standard.md)
**Scope**: Pipeline complet SuperDelphi → Cortex → INSIGHT REFINE + **External Practical Research (EPR)** pentru topics non-academice (tool investigation, product evaluation, integration research, market research)

---

## 1. Cele Două Sisteme de Self-Grading — Nu Sunt Incompatibile

Există două sisteme de scoring, fiecare măsoară ceva diferit:

| Sistem | Ce măsoară | Când rulează | Output |
|--------|-----------|--------------|--------|
| **SuperDelphi `self_grade`** | Calitatea research-ului (coverage, diversity, efficiency) | La finalul fiecărui SuperDelphi run | Integer 0-100 |
| **INSIGHT REFINE Phase 3.5** | Calitatea sintezei INSIGHT (5 dimensiuni umane × 20 pts) | După INSIGHT REFINE (Opus) | Score 0-100 per dimensiune |

**Concluzie**: Sunt **sequential**, nu competing. SuperDelphi gradează INPUT-ul (cercetarea). INSIGHT REFINE Phase 3.5 gradează OUTPUT-ul (sinteza).

---

## 2. Rezolvare — Sequential Gate Model

```
SuperDelphi run
    ↓
self_grade calculat (0-100)
    ↓
┌─────────────────────────────────────────────────────────┐
│  PRE-INSIGHT GATE (SuperDelphi self_grade)               │
│                                                         │
│  self_grade < 60  → INSUFFICIENT RESEARCH               │
│    → Re-run SuperDelphi cu depth D3/D4 sau mai multe    │
│      iterații. NU continua la INSIGHT.                  │
│                                                         │
│  self_grade 60-79 → PARTIAL COVERAGE                    │
│    → Continuă la INSIGHT REFINE (Opus)                 │
│    → Flag în output: "partial_coverage: true"           │
│    → Opus va nota lacunele în synthesis                 │
│                                                         │
│  self_grade ≥ 80  → GOOD RESEARCH                       │
│    → Full INSIGHT REFINE (Opus) fără constrângeri      │
└─────────────────────────────────────────────────────────┘
    ↓
INSIGHT REFINE — Opus citește TOT
    ↓
┌─────────────────────────────────────────────────────────┐
│  POST-INSIGHT GRADING (REFINE Phase 3.5)               │
│                                                         │
│  Model: **Sonnet** (nu Opus — Opus a făcut deja INSIGHT)│
│  5 dimensiuni × 20 pts fiecare = 100 total:            │
│  1. Coverage (cât din materialul ingerat a fost acoperit)│
│  2. Insight Quality (profunzimea analizei)              │
│  3. Actionability (cât de acționabile sunt outputs)    │
│  4. Novelty (idei noi vs ce știam deja)                 │
│  5. Integration (legăturile cross-domain detectate)    │
│                                                         │
│  Rating:                                                │
│  85-100 = EXCELLENT | 70-84 = GOOD                      │
│  50-69 = NEEDS WORK | <50 = RE-RUN INSIGHT              │
└─────────────────────────────────────────────────────────┘
```

**Mapare numerică între sisteme**:
```
SuperDelphi self_grade → interpretare:
  ≥80 = research bun → INSIGHT va găsi suficient material
  60-79 = research parțial → INSIGHT posibil incomplet, va nota lacunele
  <60 = research insuficient → nu rula INSIGHT, pierzi tokens Opus

REFINE Phase 3.5 score → interpretare:
  ≥85 = ciclu complet → pot trece la STANDARDIZE (Phase 4)
  <70 = INSIGHT insuficient → re-run cu mai mult material ingerat
```

---

## 3. REFINE Phase 3.5 — Model Specificat

**Model pentru Phase 3.5 (Self-Grading după INSIGHT REFINE)**:

```
Model: claude-sonnet-4-6 (Sonnet)
```

**Justificare**:
- Opus a rulat deja Phase 3 (INSIGHT REFINE) — cost ~$2-5/run
- Phase 3.5 e evaluare/scoring, nu sinteză — Sonnet e suficient
- Sonnet are context complet (INSIGHT output) și poate evalua calitativ
- Cost: Sonnet ~10% din Opus pentru același nr. de tokens

**Prompt pentru Phase 3.5 Sonnet**:
```
You are a research quality evaluator for the Nexus OS INSIGHT REFINE pipeline.
Your job is to score a synthesis output against the original ingested material — honestly and critically.
Do NOT inflate scores. A score of 15/20 means genuinely good, not average.

INSIGHT output to evaluate:
---
{full_insight_refine_output}
---

Material ingested (titles/summaries):
---
{ingested_material_list}
---

Score these 5 dimensions (0-20 each). For each dimension, provide:
- Score (integer 0-20)
- 1-2 sentence justification with specific examples from the output
- One concrete improvement suggestion

DIMENSIONS:
1. COVERAGE (0-20): What % of ingested material was referenced in the synthesis? Name any major sources that were ignored.
2. INSIGHT QUALITY (0-20): Depth of analysis — surface observations or genuine insights? Quote the best and worst insight.
3. ACTIONABILITY (0-20): Are PROCEDURE/SKILL/PLUGIN/INFO outputs concrete and usable? Could someone execute them without further research?
4. NOVELTY (0-20): New connections and ideas vs. just summarizing what was ingested? Identify any original cross-references.
5. INTEGRATION (0-20): Cross-domain links found? Connections across different areas? Name specific domains linked.

OUTPUT FORMAT — respond in exactly this JSON structure:
{
  "dimensions": [
    {"name": "COVERAGE", "score": N, "justification": "...", "improvement": "..."},
    {"name": "INSIGHT_QUALITY", "score": N, "justification": "...", "improvement": "..."},
    {"name": "ACTIONABILITY", "score": N, "justification": "...", "improvement": "..."},
    {"name": "NOVELTY", "score": N, "justification": "...", "improvement": "..."},
    {"name": "INTEGRATION", "score": N, "justification": "...", "improvement": "..."}
  ],
  "total": N,
  "rating": "EXCELLENT|GOOD|NEEDS_WORK|RE_RUN",
  "top_gaps": ["gap1", "gap2", "gap3"],
  "one_line_verdict": "..."
}

RATING THRESHOLDS: 85-100=EXCELLENT | 70-84=GOOD | 50-69=NEEDS_WORK | <50=RE_RUN
Include top_gaps only if total < 85. Max output: 600 tokens.
```

---

## 4. Pipeline Complet: SuperDelphi → Cortex → INSIGHT REFINE

```
1. SuperDelphi run (superdelphi.py)
   Input: query, domain, depth (D1-D4)
   Output: JSON {results, coverage, self_grade, iterations}

2. Cortex Store — results din SuperDelphi
   Collection: self-improve-{topic}
   curl -X POST http://localhost:6400/api/store \
     -d '{
       "text": "[QUERY]\n{query}\n\n[SYNTHESIS]\n{fusion_result}\n\n[KEY FINDINGS]\n{top_results}",
       "collection": "self-improve-{topic}",
       "metadata": {
         "type": "superdelphi_run",
         "self_grade": {N},
         "depth": "D2",
         "query": "{query}",
         "source_count": {N},
         "iterations": {N},
         "source_agent": "superdelphi",
         "genie_visible": true
       }
     }'

3. Pre-INSIGHT Gate
   if self_grade < 60 → stop, re-run SuperDelphi
   if self_grade 60-79 → continue with partial_coverage flag
   if self_grade ≥ 80 → continue normally

4. INSIGHT REFINE — Opus subagent
   Input: Reads ALL from Cortex self-improve-{topic}
   Process: Full synthesis → categorize as PROCEDURE/SKILL/PLUGIN/INFO
   Output: Structured list with category tags

5. Cortex Store — INSIGHT REFINE output
   Collection: procedures (for PROCEDURE/SKILL/PLUGIN outputs)
             + self-improve-{topic} (full INSIGHT output for reference)
   curl -X POST http://localhost:6400/api/store \
     -d '{
       "text": "[INSIGHT CASCADE]\n{full_opus_output}",
       "collection": "procedures",
       "metadata": {
         "type": "insight_refine",
         "topic": "{topic}",
         "cascade_cycle": {N},
         "source_agent": "opus",
         "items_procedure": {N},
         "items_skill": {N},
         "items_plugin": {N},
         "items_info": {N},
         "genie_visible": true
       }
     }'

6. Phase 3.5 — Sonnet Self-Grading
   Input: INSIGHT REFINE output + list of ingested material
   Output: Score 0-100, rating, gaps
   Store in Cortex: collection procedures, type: refine_phase35_grade

7. IF score ≥ 70 → continue to Phase 4 (STANDARDIZE)
   IF score < 70 → ingest more material → re-run INSIGHT REFINE
```

---

## 5. Enforcement

**CINE verifică**:
- La fiecare REFINE ciclu, Genie verifică că `self_grade` e ≥60 înainte de INSIGHT
- Phase 3.5 se rulează OBLIGATORIU după fiecare INSIGHT REFINE
- Rezultatele se stochează în Cortex (nu se pierd între sesiuni)

**CONNECTS TO**:
- `superdelphi.py` — produce self_grade
- `~/.nexus/procedures/ENRICH-PROCEDURE.md` — enrichment individual post (separat de INSIGHT REFINE)
- REFINE v2.1 procedures (în Cortex: slug `refine-v2.1-procedure`)
- `~/.nexus/delphi/delphi_description.md` — overview SuperDelphi + pipeline
- **EPR Code** (în `~/repos/delphi/`): `epr.py`, `epr_depth.py`, `epr_sources.py`, `epr_verify.py`, `research.py`

---

## 6. Clasificare Research Type (v2.0)

SuperDelphi §1-§5 acoperă **Research Academic/Tehnic** (papers, ArXiv, DAG queries, fusion). Secțiunile §6-§10 extind pipeline-ul cu **External Practical Research (EPR)** — un tip de research complet diferit, cu reguli proprii.

### 6.1 Când se activează EPR vs. SuperDelphi Academic

| Semnal în query | Tip | Pipeline |
|-----------------|-----|----------|
| "cum funcționează X", "ce features are X" | **EPR — Tool Investigation** | §7 EPR Pipeline |
| "cum integrez X cu Y" | **EPR — Integration Research** | §7 EPR Pipeline |
| "ce opțiuni există pentru X", "competitor analysis" | **EPR — Market/Product** | §7 EPR Pipeline |
| "ce zice literatura despre X", "studii despre X" | **Academic** | §1-§5 SuperDelphi |
| "overview X domain" (broad, deep) | **Academic** | §1-§5 SuperDelphi |
| Mix: "cum funcționează X și ce zice research-ul" | **EPR + Academic** | §7 apoi §4 (sequential) |

**Regulă de clasificare**: Dacă răspunsul corect implică *instalare, configurare, API calls, CLI commands sau evaluare produs* → EPR. Dacă implică *sinteză de cunoștințe, analiza literaturii, gap analysis* → Academic.

### 6.2 De ce EPR e diferit

SuperDelphi Academic:
- Surse: papers, docs, repos → **fusionare** → insight
- Output: sinteză, insight, recomandare
- Verificare: cross-referencing între papers, consensus

EPR:
- Surse: documentație oficială, GitHub, release notes, community → **verificare factuală**
- Output: capability matrix, install steps, integration architecture
- Verificare: **testare directă** (comanda funcționează? API-ul returnează ce zice docs-ul?)

**Diferența fundamentală**: Academic = sinteză (combine knowledge). EPR = verificare (is this claim true?).

---

## 7. EPR Pipeline — External Practical Research

### 7.0 Depth Classification (ca Academic)

| Depth | Scope | Query count | Surse min (total) | Min T1 | Min adaptoare unice | Budget tokens |
|-------|-------|-------------|-------------------|--------|---------------------|---------------|
| **E1 Quick** | Un singur tool, fact check rapid | 1-2 | 1 | 1 | 1 | 5K |
| **E2 Standard** | Tool investigation complet | 3-6 | 4 (2T1+1T2+1T3) | 2 | 3 | 30K |
| **E3 Deep** | Comparație tools / integration design | 6-12 | 7 (3T1+2T2+2T3) | 3 | 5 | 80K |
| **E4 Exhaustive** | Market survey / full ecosystem map | 12-20 | 10 (4T1+3T2+3T3) | 4 | 8 | 150K |

### 7.0.1 Execution Mode (Obligatoriu — fix C1 FORGE-AUDIT 2026-03-01)

| Depth | Execution | Comandă |
|-------|-----------|---------|
| E1 Quick | Genie direct | WebFetch manual pe T1 URL oficial → skip la §7.3 Step 8 |
| E2 Standard | Script automat | `python3 ~/repos/delphi/research.py "{query}" --type epr --depth E2` |
| E3 Deep | Script automat | `python3 ~/repos/delphi/research.py "{query}" --type epr --depth E3` |
| E4 Exhaustive | Script automat | `python3 ~/repos/delphi/research.py "{query}" --type epr --depth E4` |

**Cu URL oficial cunoscut** (recomandat E2+):
```
python3 ~/repos/delphi/research.py "{query}" --type epr --depth E{N} --official-url "{T1 URL}"
```

**⛔ INTERDICȚIE E2+**: Nu folosi WebFetch/WebSearch manual ca înlocuitor pentru script.

**Fallback**: Dacă scriptul lipsește sau eșuează → marchează `⚠️ MANUAL EPR FALLBACK` + documentează motivul → urmează §7.3 Steps 1-9 manual.

**Subagent delegation** (§8): Doar când Delphi nu e instalat pe mașina curentă (ex: VPS fără `~/repos/delphi/`).

### 7.1 Query Plan (obligatoriu E2+)

**ÎNAINTE de a lansa orice search**, construiește un Query Plan explicit:

```
📋 [EPR QUERY PLAN] depth: E{N} | topic: "{topic}"
├── Q1: "{query}" → target: {official docs / GitHub / community}
├── Q2: "{query}" → target: {release notes / changelog}
├── Q3: "{query}" → target: {community experience / issues}
├── ...
└── QN: "{verification query}" → target: {cross-check a critical claim}
```

**Reguli Query Plan:**
1. **Prima query = ÎNTOTDEAUNA documentația oficială** (site-ul producătorului, GitHub readme, API docs)
2. **A doua query = GitHub issues/discussions** (realitatea vs marketing)
3. **A treia query = community experience** (Reddit, HN, Stack Overflow, blogs tehnice)
4. **Ultima query = cross-check** pe claim-ul cel mai important

### 7.2 Source Requirements (mai stricte decât Academic)

| E-depth | T1 (docs oficiale) | T2 (repos, papers) | T3+ (community) | T1 OBLIGATORIU |
|---------|--------------------|--------------------|------------------|----------------|
| E1 | ≥1 | - | - | DA |
| E2 | ≥2 | ≥1 | ≥1 | DA |
| E3 | ≥3 | ≥2 | ≥2 | DA |
| E4 | ≥4 | ≥3 | ≥3 | DA |

**T1 e ÎNTOTDEAUNA obligatoriu în EPR. Zero excepții.** Dacă un tool nu are documentație oficială, marchează explicit: `⚠️ No T1 source found — all claims UNVERIFIED`.

**Clasificare surse EPR** (implementat în `epr_sources.py`):
- **T1**: Official docs (site producător, GitHub README oficial, API reference). **Cortex NU e T1** — e T2 (internal KB, poate conține date stale/hallucinate din run-uri anterioare).
- **T2**: GitHub repos/issues, ArXiv, Wikipedia, StackOverflow, Perplexity, OpenRouter (AI synthesis), Semantic Scholar (papers), Cortex (internal KB)
- **T3+**: Reddit, HackerNews, Brave Search, DuckDuckGo

**T1 gate enforcement**: Dacă la finalul EPR pipeline `t1_count < config.min_t1`:
- E1/E2: **STOP** — nu genera raport, emite warning
- E3/E4: genera raport cu `⚠️ INSUFFICIENT T1` watermark

### 7.3 Execution Protocol

```
0. INVOKE DELPHI EPR ENGINE (E2+ — §7.0.1 OBLIGATORIU)
   → E1 Quick: skip (execută manual Steps 1-9)
   → E2+: python3 ~/repos/delphi/research.py "{query}" --type epr --depth E{N} [--official-url "{url}"]
   → Dacă script disponibil: Steps 1-9 se execută AUTOMAT — nu replica manual cu WebFetch/WebSearch
   → Dacă script indisponibil: marchează ⚠️ MANUAL EPR FALLBACK, documentează motivul, continuă Steps 1-9

1. CORTEX CHECK (identic cu §I.1 din WISH Pipeline)
   → Avem deja research pe acest topic? Score ≥ 0.7 → start de acolo.

2. QUERY PLAN (§7.1)
   → Scrie planul ÎNAINTE de orice search. VK obligatoriu.

3. FETCH T1 — Documentație oficială
   → WebFetch pe URL-ul oficial (docs, GitHub README, API reference)
   → Extrage: versiune, features, limitations, auth method, pricing
   → MARCHEAZĂ: [T1 VERIFIED] sau [T1 UNAVAILABLE]
   → FALLBACK dacă T1 UNAVAILABLE:
     a) Retry cu URL alternativ (ex: GitHub README dacă docs site e down)
     b) Dacă retry fail → continue pipeline cu T2/T3 dar marchează TOTUL ca [UNVERIFIED]
     c) Gate check la final: dacă t1_count < min_t1, E1/E2 → STOP, E3/E4 → watermark ⚠️

4. FETCH T2 — GitHub/repos
   → Caută: issues (broken/fixed), PRs recente, stars/activity
   → Extrage: known issues, workarounds, community health
   → MARCHEAZĂ: [T2 VERIFIED]

5. FETCH T3+ — Community
   → WebSearch pe Reddit, HN, Stack Overflow, blogs
   → Extrage: real-world experience, gotchas, alternatives
   → MARCHEAZĂ: [T3 UNVERIFIED] — community claims need cross-check

6. CROSS-CHECK CRITICAL CLAIMS
   → Fiecare claim CRITICAL (ex: "supports headless mode", "OAuth tokens auto-refresh")
     trebuie confirmat din ≥2 surse independente
   → Dacă o singură sursă → marchează [SINGLE SOURCE — UNVERIFIED]

7. VERIFY BY TESTING (când posibil)
   → Dacă tool-ul e instalabil: `brew install X`, `npm install X` → test direct
   → Dacă are CLI: rulează comanda și confirmă output
   → Dacă are API: fă un test call
   → MARCHEAZĂ: [TESTED ✅] sau [NOT TESTED — rely on docs]

8. SYNTHESIZE
   → Output structurat (§7.4)

9. CORTEX STORE (obligatoriu)
   → Store EPR report in Cortex:
     collection: "epr-{topic}" (ex: epr-gemini-cli)
     metadata: type=epr_report, depth=E{N}, epr_score={score}, query="{query}",
               t1_count={N}, verified_pct={N}%, genie_visible=true
   → Permite future research să găsească EPR reports existente (Step 1 Cortex Check)
```

### 7.4 Output Format EPR (obligatoriu)

```markdown
## EPR Report: {Topic}
**Depth**: E{N} | **Date**: {date} | **Sources**: {N} ({T1}×T1, {T2}×T2, {T3+}×T3+)

### Capability Matrix
| Feature | Status | Source | Verified |
|---------|--------|--------|----------|
| Feature A | ✅ Available | [T1] Official docs | TESTED ✅ |
| Feature B | ❌ Not available | [T1] Official docs | VERIFIED |
| Feature C | ⚠️ Claimed | [T3] Reddit user | SINGLE SOURCE |
| Feature D | ❓ Unknown | No source found | UNVERIFIED |

### Install / Setup
[Verified steps — only TESTED commands]

### Integration Architecture
[How it connects to Nexus/existing systems]

### Limitations & Gotchas
[Cross-checked from T2+T3 sources]

### Recommendation
[Clear decision: USE / DON'T USE / CONDITIONAL + justification]

### Sources
[Numbered list with tier, URL, date accessed]
```

### 7.5 Anti-Hallucination Protocol (OBLIGATORIU)

**Problema reală** (incident 2026-03-01): Research Gemini CLI delegat la subagent → raport cu date specifice (model IDs, API endpoints, pricing) → neverificate → user nu poate ști ce e real vs hallucinated.

**Reguli anti-hallucination:**

1. **Fiecare claim factuală TREBUIE să aibă source + tier**: `"Gemini CLI supports headless mode" [T1: github.com/google-gemini/gemini-cli README]`
2. **Niciun model ID, endpoint URL, sau preț fără sursă T1**: Dacă nu găsești în docs oficiale → `[UNVERIFIED]`
3. **Subagent output = UNVERIFIED by default**: Genie sau Opus TREBUIE să verifice claims critice din output-ul subagentului
4. **Test > Docs > Community**: O comandă testată local bate documentația. Documentația bate un comentariu Reddit.
5. **"Nu știu" > guess**: Dacă nu poți verifica → scrie explicit `Unknown — no T1/T2 source found`

**Enforcement mecanism** (nu doar aspirational):
- `epr_verify.py` → `cross_check_claims()` face keyword matching automat pe claims critice
- Genie/Opus verificare manuală (post-delegation §8.3) = re-fetch TOP 3 T1 URLs citate → confirmare content match
- Dacă re-fetch nu confirmă → marchează `[REFUTED]` și scade EPR Verification Score
- **Limită automatizare**: heuristic keyword matching NU poate detecta hallucinations subtile. Verificarea manuală (§8.3) rămâne OBLIGATORIE pentru claims critice. Tool-ul automatizat e FIRST PASS, nu replacement.

---

## 8. EPR Agent Delegation Protocol

### 8.1 Când delegi EPR la subagent

| Situație | Acțiune |
|----------|---------|
| E1 Quick (1-2 queries) | Genie direct — NU delega |
| E2 Standard | Subagent Haiku sau Sonnet (nu Opus) |
| E3+ Deep | Subagent Opus — verificare critică |
| Orice cu install/test | Genie direct sau Codex — necesită acces la mașină |

### 8.2 Prompt obligatoriu pentru subagent EPR

Fiecare subagent EPR primește acest prefix obligatoriu:

```
You are a senior research analyst conducting External Practical Research (EPR) on {domain}.
Your task is to produce a structured EPR report that separates verified facts from unverified claims.

RULES:
1. EVERY factual claim must have a source URL and tier [T1/T2/T3+]
2. Mark claims without T1 source as [UNVERIFIED]
3. NEVER invent model IDs, API endpoints, prices, or version numbers
4. If you cannot verify something, write "Unknown — not found in official docs"
5. Use WebSearch and WebFetch — do not rely on training data for current facts
6. Prefer T1 (official docs) over T2 (repos/papers) over T3+ (community)

OUTPUT FORMAT — use this exact structure:

## EXECUTIVE SUMMARY
2-3 sentences: what was researched, key conclusion, confidence level.

## CAPABILITY MATRIX
| Feature | Status | Source | Verified |
|---------|--------|--------|----------|
(one row per capability — use ✅/❌/⚠️/❓ for status)

## KEY FINDINGS
Numbered list. Each finding = one claim + [T1/T2/T3+] source tag + URL.

## LIMITATIONS & GOTCHAS
Bullet points. Cross-checked from T2+T3 sources.

## EVIDENCE GAPS
What could NOT be verified. What needs testing.

## SOURCES
Numbered list: [T1/T2/T3+] URL — date accessed — what was extracted.

CONSTRAINTS:
- Cite specific sources for every claim. Do not speculate beyond evidence.
- If a claim appears in only one source, mark it [SINGLE SOURCE — UNVERIFIED].
- Total output: 500-1500 words depending on depth.
```

### 8.3 Post-Delegation Verification (OBLIGATORIU)

După ce subagentul returnează:

```
1. Verifică claim-urile CRITICE (feature availability, auth method, pricing):
   → Re-fetch URL-ul T1 citat → confirmă că spune ce zice raportul
   → Dacă URL-ul nu există sau spune altceva → marchează [REFUTED]

2. Contorizează verified vs unverified:
   → ≥80% verified claims → raportul e utilizabil
   → <80% → re-run cu queries mai specifice

3. Test local (când posibil):
   → Install tool-ul (dacă e safe) și verifică 1-2 claims critice
   → Adaugă [TESTED ✅] pe claims confirmate
```

---

## 9. EPR Gate Integration cu SuperDelphi Pipeline

EPR nu înlocuiește SuperDelphi academic — îl completează. Integrarea:

```
Genie primește research task
    ↓
§6.1 Clasificare: EPR / Academic / Mix?
    ↓
┌─────────────────────┬──────────────────────────┐
│                     │                          │
│  ACADEMIC           │  EPR                     │
│  §1-§5 Pipeline     │  §7 EPR Pipeline         │
│  superdelphi.py     │  Genie direct / subagent │
│  DAG + Fusion       │  Query Plan + Verify     │
│                     │                          │
└────────┬────────────┴──────────┬───────────────┘
         │                       │
         └───────────┬───────────┘
                     ↓
            Cortex Store (AMBELE)
                     ↓
            INSIGHT REFINE (Opus)
            dacă ambele pipeline-uri
            au produs material suficient
                     ↓
            Phase 3.5 Self-Grading
            + EPR Verification Score (§9.1)
```

### 9.1 EPR Verification Score (SEPARAT de Phase 3.5)

Când research-ul include EPR, se calculează un **scor de verificare separat** (NU o dimensiune extra la Phase 3.5). Phase 3.5 rămâne 5 dimensiuni × 20 pts = 100 total (identic cu Academic).

**EPR Verification Score** (calculat de `epr_verify.py`, tracked separat):
```
VERIFICATION (0-20): Ce % din claims sunt verified ([T1]+[TESTED])?
   - 18-20: ≥90% verified, multiple claims tested local
   - 14-17: 70-89% verified, T1 sources pe claims critice
   - 10-13: 50-69% verified, some T1 gaps
   - 0-9: <50% verified, research unreliable
```

**Raportare**: Phase 3.5 score (0-100) + EPR Verification Score (0-20) = raportate separat.
- Phase 3.5: 85-100 EXCELLENT | 70-84 GOOD | 50-69 NEEDS WORK | <50 RE-RUN (neschimbat)
- EPR Verification: ≥14 GOOD | 10-13 PARTIAL | <10 UNRELIABLE

**Gate**: Dacă EPR Verification < 10 → research-ul NU poate trece la INSIGHT REFINE, indiferent de Phase 3.5 score.

**Interacțiunea celor două gate-uri** (H5 clarification):
- Gate-ul `self_grade < 60` (§2) se aplică ACADEMIC pipeline (`superdelphi.py` runs).
- Gate-ul `EPR Verification < 10` (§9.1) se aplică EPR pipeline (`epr.py` runs).
- **Nu se suprapun**: dacă un topic e clasificat EPR (§6.1), gate-ul self_grade NU se mai verifică — EPR Verification Score e singurul gate activ.
- **Eroare infrastructure**: dacă `epr_verify.py` nu e disponibil → marchează EPR Verification = `UNKNOWN`, continuă cu warning `[UNVERIFIED-EPR]`, nu blocare.

---

## 10. Enforcement Loop EPR (FORGE META-H-002)

### WHERE
- La FIECARE research task clasificat EPR (§6.1)
- La FIECARE delegare research la subagent
- La FIECARE output de research prezentat userului

### WHEN
- **Pre-execution**: Query Plan (§7.1) obligatoriu E2+ — VK: `📋 [EPR QUERY PLAN] ...`
- **During execution**: T1 source check pe fiecare claim critical
- **Post-execution**: Verification count (§8.3) obligatoriu
- **Post-delegation**: Subagent output verification (§8.3) obligatoriu

### HOW (violation detection)
- Research EPR fără Query Plan → violation (RESEARCH-S-002)
- Research EPR fără sursă T1 → violation (RESEARCH-S-002)
- Claim factual fără source tag [T1/T2/T3+/UNVERIFIED] → violation
- Subagent output prezentat userului fără verification pass → violation
- Model ID / API endpoint / preț fără T1 source → violation

### Error Recovery (infrastructure failures)
- **Cortex unavailable**: skip store (Steps 2+5) → log warning, continuă pipeline fără persistență. Re-încearcă store la finalul sesiunii.
- **Opus API timeout** (INSIGHT REFINE): retry 1x după 30s. Dacă al doilea timeout → notifică Pafi, livrează research brut (Cortex results fără INSIGHT).
- **superdelphi.py lipsă/crash**: oprește Academic pipeline, interoghează direct Cortex (`cortex search --query "$topic"`) ca fallback. Notifică Pafi.
- **epr_verify.py indisponibil**: marchează `EPR_VERIFICATION=UNKNOWN`, continuă cu `[UNVERIFIED-EPR]` watermark.
- **cascade_cycle increment la retry**: la re-run pe același topic (self_grade < 60 → retry), `cascade_cycle` se incrementează automat în Step 2 metadata. Nu se creează duplicate — session distinctă per cycle number.

### VERIFY
- [ ] E2+: `research.py` invocat (nu WebFetch manual)? Sau `⚠️ MANUAL EPR FALLBACK` documentat?
- [ ] Min adaptoare unice atinse? (E2=3, E3=5, E4=8)
- [ ] Query Plan emis ca VK înainte de prima căutare
- [ ] Output respectă format EPR (§7.4) cu Capability Matrix
- [ ] Claims critice au ≥2 surse sau [TESTED ✅]
- [ ] Subagent output a trecut verification (≥80% claims verified)
- [ ] Cortex store cu metadata `type: epr_report`

---

## 11. Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-02-27 | Initial: SuperDelphi ↔ INSIGHT REFINE bridge, sequential gate model, Phase 3.5 scoring |
| 2.0 | 2026-03-01 | **MAJOR**: Added §6-§10 External Practical Research (EPR). New research type classification (§6), full EPR pipeline with Query Plan + Source Requirements + Verification Protocol (§7), Anti-Hallucination Protocol (§7.5), Agent Delegation rules (§8), EPR-Academic gate integration (§9), EPR Verification Score dimension for Phase 3.5 (§9.1), FORGE enforcement loop (§10). Triggered by: unverified Gemini CLI research incident. |
| 2.1 | 2026-03-01 | **FORGE-AUDIT fixes** (Opus audit score 2.50→target 3.0+): (C1) Rules RESEARCH-S-001/002 need formal definition in rules-standard.md. (C2) Phase 3.5 scoring kept at 100pts, EPR Verification Score tracked SEPARATELY (0-20). (H1) E1 min_sources fixed 2→1 to match §7.2. (H2) T1 gate enforcement added. (H3) Anti-Hallucination enforcement mechanism clarified (automated=first pass, manual=obligatory). (H4) Cortex reclassified T2 not T1 in source tiers. (M1) T1 fallback handling added to §7.3. (M6) EPR code file paths added to §5. Added Cortex store step (Step 9) to EPR pipeline. |
| 2.2 | 2026-03-01 | **FORGE-AUDIT DEEP fixes** (Opus audit score 2.475/4.0 CONDITIONAL → target PASS): (C1) §7.0.1 Execution Mode ADĂUGAT — mandatează `research.py` E2+, interzice WebFetch manual, definește fallback. §7.3 Step 0 adăugat. §10 VERIFY +2 checks (script invocat, adapter diversity). Rezolvă root cause incident Gemini. (C2) §7.0 tabel: coloană "Min adaptoare unice" (E2=3, E3=5, E4=8). (H1) §7.2: OpenRouter + Semantic Scholar adăugate ca T2. (H2/H3/domain-exclusions) → Codex m4-241 (expand build_epr_query_plan, router integration, DOMAIN_EXCLUDED_SOURCES fix). |

---

## Checklist Pre-Publicare (FORGE META-H-002)

- [x] Regulă asociată: RESEARCH-S-001 + RESEARCH-S-002
- [x] Enforcement loop complet: WHERE + WHEN + HOW + VERIFY (§10)
- [x] Pipeline clar per research type: Academic (§1-§5) + EPR (§7)
- [x] Research type classification definit (§6)
- [x] Source requirements per depth (§7.2)
- [x] Output format standardizat (§7.4)
- [x] Anti-hallucination protocol (§7.5)
- [x] Agent delegation rules (§8)
- [x] Gate integration Academic ↔ EPR (§9)
- [x] Verification scoring (§9.1)
- [x] Incident reference (2026-03-01 Gemini CLI)
