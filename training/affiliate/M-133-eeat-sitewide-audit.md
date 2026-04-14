---
type: procedure
created: 2026-03-17
status: active
slug: m-133-eeat-sitewide-audit
tags: [procedure, nexus]
---

# M-133 — E-E-A-T Site-Wide Authority Audit

**ID**: M-133
**Domeniu**: Affiliate / SEO Content
**Sub-domeniu**: eeat-authority-audit
**Versiune**: 2.0
**Creat**: 2026-03-05
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Conexiuni**: M-126, M-127, M-130
**Trigger**: Quarterly + pre-launch orice site afiliat nou

---

## Scope

Această procedură acoperă auditul complet E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) la nivel de site pentru proprietăți afiliate SMSads active în verticalele betting/casino, forex/CFD trading și health supplements, targetând GEO-urile PE, KE, CL, BO, RO. Procedura produce un scorecard cuantificat, un gap analysis față de competitori și un action plan prioritizat pentru îmbunătățirea semnalelor de autoritate înainte de algoritmii Google SpamBrain și Helpful Content Update (HCU).

---

## Section 1 — Problema

### Context și Consecințe

Siturile afiliate din verticalele gambling, forex și health supplements se încadrează în categoria YMYL (Your Money or Your Life) conform Search Quality Evaluator Guidelines (SQEG) Google. Pentru această categorie, Google aplică standarde E-E-A-T considerabil mai stricte decât pentru conținut general. Post-HCU (septembrie 2023, martie 2024) și cu SpamBrain activ în mod continuu, siturile YMYL fără semnale clare de autoritate și expertiză reală au pierdut în medie 40–65% din traficul organic.

### Impactul Concret asupra Siturilor de Gambling Afiliate

**Pierderi documentate post-HCU:**
- Siturile de casino/betting fără autori verificabili și fără bio-uri cu expertiză reală au înregistrat scăderi medii de 52% în vizibilitate organică (sursa: Semrush Sensor, Q4 2023).
- Siturile fără pagini de responsible gambling conforme (nu doar disclaimer generic) au primit penalizări manuale în UK, AU și RO (documentate în Search Console reports 2024).
- Absența licențelor de gambling vizibile (număr licență + autoritate emitentă) a corelat direct cu rate de bounce crescute de 70%+ și cu dezindexare în piețele reglementate.

**Riscuri specifice GEO-urilor SMSads:**
- **Peru (PE)**: Ministerul de Cultura + MINCETUR impun avertismente de joc responsabil în spaniolă. Absența → risc de blocare ISP.
- **Kenya (KE)**: BCLB (Betting Control and Licensing Board) solicită numărul de licență vizibil pe fiecare pagină. Absența → penalizare manuală Google Kenya SERP.
- **Chile (CL)**: SJC (Superintendencia de Casinos de Juego) + Ley 21.645 (2024) impun disclaimer de vârstă 18+ și link activ către linia de ajutor de gambling.
- **Bolivia (BO)**: Cadru legal în formare — risc de over-restriction dacă site-ul nu este transparent cu privire la statutul afiliat.
- **Romania (RO)**: ONJN (Oficiul Național pentru Jocuri de Noroc) — licența operatorului promovat trebuie afișată explicit. Siturile afiliate fără mențiunea "conținut publicitar" sunt penalizate.

**SpamBrain și semnale negative:**
- Conținut generat AI fără layer de expertiză umană verificabilă → SpamBrain flag "scaled content abuse" (Google policy update martie 2024).
- Profiluri de link building artificiale detectate prin Majestic Trust Flow < 15 pe domenii de referință → dezautorizare automată în 60–90 zile.
- Absența schemei de markup `Review`, `Person`, `Organization` pe paginile de recenzii → CTR scăzut și tratament ca "thin content".

### Consecința Neauditării

Un site afiliat în vertical gambling care nu trece prin auditul E-E-A-T înainte de lansare sau trimestrial riscă:
1. Pierderea a 30–70% din traficul organic la primul update major.
2. Penalizare manuală din echipele de Quality Raters Google (în special pentru GEO-urile reglementate KE, CL, RO).
3. Dezindexare parțială sau completă în cazul semnalelor cumulate negative (thin content + lipsă autori + lipsă trust signals).
4. Pierderea comisioanelor afiliate asociate — estimat $2,000–$50,000/lună per site în portofoliul SMSads.

---

## Section 2 — Procedura

### Step 1 — Author Credentials Audit

Verifică dacă fiecare autor care semnează conținut pe site are un profil de expertiză verificabil și consistent cu cerințele YMYL.

**Tool**:
- Ahrefs Content Explorer (https://ahrefs.com/content-explorer) — caută author bylines indexate
- LinkedIn Search — verificare profil autor real
- Google Search: `"[author name]" site:[domeniu]` + `"[author name]" gambling/betting/forex`
- Schema.org Markup Validator (https://validator.schema.org) — verificare markup `Person` + `sameAs`
- Google Search Quality Evaluator Guidelines (SQEG) — secțiunea 3.3 (Author E-E-A-T)

**Output**: `author-credentials-audit-[site]-[YYYY-MM].xlsx`
- Sheet 1: Listă autori (Nume, Bio URL, LinkedIn URL, Schema markup prezent Y/N, Expertiză verificată Y/N)
- Sheet 2: Gaps identificate per autor
- Sheet 3: Scor per autor (1–5)

**Checkpoint**:
- Minim 80% din articolele YMYL au byline cu autor real (nu "Editorial Team" generic)
- Fiecare autor YMYL are bio cu minim 150 cuvinte care menționează experiența specifică în domeniu
- Schema `Person` cu `sameAs` (LinkedIn/Twitter) implementat pe minim 70% din pagini de autor
- Zero autori cu profil LinkedIn inexistent sau cu mai puțin de 12 luni experiență relevantă declarată

---

### Step 2 — About Page & Team Transparency Audit

Evaluează dacă paginile About, Team și Contact comunică în mod credibil entitatea din spatele site-ului, conform cerințelor SQEG secțiunea 2.6 (Website Information and Reputation).

**Tool**:
- Wayback Machine (https://web.archive.org) — istoricul paginilor About (verificare consistență)
- Google Search: `site:[domeniu] about OR team OR contact`
- Schema.org Markup Validator — verificare markup `Organization`, `LocalBusiness`
- WHOIS Lookup (https://who.is) — verificare transparență ownership (dacă e public)
- Manual review checklist SQEG secțiunea 2.6

**Output**: `transparency-audit-[site]-[YYYY-MM].md`
- Pagina About: URL, word count, entitate legală menționată Y/N, contact fizic Y/N, dată fondare Y/N
- Pagina Team: URL, număr membri listați, foto reale Y/N, bio-uri individuale Y/N
- Schema Organization: implementat Y/N, câmpuri completate (name, url, logo, contactPoint, address)
- Scor global transparență (1–5)

**Checkpoint**:
- Pagina About are minim 400 cuvinte cu descrierea misiunii și a echipei
- Adresă de contact reală sau formular funcțional prezent (nu doar email generic)
- Schema `Organization` implementată cu minim 5 câmpuri completate
- Pagina About actualizată în ultimele 12 luni (verificat via Wayback Machine sau dată publicare)
- Pentru GEO KE/CL/RO: entitatea legală locală sau partenerul licențiat menționat explicit

---

### Step 3 — External Authority Signals Audit

Evaluează calitatea și relevanța profilului de backlink-uri și a mențiunilor externe care contribuie la Authoritativeness conform E-E-A-T.

**Tool**:
- Ahrefs Site Explorer (https://ahrefs.com/site-explorer) — Domain Rating, backlink profile, referring domains
- Majestic SEO (https://majestic.com) — Trust Flow (TF) / Citation Flow (CF), Topical Trust Flow
- SEMrush Backlink Analytics (https://semrush.com) — Authority Score, toxic link detection
- Google Search: `[brand name] review` + `[brand name] legit` — brand mention monitoring
- Ahrefs Alerts — brand mention tracking setup

**Output**: `external-authority-audit-[site]-[YYYY-MM].xlsx`
- Sheet 1: Top 50 referring domains (Domain, DR, TF, Relevant Y/N, Anchor text)
- Sheet 2: Toxic/spammy links identificate (URL, TF score, acțiune recomandată)
- Sheet 3: Brand mentions (sursă, sentiment, link prezent Y/N)
- Sheet 4: Scor autoritate externă (1–5)

**Checkpoint**:
- Domain Rating Ahrefs ≥ 30 pentru site-uri cu vârsta > 12 luni
- Majestic Trust Flow ≥ 20 (pentru gambling niche, TF ≥ 15 este minimul acceptabil)
- Topical Trust Flow categoria "Gambling" sau "Finance" în top 3 categorii
- Minim 15 referring domains cu DR ≥ 40 și relevanță tematică
- Zero backlink-uri din rețele PBN identificate (Majestic CF/TF ratio > 3:1 = flag)
- Procentaj linkuri dofollow: 60–80% (sub 60% sau peste 85% = flag)

---

### Step 4 — Content Expertise Depth Audit

Evaluează dacă conținutul demonstrează experiență reală și cunoaștere în profunzime specifică verticalului, conform cerinței "Experience" din E-E-A-T (adăugat de Google în decembrie 2022).

**Tool**:
- SEMrush Content Audit (https://semrush.com/content-audit/) — identificare pagini thin content
- Ahrefs Site Audit — "Low word count" + "Duplicate content" flags
- Screaming Frog SEO Spider (https://screamingfrog.co.uk) — crawl complet, word count per pagină
- Copyscape (https://copyscape.com) — verificare unicitate conținut
- ChatGPT/Claude — detectare pattern conținut AI neredactat (manual spot-check)
- Google Search Quality Evaluator Guidelines — secțiunea 4.5 (High Quality pages)

**Output**: `content-depth-audit-[site]-[YYYY-MM].xlsx`
- Sheet 1: Inventar pagini (URL, Word Count, Thin Content Y/N, Autor Y/N, Schema Review Y/N)
- Sheet 2: Pagini YMYL identificate + scor expertiză (1–5 per pagină)
- Sheet 3: Conținut duplicat sau AI-detectat fără redactare
- Sheet 4: Recomandări prioritizate (HIGH/MEDIUM/LOW)

**Checkpoint**:
- Zero pagini YMYL (recenzii casino, pagini odds, ghiduri forex) cu sub 800 cuvinte
- Minim 60% din recenziile de operator includ date actualizate (bonusuri, cotă, licență) verificate manual
- Zero pagini cu conținut identic sau quasi-identic (similaritate > 80% via Copyscape)
- Paginile de top 20 trafic au minim un element de "Experience" demonstrat: screenshot personal, test real, date proprii, video review
- Schema markup `Review` cu `ratingValue` și `reviewBody` pe 100% din paginile de recenzii

---

### Step 5 — Trust Signals Audit (Disclaimers, T&C, Responsible Gambling)

Verifică prezența și conformitatea tuturor semnalelor de trust obligatorii pentru verticalele YMYL gambling și forex, specific GEO-urilor targetate.

**Tool**:
- Screaming Frog — crawl footer/header pentru elemente trust (disclaimer, 18+, licență)
- Manual review checklist per GEO (PE, KE, CL, BO, RO)
- Schema.org Markup Validator — verificare `LegalService`, `FinancialProduct` schema
- Google Search Console — verificare penalizări manuale active (Search Console > Manual Actions)
- Ahrefs — verificare indexare pagini T&C, Privacy Policy, Responsible Gambling

**Output**: `trust-signals-audit-[site]-[YYYY-MM].md`

**Checklist GEO-specific:**

| Element Trust | PE | KE | CL | BO | RO | Status |
|---|---|---|---|---|---|---|
| Disclaimer responsabilitate gambling (limba locală) | REQ | REQ | REQ | REQ | REQ | |
| Număr licență operator promovat vizibil | REC | REQ (BCLB) | REQ (SJC) | REC | REQ (ONJN) | |
| Link GamCare / BeGambleAware sau echivalent local | REC | REQ | REQ | REC | REQ | |
| Avertisment 18+ / 21+ pe fiecare pagină | REQ | REQ | REQ | REQ | REQ | |
| Mențiunea "conținut publicitar afiliat" | REQ | REQ | REQ | REQ | REQ | |
| Privacy Policy + GDPR/LGPD compliant | REQ | REQ | REQ | REQ | REQ | |
| T&C pagină dedicată indexabilă | REQ | REQ | REQ | REQ | REQ | |
| Odds verification disclaimer (forex) | N/A | REC | REC | REC | REQ | |

**Checkpoint**:
- 100% din paginile cu conținut gambling au disclaimer responsible gambling vizibil (nu ascuns în footer cu font-size < 10px)
- Numărul de licență BCLB (KE), SJC (CL), ONJN (RO) prezent în footer pe fiecare pagină
- Pagina Responsible Gambling există, are minim 300 cuvinte și include link-uri funcționale la resurse locale
- Zero penalizări manuale active în Search Console la momentul auditului
- Privacy Policy actualizată în ultimele 24 luni și conformă cu legislația locală (LGPD pentru CL/PE, GDPR pentru RO)

---

### Step 6 — Competitor E-E-A-T Benchmarking

Compară semnalele E-E-A-T ale site-ului auditat față de top 5 competitori organici din SERP-urile targetate.

**Tool**:
- Ahrefs Site Explorer — comparație DR, backlink profile, keyword overlap
- SEMrush Organic Research — top competitor identification per GEO
- Majestic Bulk Backlink Checker — comparație TF/CF per competitori
- Manual SQEG review — evaluare comparativă pagini About, autori, trust signals
- Ahrefs Content Gap — identificare topicuri de autoritate acoperite de competitori, lipsă la noi

**Output**: `competitor-eeat-benchmark-[site]-[YYYY-MM].xlsx`
- Sheet 1: Top 5 competitori identificați per GEO (domeniu, DR, TF, estimat trafic)
- Sheet 2: Scorecard comparativ E-E-A-T (Experience / Expertise / Authoritativeness / Trustworthiness) 1–5 per competitor
- Sheet 3: Content gaps identificate (topicuri autoritate lipsă)
- Sheet 4: Quick wins (ce putem implementa în 30 zile pentru a reduce gap-ul)

**Checkpoint**:
- Site-ul auditat are scor E-E-A-T agregat minim egal cu media top 3 competitori (nu neapărat lider)
- Identificate minim 10 topicuri de autoritate acoperite de competitori și lipsă pe site-ul nostru
- Benchmark realizat separat per GEO principal (nu global — SERP-urile diferă semnificativ KE vs RO)
- Quick wins list are minim 5 acțiuni cu impact estimat și efort estimat

---

### Step 7 — E-E-A-T Improvement Action Plan

Sintetizează toate gap-urile identificate în pașii 1–6 într-un plan de acțiune prioritizat cu owners, deadlines și metrici de succes.

**Tool**:
- Google Sheets / Notion — action plan tracker
- Ahrefs / SEMrush — tracking implementare (re-crawl post-fixes)
- Cortex MCP — logging și task tracking
- Google Search Console — monitorizare impact post-implementare

**Output**: `eeat-action-plan-[site]-[YYYY-MM].xlsx`
- Sheet 1: Action items (Acțiune, Prioritate HIGH/MED/LOW, Owner, Deadline, Status)
- Sheet 2: E-E-A-T Scorecard pre/post (comparație după implementare)
- Sheet 3: KPI tracking (trafic organic, poziții medii, CTR, DR)

**Checkpoint**:
- Action plan are minim 15 acțiuni concrete, fiecare cu owner și deadline specific
- Prioritatea HIGH: toate acțiunile au deadline ≤ 30 zile
- Prioritatea MEDIUM: deadline ≤ 90 zile
- Scorecard post-implementare programat la 60 zile după finalizarea acțiunilor HIGH
- Plan aprobat de Pafi înainte de implementare (pentru acțiuni cu risc MEDIUM+)

---

### E-E-A-T Scorecard — Template & Thresholds

#### Scoring per Dimensiune (1–5)

| # | Dimensiune E-E-A-T | Scor 1 (Fail) | Scor 3 (Mediu) | Scor 5 (Excelent) |
|---|---|---|---|---|
| 1 | Experience (Autori) | Zero bylines reale | Bylines prezente, bio generic | Bio detaliat, experiență verificabilă, content personal demonstrat |
| 2 | Expertise (Conținut) | Thin content, fapte greșite | Conținut corect, superficial | Profunzime demonstrată, date proprii, actualizat regulat |
| 3 | Authoritativeness (Linkuri + Mențiuni) | DR < 15, TF < 10 | DR 25–40, TF 15–25 | DR > 50, TF > 30, mențiuni media relevante |
| 4 | Trustworthiness (Trust Signals) | Fără disclaimer, fără licență | Disclaimer prezent, incomplet | 100% compliant GEO, toate licențele vizibile, responsible gambling complet |
| 5 | Schema Markup | Absent | Parțial implementat | 100% pagini YMYL cu schema corectă |
| 6 | Transparență Entitate | Fără About/Team | About generic fără echipă | About detaliat, echipă reală, entitate legală menționată |
| 7 | Competitor Positioning | Sub media top 5 cu > 2 puncte | La media top 5 | Peste media top 3 pe minim 3 din 4 dimensiuni |

**Scor Maxim**: 35 puncte (7 dimensiuni × 5)

#### Thresholds Globale

| Scor Total | Rating | Decizie |
|---|---|---|
| 28–35 | PASS | Site gata pentru campanii trafic plătit + SEO agresiv |
| 21–27 | CONDITIONAL | Implementare action plan obligatorie în 30 zile; campanii cu buget limitat |
| 14–20 | FAIL — Remediere Necesară | Stop campanii SEO; fix prioritar înainte de orice investiție în conținut |
| < 14 | CRITICAL FAIL | Risc penalizare manuală iminentă; escaladare urgentă la Pafi |

#### Thresholds per Dimensiune (Minim Acceptabil)

| Dimensiune | Minim PASS | Sub acest scor = blocker |
|---|---|---|
| Trustworthiness | ≥ 4 | DA — blocker absolut pentru GEO reglementate |
| Experience (Autori) | ≥ 3 | DA — blocker pentru vertical YMYL |
| Authoritativeness | ≥ 2 | NU blocker individual, dar penalizează scorul global |
| Schema Markup | ≥ 3 | DA — blocker pentru pagini de recenzii (CTR impact direct) |

---

## Section 3 — Cortex Logging

```json
{
  "procedure_id": "M-133",
  "version": "2.0",
  "execution_date": "{{YYYY-MM-DD}}",
  "executor": "{{analyst_name}}",
  "site": {
    "domain": "{{domain}}",
    "geo_targets": ["PE", "KE", "CL", "BO", "RO"],
    "vertical": "{{betting|casino|forex|health}}",
    "site_age_months": {{integer}},
    "launch_type": "{{new|existing|redesign}}"
  },
  "eeat_scorecard": {
    "experience_authors": {{1-5}},
    "expertise_content": {{1-5}},
    "authoritativeness_links": {{1-5}},
    "trustworthiness_signals": {{1-5}},
    "schema_markup": {{1-5}},
    "entity_transparency": {{1-5}},
    "competitor_positioning": {{1-5}},
    "total_score": {{7-35}},
    "rating": "{{PASS|CONDITIONAL|FAIL|CRITICAL_FAIL}}"
  },
  "step_outputs": {
    "step1_author_audit": "{{path/to/author-credentials-audit.xlsx}}",
    "step2_transparency_audit": "{{path/to/transparency-audit.md}}",
    "step3_external_authority": "{{path/to/external-authority-audit.xlsx}}",
    "step4_content_depth": "{{path/to/content-depth-audit.xlsx}}",
    "step5_trust_signals": "{{path/to/trust-signals-audit.md}}",
    "step6_competitor_benchmark": "{{path/to/competitor-eeat-benchmark.xlsx}}",
    "step7_action_plan": "{{path/to/eeat-action-plan.xlsx}}"
  },
  "geo_compliance": {
    "PE": "{{PASS|FAIL|N/A}}",
    "KE": "{{PASS|FAIL|N/A}}",
    "CL": "{{PASS|FAIL|N/A}}",
    "BO": "{{PASS|FAIL|N/A}}",
    "RO": "{{PASS|FAIL|N/A}}"
  },
  "blockers_identified": [
    {
      "type": "{{trustworthiness|experience|authoritativeness|schema}}",
      "description": "{{descriere concretă}}",
      "severity": "{{HIGH|MEDIUM|LOW}}",
      "deadline_days": {{integer}}
    }
  ],
  "action_items_count": {
    "HIGH": {{integer}},
    "MEDIUM": {{integer}},
    "LOW": {{integer}}
  },
  "next_audit_date": "{{YYYY-MM-DD}}",
  "approved_by": "Pafi",
  "tags": ["eeat", "affiliate-seo", "gambling", "ymyl", "{{vertical}}", "{{geo}}"]
}
```

**Cortex save command:**
```
cortex_store(
  collection="seo-audits",
  key="M-133-{{domain}}-{{YYYY-MM}}",
  content={{json_above}},
  tags=["eeat", "m-133", "{{domain}}", "{{vertical}}"]
)
```

---

## Section 4 — Enforcement Loop

### WHERE

Această procedură se aplică obligatoriu pentru:
- **Pre-launch**: orice site afiliat nou în portofoliul SMSads, indiferent de vertical sau GEO, cu minim 7 zile înainte de prima campanie plătită sau de publicarea primelor 10 articole YMYL
- **Quarterly**: site-uri active cu trafic organic > 500 sesiuni/lună — audit la fiecare 90 de zile
- **Post-penalizare**: imediat după orice semnal de penalizare manuală sau scădere bruscă de trafic (> 30% în 7 zile) detectată în Search Console sau Ahrefs
- **Post-update major Google**: în termen de 14 zile după confirmarea unui Core Update sau HCU-related update de Google

### WHEN

| Trigger | Frecvență | Deadline Execuție |
|---|---|---|
| Site nou (pre-launch) | O dată | Cu 7 zile înainte de lansare |
| Quarterly review | La 90 zile | Până la data Q (1 mar, 1 iun, 1 sep, 1 dec) |
| Google Core Update | Ad-hoc | 14 zile de la anunțul oficial |
| Scădere trafic > 30% | Ad-hoc | 72 ore de la detectare |
| Penalizare manuală Search Console | Ad-hoc | 24 ore de la detectare |
| Schimbare echipă editorială (autori noi) | La eveniment | 30 zile de la schimbare |

### HOW

**Violări și consecințe:**

| Violare | Severitate | Consecință |
|---|---|---|
| Scor Trustworthiness < 4 în GEO reglementat (KE, CL, RO) | CRITICAL | Stop imediat campanii; escaladare Pafi în 2 ore |
| Audit omis la launch nou | HIGH | Buget campanie blocat până la audit completat |
| Action items HIGH neimplementate după 30 zile | HIGH | Escaladare Pafi; site marcat "at-risk" în tracker |
| Audit quarterly omis | MEDIUM | Reminder automat; penalizare internă scor proiect |
| Outputs nelivrate sau nelogate în Cortex | MEDIUM | Audit invalidat; trebuie refăcut |
| Schema markup absent pe pagini review YMYL | HIGH | Fix obligatoriu înainte de orice campanie SEO |

### CONNECT

| Procedură | Conexiune | Detalii |
|---|---|---|
| **M-126** | Input | Furnizează inventarul de conținut și arhitectura site-ului pentru audit Step 4 (Content Depth) |
| **M-127** | Input | Furnizează profilul de backlink-uri și strategia de link building — context pentru Step 3 (External Authority) |
| **M-130** | Output → Input | Primește action plan-ul din Step 7; execută implementarea conținutului nou pentru gap-urile de expertiză identificate |
| **TRAIN-S-001** | Parent Rule | Regula-cadru care mandatează standardele de calitate pentru procedurile din domeniul SEO afiliat |

### VERIFY

Înainte de marcarea auditului ca DONE, verifică obligatoriu toate cele 6 puncte:

- [ ] **V1 — Scorecard completat**: Toate 7 dimensiuni au scor numeric 1–5; scor total calculat; rating PASS/CONDITIONAL/FAIL/CRITICAL_FAIL atribuit
- [ ] **V2 — Outputs livrate**: Toate 7 fișiere output (xlsx/md) există în folderul de audit, sunt complete și accesibile
- [ ] **V3 — Cortex logged**: JSON de audit salvat în Cortex cu key `M-133-{{domain}}-{{YYYY-MM}}`; confirmare returned
- [ ] **V4 — GEO compliance verificat**: Checklist GEO-specific din Step 5 completat pentru toate GEO-urile active ale site-ului
- [ ] **V5 — Action plan aprobat**: Dacă rating este CONDITIONAL sau FAIL, action plan trimis la Pafi și aprobat înainte de implementare
- [ ] **V6 — Next audit scheduled**: Data auditului următor înregistrată în tracker (Notion sau Google Calendar) și confirmată

### MODEL ROUTING

| Step | Model Recomandat | Justificare |
|---|---|---|
| Step 1 — Author Credentials Audit | Sonnet | Task structurat, checklist-based; nu necesită raționament complex |
| Step 2 — About/Team Transparency Audit | Sonnet | Evaluare binară (Y/N), crawl + manual check |
| Step 3 — External Authority Signals | Sonnet | Procesare date Ahrefs/Majestic; pattern recognition simplu |
| Step 4 — Content Expertise Depth Audit | Opus | Necesită judecată nuanțată despre calitatea conținutului YMYL; evaluare E-E-A-T complexă |
| Step 5 — Trust Signals Audit | Sonnet | Checklist GEO-specific; verificare conformitate reglementară |
| Step 6 — Competitor Benchmarking | Opus | Analiză comparativă complexă; interpretare date multi-dimensionale; strategic framing |
| Step 7 — Action Plan Synthesis | Opus | Sinteză cross-steps; prioritizare strategică; recomandări cu impact business |
| Cortex Logging | Sonnet | Completare template JSON structurat |

---

## Section 5 — Dependente

| Dependenta | Tip | Cum se Obtine |
|---|---|---|
| Ahrefs (Site Explorer, Content Explorer, Alerts) | Tool plătit — REQUIRED | Cont activ SMSads; credențiale în env-template.md |
| SEMrush (Content Audit, Backlink Analytics, Organic Research) | Tool plătit — REQUIRED | Cont activ SMSads; credențiale în env-template.md |
| Majestic SEO (TF/CF, Bulk Checker) | Tool plătit — REQUIRED | Cont activ SMSads; credențiale în env-template.md |
| Screaming Frog SEO Spider | Tool desktop — REQUIRED | Instalat pe MacM4; licență activă |
| Google Search Console | Gratuit — REQUIRED | Acces verificat pentru domeniul auditat; adăugare property dacă lipsă |
| Copyscape | Tool plătit (per check) | Credențiale în env-template.md; buget alocat per audit |
| Schema.org Markup Validator | Gratuit — REQUIRED | https://validator.schema.org — acces direct |
| Google Search Quality Evaluator Guidelines | Gratuit — REQUIRED | Ultima versiune PDF de la https://static.googleusercontent.com |
| Cortex MCP Server | Infrastructură internă — REQUIRED | Activ pe MacM4; verificare: `cortex_list_collections()` |
| M-126 — Content Inventory | Procedură dependentă — INPUT | Execută M-126 înainte de Step 4; output: inventar URL + word count |
| M-127 — Link Building Profile | Procedură dependentă — INPUT | Execută M-127 sau pull date din Ahrefs dacă M-127 recent (< 30 zile) |
| M-130 — Content Gap + Production | Procedură dependentă — OUTPUT | Trimite action plan Step 7 către M-130 pentru execuție conținut |
| Licențe BCLB/SJC/ONJN (numere licență operatori) | Date externe — REQUIRED | Solicitat de la managerii de cont ai operatorilor parteneri |
| Google Sheets / Notion | Infrastructură — REQUIRED | Acces cont SMSads; template-uri pre-populat în Drive |

---

## Section 6 — Metrics

| Metric | Target | Frecventa Masurare | Sursa |
|---|---|---|---|
| E-E-A-T Scorecard Total | ≥ 28/35 (PASS) pentru site-uri mature (> 12 luni) | Quarterly | Output Step 7 |
| Trustworthiness Score | ≥ 4/5 pe toate site-urile YMYL | Quarterly | Output Step 5 |
| Authoritativeness — Ahrefs DR | ≥ 30 (site > 12 luni); ≥ 15 (site < 6 luni) | Lunar | Ahrefs Site Explorer |
| Majestic Trust Flow | ≥ 20 (site > 12 luni); ≥ 12 (site < 6 luni) | Lunar | Majestic |
| % Pagini YMYL cu Author Byline real | ≥ 80% | Quarterly | Screaming Frog + manual |
| % Pagini review cu Schema `Review` markup | 100% | Quarterly | Schema Validator + Screaming Frog |
| GEO Compliance Rate | 100% (zero defecte critice) | Quarterly + post-update | Output Step 5 |
| Action Items HIGH completate în SLA | ≥ 90% în 30 zile | Lunar | Notion tracker |
| Timp mediu execuție audit complet (toate 7 steps) | ≤ 8 ore/site | Per execuție | Time tracking intern |
| Trafic organic post-fix (60 zile) | ≥ +10% față de baseline pre-audit | La 60 zile post-implementare | Google Search Console |
| Penalizări manuale active | 0 | Lunar | Google Search Console > Manual Actions |
| Pagini YMYL thin content (< 800 cuvinte) | 0 pe pagini strategice | Quarterly | Screaming Frog + SEMrush Content Audit |

---

## Checklist Pre-Publicare

- [x] Toate 7 secțiuni FORGE prezente și completate
- [x] ID unic M-133 atribuit, non-duplicat față de index master
- [x] Versiune 2.0 specificată cu dată creare 2026-03-05
- [x] Conexiuni M-126, M-127, M-130 documentate în Section 4 CONNECT
- [x] Regula asociată TRAIN-S-001 menționată în header și Section 4
- [x] Trigger definit: Quarterly + pre-launch orice site nou
- [x] Minim 5 pași cu Tool, Output, Checkpoint (livrat: 7 pași)
- [x] Tool-uri concrete specificate: Ahrefs, SEMrush, Majestic, Screaming Frog, Schema Validator, Copyscape, SQEG
- [x] E-E-A-T Scorecard cu scală 1–5 și thresholds PASS/CONDITIONAL/FAIL/CRITICAL_FAIL inclus
- [x] Thresholds per dimensiune (blocker-uri) definite explicit
- [x] Gambling-specific requirements: BCLB (KE), SJC (CL), ONJN (RO), responsable gambling, 18+, afiliat disclosure
- [x] Referințe SpamBrain și HCU incluse în Section 1
- [x] Checklist GEO-specific per dimensiune (PE, KE, CL, BO, RO) în Step 5
- [x] JSON Cortex template complet cu toate câmpurile necesare
- [x] VERIFY cu 6 checkpoints obligatorii (V1–V6)
- [x] Model Routing table cu justificări (Opus vs Sonnet per step)
- [x] Section 5 Dependente cu toate tool-urile și procedurile conexe
- [x] Section 6 Metrics cu 12 metrici, targets, frecvență și sursă
- [x] Limba română utilizată consistent; termeni tehnici în engleză acceptați
- [x] Fișier salvat la path: `/Users/pafi/.nexus/procedures/training/affiliate/M-133-eeat-sitewide-audit.md`
