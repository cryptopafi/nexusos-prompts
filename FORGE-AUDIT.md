---
type: procedure
created: 2026-03-22
status: active
slug: forge-audit
tags: [procedure, nexus]
---

> **⚠️ DEPRECATED (2026-03-22)** — This procedure has been absorbed into **AUDIT-PRO** (`~/.nexus/audit/AUDIT-PRO.md`). Do not use. Use `/audit` command instead.

# FORGE-AUDIT v1.7 — Procedură Standard de Operare

**Status**: DEPRECATED | Absorbed into AUDIT-PRO v1.1 (`~/.nexus/audit/AUDIT-PRO.md`) on 2026-03-21. Use `/audit` instead.
**Creat**: 2026-02-28 | **Updated**: 2026-03-19
**Versiune**: 1.7
**Regulă asociată**: QUAL-H-004 (Verify-Before-Assert) + DEV-H-010 (Dual Audit)
**Scope**: Template universal de audit — aplicabil pe orice subiect (proceduri, cod, securitate, infrastructură, livrări Codex, configurații). Scalabil: audit simplu = 3 pași, audit profund = 6 pași.
**Research basis**: ISO 19011:2018 (3 dimensiuni audit), ISO 15504 SPICE (scala NPLF), CMMI v2.0 (maturity levels), FDA GMP SOP Checklist (PharmUni 2025), FAA AC 120-71B (aviation SOPs), NUREG-1358 (nuclear procedure completeness), IFEval (Zhou et al. Google 2023 — verifiable instructions), AgentBench (Liu et al. ICLR 2024), AdvancedIF (Meta 2025 — rubric decomposition), Osmani 2025 (AI agent spec quality).

---

## Ce este FORGE-AUDIT — ghid pentru utilizator

FORGE-AUDIT este un **checklist universal și scalabil** pe care Genie îl aplică de fiecare dată când auditează ceva. Funcționează pe orice subiect — de la o procedură la un server.

### De ce există

Fără template standardizat, fiecare audit e ad-hoc: criterii diferite, profunzime diferită, scoring inconsistent. Rezultatul: audituri incomplete, lipsă comparabilitate, gap-uri nedetectate.

### Cum funcționează

1. Genie clasifică auditul în unul din 3 niveluri (LIGHT / STANDARD / DEEP)
2. Fiecare nivel are un set fix de dimensiuni + checklist
3. Scoring-ul e uniform (scala NPLF: N/P/L/F)
4. Output-ul e un raport standardizat — comparabil între audituri

### Ce vei vedea

- `🔍 [AUDIT] tier: LIGHT/STANDARD/DEEP | subject: "X"` la start
- Checklist cu rezultate per dimensiune
- Scor final: `PASS` / `CONDITIONAL` / `FAIL`
- `✅ [AUDIT] subject: "X" | score: {medie}/4.0 | verdict: PASS` la final

---

## 1. Problema

### Ce nu funcționează fără un template de audit standardizat

1. **Audituri ad-hoc** — fiecare audit folosește criterii diferite, nu poți compara rezultatele
2. **Gap-uri nedetectate** — fără checklist fix, dimensiuni importante sunt omise (ex: safety, maintainability)
3. **Over-auditing pe lucruri simple** — un audit de 30 min pe un fix de 5 linii e waste
4. **Under-auditing pe lucruri complexe** — un audit superficial pe o schimbare de arhitectură e risc
5. **Scoring subiectiv** — fără scală definită, "PASS" înseamnă lucruri diferite de la audit la audit

### Situații acoperite

- Audit procedură (FORGE compliance + content quality)
- Audit cod (Codex delivery, PR review, security)
- Audit securitate (VPS, API exposure, access control)
- Audit infrastructură (services, daemons, configurații)
- Audit configurație (CLAUDE.md, rules, ECHELON setup)
- Orice alt subiect care necesită evaluare sistematică

---

## 2. Procedura

### Pas 1: CLASIFICĂ TIER-UL

Evaluează subiectul și alege tier-ul de audit:

| Tier | Când | Dimensiuni | Timp | Pași |
|------|------|-----------|------|-----|
| **LIGHT** | Fix simplu, schimbare minoră, config change, quick review | 3 | ~3 min | 1→2→6 |
| **STANDARD** | Procedură nouă, feature, Codex delivery, security change | 6 | ~15 min | 1→2→3→4→6 |
| **DEEP** | Arhitectură, production audit, HARD rule, security audit complet | 8 | ~30 min | 1→2→3→4→5→6 |

**Auto-escalation**: dacă LIGHT descoperă un **N** pe orice dimensiune → escalează la STANDARD.

**NEVER (interdicții auditor)**:
- NEVER skip dimensiuni din tier-ul selectat — evaluează TOATE, chiar dacă par "evident OK"
- NEVER override verdict manual — scorul numeric determină verdict-ul, fără excepții
- NEVER self-audit pe orice tier — FORGE-AUDIT se execută EXCLUSIV cu Opus subagent (toate tier-urile: LIGHT/STANDARD/DEEP)

Emit VK: `🔍 [AUDIT] tier: {TIER} | subject: "{subiect}"`

#### DSE Detection (Domain-Specific Extensions)

După clasificarea tier-ului, identifică **tipul de subiect** pentru a activa extensii suplimentare:

| DSE | Trigger (subiect conține) | Dimensiuni extra | Secțiune |
|-----|--------------------------|-----------------|----------|
| **DSE-RESEARCH** | Research engine, search pipeline, data aggregator, surse de date, scraper cu multiple surse | +4 (R1-R4) | 2b |
| **DSE-WORKFLOW** | Multi-agent workflow, handoff protocol, pipeline multi-step, daemon+queue system | +4 (W1-W4) | 2c |
| *niciun DSE* | Subiect generic, single-file, config, reguli | +0 | — |

**Reguli DSE:**
- **Multiple DSE-uri permise** dacă subiectul califică pentru mai mult de unul (ex: ECHELON = DSE-WORKFLOW + DSE-RESEARCH). Evaluează TOATE DSE-urile relevante.
- **CHECKLIST obligatoriu** (FM-5 prevention): după selectarea DSE-ului inițial, Genie MUST verifică explicit fiecare DSE din tabel: "Subiectul califică și pentru DSE-X?" Dacă da → adaugă-l. Fără acest check → violation.
- DSE se aplică pe ORICE tier (LIGHT/STANDARD/DEEP) — extensiile sunt ortogonale pe tier
- DSE dimensiunile se scorează separat (scala 1-5, nu NPLF) — specifice domeniului, nu generice
- Scorul final combină ambele: Core NPLF + DSE score (vezi Pas 6)

Emit VK: `🔍 [AUDIT] tier: {TIER} | dse: {DSE-NAME(s)|none} | subject: "{subiect}"`

### Pas 1b: BATCH MODE (multiple subiecte într-o sesiune)

Când auditezi **2+ subiecte** într-o sesiune (ex: multiple Codex deliveries, multiple proceduri):

#### Când se aplică batch mode

| Situație | Batch? | Motiv |
|----------|--------|-------|
| 2+ Codex deliveries din aceeași zi | DA | Frecvent, eficient |
| 2+ proceduri independente | DA | Economie timp |
| 1 subiect complex cu sub-componente | NU | E un singur audit STANDARD/DEEP |
| 2+ subiecte cu dependențe între ele | DA + cross-check | Trebuie verificat că interacționează corect |

#### Procedura batch

1. **Listează toate subiectele** cu tier-ul fiecăruia
2. **Auditează individual** — fiecare subiect primește propriul raport cu scor NPLF
3. **Cross-dependency check** — dacă subiectele au dependențe între ele:
   - Listează dependențele: "m4-159 creează forge.ts → m4-160 îl folosește"
   - Verifică compatibilitatea: output-ul unui subiect e consumat corect de altul?
   - Emit VK: `🔗 [AUDIT-BATCH] cross-deps: {N} checked, {N} OK, {N} issues`
4. **Batch rollup** — calculează verdictul agregat

#### Batch verdict

| Condiție | Batch verdict |
|----------|---------------|
| Toate individuale PASS | **BATCH PASS** |
| Cel puțin 1 CONDITIONAL, zero FAIL | **BATCH CONDITIONAL** |
| Cel puțin 1 FAIL | **BATCH FAIL** |
| Cross-dep issue detectat | Downgrade cu 1 nivel (PASS→CONDITIONAL, CONDITIONAL→FAIL) |

**Batch score** = media aritmetică a scorurilor individuale.

#### Batch report format

```
🔍 BATCH AUDIT REPORT: "{context}" ({N} subiecte)
Date: YYYY-MM-DD

| # | Subiect | Tier | Score | Verdict | Findings |
|---|---------|------|-------|---------|----------|
| 1 | {sub1}  | LIGHT | 3.0/4.0 | CONDITIONAL | 2 |
| 2 | {sub2}  | LIGHT | 4.0/4.0 | PASS | 0 |

Batch score: {medie}/4.0 | Batch verdict: {VERDICT}
Cross-deps: {N} checked, {N} OK
Total findings: {N} | Critical: {N}
```

Emit VK: `✅ [AUDIT-BATCH] context: "{context}" | subjects: {N} | batch-score: {medie}/4.0 | verdict: {VERDICT}`

### Pas 2: EVALUEAZĂ DIMENSIUNILE

8 dimensiuni de audit, grupate pe tier-uri. Fiecare se scorează pe scala **NPLF** (ISO 15504):

| Scor | Nivel | Procent | Înseamnă |
|------|-------|---------|----------|
| **F** | Fully achieved | 86-100% | Complet satisfăcut |
| **L** | Largely achieved | 51-85% | Satisfăcut cu mici lipsuri |
| **P** | Partially achieved | 16-50% | Parțial, gap-uri semnificative |
| **N** | Not achieved | 0-15% | Absent sau complet inadecvat |

#### NPLF Calibration Anchors (P6 — use to ground scores)

| Dim | **F** (86-100%) | **L** (51-85%) | **P** (16-50%) | **N** (0-15%) |
|-----|----------------|----------------|----------------|---------------|
| D1 Completeness | All sections present + test cases + edge cases + enforcement loop | All sections present, 1-2 test cases missing or enforcement loop incomplete | Major section missing (e.g., §2 Procedura has only 2 of 5 steps) | Skeleton only — title + 1 paragraph, no actionable content |
| D2 Accuracy | All technical claims verified, all paths executable, zero contradictions | Mostly correct, 1 minor inaccuracy (e.g., wrong path, outdated version ref) | Multiple inaccuracies or untested claims (e.g., references nonexistent script) | Fundamentally wrong (e.g., NPLF scale inverted, wrong verdict thresholds) |
| D3 Verifiability | Objective checklist, measurable criteria, VK format, automated runner | Checklist present but some items subjective ("good quality") | Vague success criteria ("should work well"), no checklist | No way to verify — "trust me" |
| D4 Clarity | Understandable by target user on first read, no jargon without definition | Clear overall, 1-2 terms undefined or sections that need re-reading | Confusing structure, mixed languages without reason, ambiguous steps | Incomprehensible — wrong audience, jargon-heavy, no examples |
| D5 Consistency | Zero conflicts with other procedures/rules, refs all valid | 1 minor inconsistency (e.g., version mismatch in ref) | Contradicts an active rule or references deprecated procedure | Directly violates a HARD rule or contradicts its own content |
| D6 Safety | All prohibited actions defined, boundaries clear, NEVER list present | Safety section present but incomplete (e.g., missing 1 key prohibition) | Vague safety ("be careful"), no explicit prohibitions | No safety considerations at all |
| D7 Adequacy | Solves the stated problem completely, covers all use cases | Solves main problem, 1-2 edge cases uncovered | Partially solves problem, significant gaps in coverage | Does not solve the stated problem |
| D8 Maintainability | Changelog, version control, ownership, update protocol, health entry | Most present, missing 1 (e.g., no changelog or no health entry) | Ownership unclear, no version control, hard to update | Monolithic, no version, no ownership, update = rewrite |

**REASONING REQUIREMENT (P5)**: For each dimension, the auditor MUST state the evidence found (or not found) BEFORE assigning a score. Example: "D1 Completeness: §1 present ✓, §2 has 5 steps ✓, §3 Cortex template ✓, §4 enforcement loop present but CONNECT missing FORGEBUILD.md ref → **L** (largely achieved, one gap in enforcement connectivity)."

#### Subject-Type Grounding (P9 — which dimensions matter most)

| Subject type | Focus dimensions | Reason |
|-------------|-----------------|--------|
| **Procedură FORGE** | D1 (all sections?), D4 (clear for executor?), D8 (versioned?) | Procedures must be complete, clear, and maintainable |
| **Cod / Codex delivery** | D2 (correct logic?), D3 (testable?), D6 (safe?) | Code must work, be testable, and not break things |
| **Infrastructure / config** | D2 (correct settings?), D6 (secure?), D3 (can verify running?) | Infra must be accurate, secure, and observable |
| **Skill / agent prompt** | D1 (complete instructions?), D4 (unambiguous?), D5 (consistent w/ system?) | Prompts must be complete, clear, and consistent |
| **Architecture / design** | D7 (solves the right problem?), D8 (evolvable?), D5 (fits ecosystem?) | Architecture must be adequate and maintainable |

**Note**: ALL dimensions in the tier are STILL scored. This table indicates which to scrutinize most carefully — not which to skip.

#### LIGHT (3 dimensiuni — minimum obligatoriu)

| # | Dimensiune | Întrebare cheie | Surse |
|---|-----------|-----------------|-------|
| D1 | **Completeness** | Lipsesc pași, secțiuni, sau cazuri? | ISO 9001 7.5, NUREG-1358, IFEval |
| D2 | **Accuracy** | E tehnic corect? Informația e validă? | ISO 19011, NUREG-1358 |
| D3 | **Verifiability** | Poate fi verificat obiectiv că funcționează? | IFEval, AdvancedIF, ISO 19011 |

**Notă D3 (Skill Creator / evals):**
Dacă subiectul auditat are un `evals/evals.json` asociat (skills create cu Skill Creator), D3 se evaluează obiectiv: citește ultimul benchmark run și verifică `passed: true` pe toate assertions. Scorul D3 = `assertions_passed / assertions_total`. Nu este judecată Opus în acest caz — este citire de date.

#### STANDARD (+ 3 dimensiuni)

| # | Dimensiune | Întrebare cheie | Surse |
|---|-----------|-----------------|-------|
| D4 | **Clarity** | E înțeles de utilizatorul țintă? Limbaj simplu? | FAA AC 120-71B, Osmani 2025 |
| D5 | **Consistency** | Intră în conflict cu alte documente/proceduri/reguli? | NUREG-1358, AdvancedIF context carry |
| D6 | **Safety** | Sunt definite acțiunile interzise? Limitele sunt clare? | Osmani 3-tier, agent benchmarks |

#### DEEP (+ 2 dimensiuni)

| # | Dimensiune | Întrebare cheie | Surse |
|---|-----------|-----------------|-------|
| D7 | **Adequacy** | E potrivit pentru scopul declarat? Rezolvă problema reală? | ISO 19011, CMMI L2+, AgentBench |
| D8 | **Maintainability** | Poate fi actualizat ușor? Version control? Ownership clar? | ISO 9001 7.5.3, CMMI L5 |

### 2b. DSE-RESEARCH — Extensie pentru Research Engines / Search Pipelines

**Când se activează**: Subiectul auditat este un sistem care caută, agregă sau sintetizează informație din surse multiple (research engine, search pipeline, data aggregator, scraper multi-sursă).

**4 dimensiuni DSE-RESEARCH** (scala 1-5):

| # | Dimensiune | Ce evaluează | Score guide |
|---|-----------|-------------|-------------|
| **R1** | **Source Coverage** | Sunt toate sursele relevante pentru domeniu acoperite? Există gap-uri? | 5=toate sursele critice prezente + fallback-uri. 3=sursele principale ok, 1-2 gap-uri. 1=surse critice lipsă |
| **R2** | **Prompt Quality** | Prompturile trimise la surse AI (LLM-uri, synthesis) sunt optimizate? System prompts, role definition, output format, constraints. | 5=fiecare sursă AI are system prompt specific per domain + output format + constraints. 3=prompturi prezente dar generice. 1=zero system prompt sau <20 cuvinte |
| **R3** | **Token/Resource Scaling** | Resursele (max_tokens, limits, timeout) scalează cu complexitatea cererii (depth D1→D4)? | 5=scaling explicit per depth + per source type. 3=un singur nivel, dar rezonabil. 1=fix hardcodat, identic indiferent de depth |
| **R4** | **Route Optimization** | Rutarea query-urilor spre surse e optimă? Fiecare tip de query merge la sursa cu cea mai mare autoritate? | 5=fiecare route are primary+fallback optim, zero query type orfan. 3=routing funcțional, 1-2 sub-optimal. 1=routing incorect sau lipsă |

**Checklist rapid R1-R4:**
- [ ] R1: Listează toate sursele implementate. Identifică surse lipsă cu autoritate >0.7 și API gratuit.
- [ ] R2: Pentru fiecare sursă AI (LLM endpoint): are system prompt? >50 cuvinte? Include output format? Include domain context?
- [ ] R3: max_tokens/timeout sunt parametrizate per depth? D4 primește mai mult decât D1?
- [ ] R4: Fiecare query type din router are primary cu autoritate >0.8? Fallback-ul e diferit de primary?

**R1 — Source Coverage Analysis Pattern:**
```
Surse implementate: [lista]
Surse lipsă (autoritate >0.7, API free): [lista cu justificare]
Route gaps: [route] → [surse actuale] → lipsește [sursă] (autoritate: X)
Content capture: [per sursă: ce câmpuri extrage, ce pierde]
Score R1: X/5
```

**R2 — Prompt Quality Analysis Pattern:**
```
Per sursă AI:
  [sursă]: system prompt = [da/nu/N/A]
    - Lungime: [N cuvinte]
    - Role definition: [da/nu]
    - Output format spec: [da/nu]
    - Domain awareness: [da/nu]
    - Constraints/interdicții: [da/nu]
    Score: X/5
Medie R2: X/5
```

### 2c. DSE-WORKFLOW — Extensie pentru Multi-Agent Workflows / Pipelines

**Când se activează**: Subiectul auditat este un sistem multi-step cu handoff-uri între componente (multi-agent workflow, daemon+queue, pipeline cu state management, protocol de comunicare inter-agent).

**4 dimensiuni DSE-WORKFLOW** (scala 1-5):

| # | Dimensiune | Ce evaluează | Score guide |
|---|-----------|-------------|-------------|
| **W1** | **Handoff Quality** | Protocoalele de handoff între componente sunt complete? Input/output format clar? | 5=fiecare handoff documentat cu format, validare, error path. 3=handoff-uri funcționale dar nedocumentate complet. 1=handoff-uri implicite sau lipsă |
| **W2** | **State Management** | Starea sistemului e persistentă, recuperabilă, și observabilă? | 5=state files clare, status tracking, history, rollback. 3=state prezent dar fără history/rollback. 1=state pierdut la restart |
| **W3** | **Error Recovery** | Ce se întâmplă când o componentă eșuează? Existe retry, fallback, alerting? | 5=retry+fallback+alerting+degradation path per componentă. 3=basic error handling, manual recovery. 1=fail silențios sau crash |
| **W4** | **Idempotency** | Operațiile sunt safe de re-executat? Double-run nu produce side effects? | 5=toate operațiile idempotente + documented. 3=majority idempotent, edge cases posibile. 1=re-run produce duplicat/corupție |

**Checklist rapid W1-W4:**
- [ ] W1: Fiecare handoff are: format documentat + validare input + ce se întâmplă la input invalid?
- [ ] W2: State files identificate. Sunt persistente? Pot fi inspectate manual? Există history?
- [ ] W3: Per componentă: ce se întâmplă la timeout/crash? Cine detectează? Cine recuperează?
- [ ] W4: Rulezi același task de 2x — output identic? Zero side effects? Lock files?

**W1 — Handoff Quality Analysis Pattern:**
```
Handoff-uri identificate:
  [Comp A] → [Comp B]: format=[md/json/flag], validare=[da/nu], error path=[da/nu]
  [Comp B] → [Comp C]: ...
Score W1: X/5
```

**W3 — Error Recovery Analysis Pattern:**
```
Per componentă:
  [daemon]: failure mode=[crash/hang/corrupt]
    - Detectare: [watchdog/manual/absent]
    - Recovery: [auto-restart/manual/absent]
    - Alerting: [telegram/log/absent]
    - Data loss: [da/nu/parțial]
Score W3: X/5
```

### Pas 3: CHECK DEPENDENCIES (STANDARD + DEEP)

- Subiectul auditat depinde de alte componente?
- Acele componente sunt funcționale / la zi?
- Există conflicte cu alte proceduri, reguli, sau configurații?

Concret:
- Proceduri: verifică `rules-hard.md` / `rules-standard.md` pentru regula asociată
- Cod: verifică imports, API-uri, dependențe externe
- Infra: verifică servicii dependente, porturi, access

**Gate**: Pas 3 completat → emit `🔗 [AUDIT] Pas 3 deps: {N} checked, {N} OK, {N} issues` SAU `⚠️ [AUDIT] Pas 3 skip: tier=LIGHT`

### Pas 4: VERIFY ENFORCEMENT (STANDARD + DEEP)

- Există un mecanism care verifică respectarea subiectului auditat?
- Acel mecanism funcționează efectiv (nu doar pe hârtie)?
- Test: poți provoca o violație și e detectată?

Concret:
- Proceduri: are §4 Enforcement Loop? WHERE+WHEN+HOW+CONNECT?
- Cod: are teste? Coverage? CI/CD?
- Infra: are monitoring? Alerting? Health checks?

**Gate**: Pas 4 completat → emit `🔗 [AUDIT] Pas 4 enforcement: {present/absent/partial}` SAU `⚠️ [AUDIT] Pas 4 skip: tier=LIGHT`

### Pas 5: RESEARCH VALIDATION (doar DEEP)

- Verifică subiectul contra surse externe (Cortex, web, papers)
- E aliniat cu best practices din domeniu?
- Există alternative mai bune sau mai recente?
- Folosește DARE chain dacă research-ul e D2+

Emit VK: `🔍 [AUDIT-RESEARCH] subject: "{subiect}" | cortex: {N} hits | external: {N} sources | findings: {N}`

**Gate**: Pas 6 e **BLOCAT** fără unul din:
- VK `🔍 [AUDIT-RESEARCH]` emis (research executat), SAU
- `⚠️ [AUDIT-RESEARCH] skip: {motiv}` emis explicit — motive valide: `tier=LIGHT`, `tier=STANDARD`, `subject=non-research system`, `research already done in prior session (cortex_id: X)`

Fără gate → Pas 6 NU se execută. Zero shortcut-uri. "Opus a evaluat D7/D8" ≠ research validation — Opus trebuie să facă research extern (Cortex search + web search), nu doar evaluare din knowledge propriu.

### Pas 6: SCOR + VERDICT

#### Calculare scor

Convertește NPLF în numeric: F=4, L=3, P=2, N=1

**Core score** = media aritmetică a dimensiunilor NPLF evaluate (3, 6, sau 8 după tier)

**DSE score** (dacă DSE activ) = media aritmetică a dimensiunilor DSE (scala 1-5, normalizată la /4.0 pentru compatibilitate: `dse_normalized = dse_raw * 4 / 5`)

**Combined score**:
- Fără DSE: combined = core (100%)
- Cu 1 DSE: `(core * 0.6) + (dse_normalized * 0.4)`
- Cu 2+ DSE-uri: `(core * 0.5) + (dse1_normalized * 0.25) + (dse2_normalized * 0.25)`
- Core primește minim 50% (structura generală rămâne dominantă)
- Fiecare DSE împarte restul de 50% egal

#### Verdict

| Scor (combined) | Verdict | Acțiune |
|-----------|---------|---------|
| ≥ 3.5 | **PASS** | Aprobat. Mențiuni minore opționale. |
| 2.5 — 3.4 | **CONDITIONAL** | Aprobat cu condiții. Listează fix-urile necesare. |
| < 2.5 | **FAIL** | Respins. Listează toate gap-urile. Re-audit obligatoriu după fix. |

**Excepție HARD**: Orice dimensiune individuală cu scor **N** (core) sau **1/5** (DSE) → maxim CONDITIONAL, indiferent de media totală.

**DSE flag**: Dacă DSE score < 2.5/5 (normalizat < 2.0/4) → adaugă `[DSE-WARNING]` la verdict, chiar dacă combined e PASS.

#### Raport format

```
🔍 AUDIT REPORT: "{subiect}"
Tier: LIGHT/STANDARD/DEEP | DSE: {DSE-NAME|none} | Date: YYYY-MM-DD

CORE DIMENSIONS (NPLF):
| # | Dimensiune | Scor | Notă |
|---|-----------|------|------|
| D1 | Completeness | F/L/P/N | {observație scurtă} |
| D2 | Accuracy | F/L/P/N | {observație scurtă} |
| ... | ... | ... | ... |

Core score: {medie}/4.0

DSE-{NAME} DIMENSIONS (dacă activ):
| # | Dimensiune | Scor | Notă |
|---|-----------|------|------|
| R1/W1 | {dim name} | X/5 | {observație scurtă} |
| R2/W2 | {dim name} | X/5 | {observație scurtă} |
| ... | ... | ... | ... |

DSE score: {medie}/5.0 (normalized: {X}/4.0)

Combined: {combined}/4.0 | Verdict: PASS/CONDITIONAL/FAIL
Findings: {N} | Critical: {N}

Actions required (dacă CONDITIONAL/FAIL):
1. {acțiune concretă}
2. {acțiune concretă}
```

#### Complete Audit Report Example (P1 — calibration)

**CONDITIONAL example** (real audit, anonymized):

```
🔍 AUDIT REPORT: "FIVE-STEPS-AGENTS.md"
Tier: STANDARD | DSE: DSE-WORKFLOW | Date: 2026-03-19

CORE DIMENSIONS (NPLF):
| # | Dimensiune | Scor | Notă |
|---|-----------|------|------|
| D1 | Completeness | L | §1-§4 present, test cases present, but §4 CONNECT missing procedure-health.json ref |
| D2 | Accuracy | F | All 5 steps technically sound, model routing correct, tool isolation verified |
| D3 | Verifiability | F | 8-item checklist, VK format defined, objective criteria |
| D4 | Clarity | L | Clear for technical users, Phase 0 intake questions excellent, but some steps could use examples |
| D5 | Consistency | L | Aligns with FORGE v1.4 and NexusOS patterns, minor version ref mismatch |
| D6 | Safety | F | Boundaries step prevents scope creep, NEVER drop errors, model routing prevents cost overrun |

Core score: 3.5/4.0

DSE-WORKFLOW DIMENSIONS:
| # | Dimensiune | Scor | Notă |
|---|-----------|------|------|
| W1 | Handoff Quality | 4/5 | Orchestrator→sub-agent handoff format defined, but no error path on invalid input |
| W2 | State Management | 3/5 | No persistent state between runs (stateless by design), but no mention of when state IS needed |
| W3 | Error Recovery | 4/5 | "Never drop, always flag" principle, but no retry mechanism specified |
| W4 | Idempotency | 3/5 | Re-running produces same architecture, but no lock to prevent concurrent generation |

DSE score: 3.5/5.0 (normalized: 2.8/4.0)

Combined: (3.5 * 0.6) + (2.8 * 0.4) = 3.22/4.0 | Verdict: CONDITIONAL
Findings: 4 | Critical: 0

Actions required:
1. Add procedure-health.json ref in §4 CONNECT
2. Add error path for invalid handoff input in orchestrator template
3. Clarify when persistent state is needed vs stateless
4. Fix version ref mismatch (PromptForge v3.3 → v3.5)
```

#### Delta vs Previous Audit (opțional)

Dacă există un audit anterior în Cortex pentru același subiect (`search` după `subject_id`), calculează delta per dimensiune și per scor total:
- Dimensiuni îmbunătățite
- Dimensiuni deteriorate
- Trend general (`↑` / `↓` / `=`)

Dacă nu există audit anterior, acest pas se sare explicit cu VK:
`⚠️ [AUDIT-DELTA] skip: no previous audit found for "{subiect}"`

**Format delta output**:
```
Delta vs audit {data_anterioară} (combined: {X_anterior}/4.0 → {X_nou}/4.0 | {↑/↓/=} {delta}):
| Dimensiune | Anterior | Acum | Delta |
|-----------|---------|------|-------|
| D1 Completeness | F | F | = |
| D2 Accuracy | P | L | ↑ |
| D3 Verifiability | N | P | ↑↑ |
Dimensiuni îmbunătățite: D2, D3 | Deteriorate: — | Trend: ↑
```

**Verdict confidence (P22)**:
- **HIGH**: All dimensions have clear, objective evidence (code exists, checklist present, tests pass)
- **MEDIUM**: 1-2 dimensions scored on judgment calls (e.g., D4 Clarity is partially subjective)
- **LOW**: 3+ dimensions scored on judgment, or auditor unfamiliar with subject domain

Emit VK: `✅ [AUDIT] subject: "{subiect}" | tier: {TIER} | dse: {DSE|none} | core: {X}/4.0 | dse: {X}/5.0 | combined: {X}/4.0 | verdict: {VERDICT} | confidence: {HIGH|MEDIUM|LOW}`

---

## 3. Cortex Logging

La fiecare audit completat, salvează raportul:

```json
{
  "text": "AUDIT: {subiect} | tier: {TIER} | score: {medie}/4.0 | verdict: {VERDICT}. Findings: {lista scurtă}. Actions: {lista scurtă}.",
  "collection": "procedures",
  "metadata": {
    "type": "audit-report",
    "procedure": "FORGE-AUDIT",
    "rule_id": "QUAL-H-004",
    "audit_tier": "{LIGHT/STANDARD/DEEP}",
    "audit_subject": "{subiect}",
    "audit_score": "{medie}",
    "audit_verdict": "{PASS/CONDITIONAL/FAIL}",
    "dimensions_evaluated": "{N}",
    "findings_count": "{N}",
    "has_enforcement_loop": true,
    "forge_version": "1.7",
    "forge_bypass": true,
    "forge_bypass_reason": "audit-report",
    "tags": ["audit", "quality", "{subject-type}"]
  }
}
```

### FORGE Bypass — De ce e necesar și cum se folosește

Rapoartele de audit **NU sunt proceduri FORGE** — nu au §1 Problema / §2 Procedura / §4 Enforcement Loop. FORGE gate-ul de pe Cortex API (forge.ts) le va respinge cu HTTP 422 dacă nu au bypass.

**Soluția**: `forge_bypass: true` + `forge_bypass_reason` obligatoriu în metadata.

#### Bypass reasons permise

| Reason | Când | Exemplu |
|--------|------|---------|
| `audit-report` | Raport de audit individual | FORGE-AUDIT pe m4-158 |
| `batch-audit-report` | Raport de audit batch (rollup) | Batch audit pe 4 Codex deliveries |
| `session-summary` | Sumar de sesiune | End-of-session Cortex save |
| `meta-analysis` | Analiză de proceduri/use cases | Use case audit log |
| `research-note` | Notă de research (nu procedură) | Delphi findings, web research |

**Orice alt reason** → Genie verifică cu Pafi înainte de bypass.

#### Ce se întâmplă fără bypass

```
curl → Cortex POST /api/store → forge.ts middleware →
  collection = "procedures"? DA →
  FORGE check (rule_id, enforcement_loop, sections) →
  FAIL → HTTP 422 {"error": "FORGE validation failed", "violations": [...]}
```

#### Ce se întâmplă cu bypass

```
curl → Cortex POST /api/store →
  forge_bypass: true? DA →
  forge_bypass_reason present? DA →
  LOG bypass reason → ACCEPT → HTTP 200
```

**Bypass fără reason** → 422 respins (forge.ts verifică ambele câmpuri).

#### Audit reports vs procedures — diferențe structurale

| Câmp | Procedură FORGE | Audit report |
|------|----------------|--------------|
| §1 Problema | ✅ obligatoriu | ❌ nu se aplică |
| §2 Procedura | ✅ obligatoriu | ❌ nu se aplică |
| §3 Cortex Logging | ✅ obligatoriu | ✅ da (acest template) |
| §4 Enforcement Loop | ✅ obligatoriu | ❌ nu se aplică |
| `rule_id` | ✅ ID-ul regulii | ✅ QUAL-H-004 (fix) |
| `has_enforcement_loop` | ✅ true | ✅ true (procedura FORGE-AUDIT are) |
| `forge_bypass` | ❌ absent | ✅ true |
| `forge_bypass_reason` | ❌ absent | ✅ obligatoriu |

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — orice task care implică review/audit
- DEV-H-010 — Codex delivery audit (obligatoriu)
- Precision Mode Step 7 — post-task checkpoint (când PM activ)
- Manual — Pafi cere `audit X`

### WHEN
- La fiecare Codex delivery (DEV-H-010 gate)
- La fiecare procedură nouă (pre-publicare review)
- La request explicit de audit
- La Precision Mode checkpoint dacă subiectul e complex

### HOW (violation detection)
- Audit executat fără tier classification → violation
- Audit fără scor NPLF pe fiecare dimensiune → violation
- LIGHT audit pe subiect care necesită STANDARD/DEEP → violation (under-auditing)
- Verdict CONDITIONAL fără actions list → violation
- Audit FAIL fără re-audit programat → violation
- **DEEP audit fără Pas 5 Research (VK sau skip explicit) → violation (FM-1)**
- **Pas skip-uit fără `⚠️ skip: motiv` explicit → violation**
- Runner: manual (Genie self-check) + Precision Mode Step 7

### Known Failure Modes

| # | Anti-pattern | Detectat | Root cause | Fix aplicat |
|---|-------------|----------|------------|-------------|
| FM-1 | DEEP audit: Opus evaluează D7/D8 din knowledge propriu → claim "research validation done" fără Cortex/web search | 2026-02-28 | Pas 5 nu avea gate condition — putea fi skip-uit silențios. VERIFY nu verifica execuția Pas 5. | Gate condition pe Pas 6 (VK obligatoriu). VERIFY +2 checks. HOW +2 violations. |
| FM-2 | Batch audit: audituri individuale fără rollup → findings izolate, nu se detectează cross-dependency issues între subiecte | 2026-02-28 | Lipsea complet secțiunea batch mode. 4 audituri individuale fără agregare sau cross-check. | Pas 1b Batch Mode adăugat: cross-dep check, batch verdict, rollup format. |
| FM-3 | Cortex save blocat: audit report respins de FORGE gate (HTTP 422) → bypass manual ad-hoc, nedocumentat | 2026-02-28 | §3 Cortex Logging template nu includea `forge_bypass`. Rapoartele de audit nu sunt proceduri FORGE dar mergeau în colecția `procedures`. | §3 extins cu bypass pattern complet: reasons permise, diferențe structurale, flow diagrams. |
| FM-4 | Audit PASS pe sistem complex dar domain-specific quality sub-optimal: dimensiunile NPLF verifică structura generică dar nu intră în profunzimea domeniului (ex: Delphi — 10 surse, 1 returnează text gol, 2 fără system prompt → D1 Completeness = L dar prompt quality = 1.5/5) | 2026-02-28 | FORGE-AUDIT avea doar dimensiuni generice (D1-D8). Lipseau extensii per domeniu. Un research engine poate trece D1 structural dar să fie slab pe surse/prompturi. | DSE mechanism adăugat v1.3: §2b DSE-RESEARCH (R1-R4), §2c DSE-WORKFLOW (W1-W4). DSE se activează automat la Pas 1, scorat separat, combinat cu core (60/40). |
| FM-5 | **Sistem complex auditat cu un singur DSE deși califică pentru multiple**: ECHELON auditat cu DSE-WORKFLOW dar NU cu DSE-RESEARCH, deși e simultan pipeline multi-step (workflow) ȘI agregator de date cu LLM prompts (research). Regula "max 1 DSE" a forțat alegerea, ratând 16 files cu prompturi copy-paste, zero domain awareness, over-permissive filter. Exact aceeași eroare ca FM-4 (Delphi) — audit pe structură, ratat domain depth. | 2026-02-28 | Regula DSE din v1.3 spunea "Maximum 1 DSE per audit". Sistemele complexe (ECHELON, viitoare platforme) pot avea nevoie de multiple DSE-uri simultane. Fără toate DSE-urile relevante, auditul ratează categorii întregi de probleme. | Fix v1.4: (1) Regulă schimbată: "Multiple DSE-uri permise". (2) Scoring adaptat: core 50% + DSE-uri împart restul de 50% egal. (3) **CHECKLIST DE VERIFICARE**: la Pas 1, după DSE detection, Genie MUST verifică: "Există alt DSE care se aplică?" — dacă da, adaugă-l. |

**Update protocol**: Când se detectează un nou failure mode → adaugă aici cu dată + root cause + fix.

### CONNECT
- **QUAL-H-004** → Verify-Before-Assert — auditul aplică acest principiu structural
- **DEV-H-010** → Dual Audit — Codex delivery audit folosește acest template
- **PRECISION-MODE.md** → Step 7 checkpoint folosește FORGE-AUDIT dacă subiectul e complex
- **FORGEBUILD.md** → Checklist Pre-Publicare include audit pe procedura nouă
- `procedure-health.json` → entry adăugat

### VERIFY (procedural checkpoint)
- [ ] Tier-ul e corect pentru subiect? (nu under/over-audit)
- [ ] Toate dimensiunile tier-ului au scor NPLF?
- [ ] Verdict-ul respectă regulile (N pe orice dim → max CONDITIONAL)?
- [ ] Raportul e emis în format standard?
- [ ] Salvat în Cortex?
- [ ] **DEEP only**: Pas 5 Research executat? (VK `🔍 [AUDIT-RESEARCH]` prezent SAU `⚠️ skip` cu motiv)
- [ ] **Gate check**: Fiecare pas din tier a emis VK sau skip explicit? (zero pași silențioși)

**VK-uri obligatorii:**
1. **Execution VK**: `✅ [PROC] FORGE-AUDIT | tier:{TIER} §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "AUDIT: {subiect}" | FORGE ✓ | rule: QUAL-H-004 | v1.0`

### MODEL ROUTING

> **Notă**: Tabelul de mai jos este specific FORGE-AUDIT. Pentru model routing general vezi `memory/rules/model-routing-table.md`. În caz de conflict, model-routing-table.md e sursa de adevăr.

> ⛔ **REGULĂ HARD**: verdictul final FORGE-AUDIT rămâne la **Opus subagent**.
> Motivul: Sonnet ratează bug-uri critice (ex: schema mismatch Cortex nedetectat în audit Sonnet pe liked-ingestion.py — prins doar de Opus). Orice audit Sonnet/Genie-direct este invalid și trebuie refăcut.
> Codex poate fi folosit doar ca **discovery lane read-only** pentru audit tehnic async; findings-urile lui trebuie revizuite și confirmate de Opus.

| Activitate | Model | Motivul |
|-----------|-------|---------|
| LIGHT audit | **Opus subagent** | Chiar și auditele simple necesită cross-file verification |
| STANDARD audit | **Opus subagent** | 6 dimensiuni — Sonnet omite integrări critice |
| DEEP audit | **Opus subagent** | 8 dimensiuni, research chain, high stakes |
| Codex Audit (async) | **GPT-5.4** | >200 linii, bash, security; read-only JSON output, discovery layer — Opus verdict final |
| Re-audit după fix | **Opus subagent** | Consistency + same rigor |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| FORGEBUILD.md | Template structură proceduri (D1 Completeness check) | `~/.nexus/procedures/FORGEBUILD.md` |
| procedure-health.json | Registry proceduri (D8 Maintainability check) | `~/.openclaw/procedure-health.json` |
| rules-hard.md | Reguli HARD (D5 Consistency check) | `memory/rules/rules-hard.md` |
| Cortex API | Salvare rapoarte audit | `localhost:6400/api/store` |
| PRECISION-MODE.md | Integration cu Step 7 checkpoint | `~/.nexus/procedures/PRECISION-MODE.md` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Audit coverage | % subiecte auditate vs total subiecte modificate | ≥ 90% |
| Tier accuracy | % audituri cu tier corect (nu under/over) | ≥ 95% |
| PASS rate | % audituri cu verdict PASS (indicator calitate generală) | ≥ 70% |
| Re-audit resolution | % FAIL/CONDITIONAL → PASS la re-audit | ≥ 90% |
| Scoring consistency | Standard deviation a scorurilor pe audituri similare | σ < 0.5 |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (QUAL-H-004, DEV-H-010)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu checks obligatorii
- [x] Descrie CE nu CUM
- [x] VK format specificat (per VK-H-001)
- [x] Entry adăugat în `procedure-health.json` (2026-02-28, id: 0254174c)
- [x] Salvat în Cortex cu metadata FORGE (2026-02-28, id: 77cb77d7, v1.1, supersedes bb2c2d7d)
- [x] Procesul din WHERE editat să referențieze FORGE-AUDIT (PM Step 7 + CLAUDE.md WISH H Flux B/C)
- [x] v1.2 (2026-02-28): Pas 1b Batch Mode + §3 FORGE Bypass docs + FM-2/FM-3 (use-case-audit-log findings)
- [x] v1.3 (2026-02-28): DSE mechanism (Domain-Specific Extensions): §2b DSE-RESEARCH (R1-R4) + §2c DSE-WORKFLOW (W1-W4) + Pas 6 combined scoring + FM-4 (Delphi audit gap)
- [x] v1.4 (2026-02-28): Multiple DSE fix: removed "max 1 DSE" rule → multiple DSE-uri permise. Updated scoring for 2+ DSEs (core 50% + DSEs split 50%). FM-5 (ECHELON audited with 1 DSE instead of 2 — same error as FM-4, repeated)
- [x] v1.7 (2026-03-19): SOL R3 (FINAL). +P22 Verdict Confidence (HIGH/MEDIUM/LOW with definitions). +P9 Subject-Type Grounding table (which dimensions to scrutinize per subject type). **STABLE** — delta +2.5 ≈ threshold. Next SOL eligible: 2026-04-18.
- [x] v1.6 (2026-03-19): SOL-on-FORGE-AUDIT R1. +P6 NPLF Calibration Anchors table (concrete F/L/P/N examples per D1-D8). +P5 Reasoning Requirement (auditor must state evidence before score). +P1 Complete Audit Report Example (CONDITIONAL case with DSE-WORKFLOW). SOL score: 81→est.88 (+7).
- [x] v1.5 (2026-03-06): Delta section adăugat (Pas 6 — delta vs previous audit). D3 objective scoring via evals.json (assertions_passed/assertions_total). SkillCreator FORGE-AUDIT gate integrat.
