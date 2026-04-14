---
type: procedure
created: 2026-03-17
status: active
slug: m-118-gambling-compliance-platforms
tags: [procedure, nexus]
---

# Gambling Affiliate Compliance & Platform Selection — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Framework de conformitate legala si selectie platforme specifice gambling pentru piete internationale (Peru, Kenya, Chile + extensii), cu audit structurat 10-punct GO/NO-GO, matrice GEO compliance, si deal evaluation rev-share vs CPA.

---

## 1. Problema

M-006 listeaza doar retele CPA generice — zero platforme gambling-specifice (Income Access, SoftSwiss, EveryMatrix, Kindred, Bet365 Affiliates). Compliance gambling e go/no-go per GEO, nu nice-to-have. Peru are ASJ (Asociacion Peruana de Apuestas Deportivas / Direccion General de Juegos de Casino y Maquinas Tragamonedas) cu licentiere din 2024. Kenya are BCLB (Betting Control and Licensing Board of Kenya) cu restrictii stricte advertising. Chile are SJC (Superintendencia de Juegos de Casino, sub Ley 19.995) cu framework nou 2025. Oferte fara licenta locala = ban cont + clawback comisioane. Fara procedura: risc legal, payout-uri 40-80% mai mici pe retele generice, pierdere deal-uri directe.

**Situatii acoperite:**
- Evaluare statut legal gambling per GEO (licentiat/grey/ilegal)
- Aplicare la platforme gambling-specifice
- Setup deal-uri directe cu operatori
- Compliance audit pre-lansare campanie (10-punct GO/NO-GO)
- Rev-share vs CPA decision framework cu calcul EV pe 6 luni
- Re-audit la schimbare reglementari locale

**Ce se intampla fara procedura:**
- Campanie pe GEO fara licenta → ban cont, clawback comisioane, risc legal personal
- Platforme generice in loc de gambling-specifice → payout-uri 40-80% mai mici
- Compliance omis → sanctiuni ASJ/BCLB/SJC, pierdere relatie operator
- Deal CPA in loc de rev-share pe jucatori high-LTV → pierdere venit pe termen lung
- Materiale fara disclaimer → suspendare campanie, penalitati financiare

## 2. Procedura

### Pas 1: GEO LEGAL STATUS MAP

Per piata target, documenteaza statutul legal complet:

| Dimensiune | Detaliu necesar | Exemplu Peru |
|-----------|----------------|--------------|
| Statut gambling | Licentiat complet / partial reglementat / grey / ilegal | Licentiat complet (din 2024) |
| Autoritate reglementare | Nume complet + acronim + website | Direccion General de Juegos de Casino y Maquinas Tragamonedas (ASJ) — mincetur.gob.pe |
| Legislatie primara | Lege/decret principal | Ley 31557 (2022) + DS 005-2024-MINCETUR |
| Cerinte licenta afiliat | Daca exista cerinte separate de operator | Nu exista licenta separata afiliat; opereaza sub umbrela operatorului licentiat |
| Restrictii publicitate | Per canal (Meta, Google, TikTok, native, SMS) | Meta: blocat gambling; TikTok: restrictionat cu whitelist; Google: licentiat doar; Native: permis cu disclaimer |
| Sanctiuni recente | Ultimele 12 luni | ASJ a sanctionat 3 operatori fara licenta (Q3 2025) |
| Age gate obligatoriu | Varsta minima + cerinte verificare | 18+ obligatoriu; age gate pe LP si pre-depozit |
| Disclaimer obligatoriu | Text exact cerut de regulator | "Juega responsablemente. Solo mayores de 18. Regulado por ASJ." |

**Matrice GEO Compliance Status:**

| GEO | Autoritate | Status | Licenta Afiliat | Advertising Restrictions | Risk Level |
|-----|-----------|--------|-----------------|-------------------------|------------|
| Peru (PE) | ASJ — Direccion General de Juegos de Casino y Maquinas Tragamonedas | VERDE — licentiat complet | Sub umbrela operator | Meta blocat, TikTok whitelist, Native permis | LOW |
| Kenya (KE) | BCLB — Betting Control and Licensing Board of Kenya | VERDE — licentiat complet | Nu cerinta separata | Meta restrictionat, TikTok permis, Native permis | MEDIUM (restrictii advertising stricte) |
| Chile (CL) | SJC — Superintendencia de Juegos de Casino (Ley 19.995 + Ley 21.634) | GALBEN — framework nou 2025 | In curs de definire | Meta restrictionat, TikTok nou permis, Native permis | MEDIUM (legislatie in evolutie) |

**Tool**: Claude Opus — research legislatie + registre publice autoritati reglementare (ASJ, BCLB, SJC).
**Output**: Document `geo-legal-status-{GEO}.md` cu matrice completa status + tabel restrictii publicitate.
**Checkpoint**: Toate 8 dimensiunile completate per GEO. Status VERDE/GALBEN/ROSU atribuit. Daca ROSU = BLOCKER, nu se trece la Pas 2 pentru acel GEO.

### Pas 2: PLATFORM IDENTIFICATION & SCORING

Identifica si evalueaza platforme gambling-specifice per GEO:

| Platforma | GEO Coverage | Payout Models | Min Volume | Approval Timeline | Gambling Vertical Fit |
|-----------|-------------|---------------|------------|-------------------|----------------------|
| Income Access (incomeaccess.com) | Global, strong LATAM | CPA, Rev-share, Hybrid | 50 FTD/luna | 2-4 saptamani (cu call screening) | Tier 1 — gold standard gambling |
| SoftSwiss Affiliates | Global, strong emerging | CPA, Rev-share | 30 FTD/luna | 1-2 saptamani | Tier 1 — tech-forward |
| EveryMatrix | LATAM + Africa focus | Rev-share dominant | 20 FTD/luna | 2-3 saptamani | Tier 1 — emerging markets |
| Kindred Affiliates (Unibet/32Red) | Selective GEO | CPA, Rev-share | 100 FTD/luna | 3-4 saptamani (strict) | Tier 2 — premium brands |
| Bet365 Affiliates | Global restrictiv | CPA predominant | 200 FTD/luna | 4-6 saptamani | Tier 2 — high bar, high payout |
| BetConstruct | Emerging markets | CPA, Hybrid | 15 FTD/luna | 1 saptamana | Tier 2 — easy entry |
| Pin-Up Partners | CIS + LATAM | CPA, Rev-share | 10 FTD/luna | 3-5 zile | Tier 3 — volume play |
| 1xBet Partners | Global agresiv | CPA, Rev-share, Hybrid | 10 FTD/luna | 1-3 zile | Tier 3 — grey market risk |

**Regula selectie**: Minimum 2 platforme Tier 1 active + 1 Tier 2 ca backup per GEO. Platforme Tier 3 doar pe GEO-uri grey cu due diligence suplimentar.

**Tool**: Claude Opus — research platforme + comparative analysis; M-006 (CPA networks) ca baza.
**Output**: Document `platform-evaluation-{GEO}.md` cu scoring per platforma (GEO fit, payout, volume, timeline).
**Checkpoint**: Minimum 3 platforme evaluate per GEO. Cel putin 2 Tier 1 identificate. Zero platforme fara verificare licenta operator.

### Pas 3: APPLICATION PROCESS

Per platforma selectata, urmeaza procesul structurat:

**(a) Pre-Application Prep:**
- CV afiliat gambling-specific: volume dovedite, GEO-uri active, canale trafic, compliance track record
- Site/landing page de referinta cu disclaimer regulator local vizibil
- Documentatie legala business entity (daca ceruta)

**(b) Application Steps:**
- Creare cont cu date business corecte (match cu documentatie)
- Upload CV + link LP referinta
- Completare questionnaire vertical (gambling experience, traffic sources, GEO focus)
- Pregatire pentru call screening (Income Access, SoftSwiss, Kindred fac verificare telefonica)

**(c) Post-Approval:**
- Setup tracking links per oferta
- Configurare postback URLs in Voluum (M-121)
- Test conversion flow end-to-end inainte de trafic real

**Tool**: Browser (aplicare), Claude Opus (CV generation + call prep), M-121 (tracking setup).
**Output**: Document `application-checklist-{platforma}.md` cu status per pas (DONE/PENDING/BLOCKED).
**Checkpoint**: Cont aprobat cu tracking functional. Test conversion trimis si confirmat in Voluum. Zero conturi cu date incomplete.

### Pas 4: DEAL STRUCTURE EVALUATION

Per oferta disponibila, calculeaza Expected Value pe 6 luni:

**CPA Model:**
- EV(CPA) = Payout per FTD x Conversii estimate pe 6 luni
- Avantaj: predictibil, cash flow rapid
- Risc: pierdere upside pe jucatori high-LTV

**Rev-Share Model:**
- EV(Rev-share) = (Depozit mediu x Frecventa joc x 6 luni x Rev-share%) - Negativity carryover
- Avantaj: upside nelimitat pe jucatori activi
- Risc: negativity carryover, variance, platforma poate modifica T&C

**Decision Framework:**

| Conditie | Recomandare | Rationale |
|----------|-------------|-----------|
| LTV jucator estimat > 4x CPA offer | Rev-share | Upside semnificativ peste CPA fix |
| LTV jucator estimat 2-4x CPA offer | Hybrid (CPA + rev-share redus) | Balans risc/reward |
| LTV jucator estimat < 2x CPA offer | CPA | Cash flow rapid, risc minim |
| GEO nou fara date LTV | CPA primele 30 zile, apoi reevaluare | Acumuleaza date inainte de commitment rev-share |
| Operator cu negativity carryover agresiv | CPA sau hybrid fara carryover | Protejeaza contra lunilor negative |

**Regula**: Dupa 30 zile de volume dovedit, negociaza deal personalizat (per M-112).

**Tool**: Claude Opus — calcul EV + scenario modeling; spreadsheet pentru proiectii.
**Output**: Document `deal-evaluation-{platforma}-{oferta}.md` cu calcul EV ambele modele + recomandare.
**Checkpoint**: EV calculat pe ambele modele per oferta. Recomandare justificata cu date. Zero oferte acceptate fara calcul EV prealabil.

### Pas 5: COMPLIANCE AUDIT PRE-LANSARE (10-PUNCT GO/NO-GO)

Inainte de a rula trafic pe orice campanie gambling, completeaza OBLIGATORIU auditul structurat de 10 puncte. Scor 10/10 = GO. Sub 10/10 = BLOCKER absolut.

**Tabel Audit Compliance 10-Punct:**

| # | Criteriu | Verificare | GO | NO-GO |
|---|----------|------------|-----|-------|
| 1 | Licenta operator valida in GEO target | Verificat in registrul public al autoritatii locale (ASJ/BCLB/SJC) | Licenta activa, numar vizibil | Licenta expirata / inexistenta / nesigura |
| 2 | Disclaimer regulator local pe TOATE materialele | LP, creative, funnel — text conform cerinta autoritatii | Disclaimer prezent si corect pe fiecare material | Lipsa pe orice material = FAIL |
| 3 | Age gate (18+) functional pe LP operator | Test manual age gate pe LP + verificare ca blocheaza sub 18 | Age gate prezent si functional | Absent sau bypass-able |
| 4 | Payment methods promovate sunt legale in GEO | Cross-check metode din copy vs lista metode legale per GEO | Toate metodele promovate legale si active | Orice metoda ilegala sau dezactivata |
| 5 | T&C operator — clawback conditions revizuite | Citit integral sectiunea clawback/chargeback din T&C | Conditii clare, acceptabile, documentate | Conditii abuzive sau neclare |
| 6 | Zero promisiuni castiguri garantate in materiale | Scan complet toate copy-urile (LP, ads, funnel) | Nicio propozitie implica castig garantat | Orice formulare sugerand castig sigur |
| 7 | Nu imagini/referinte la minori | Audit vizual toate creative-urile | Zero imagini cu persoane sub 18 (sau aparent sub 18) | Orice imagine ambigua = FAIL |
| 8 | Responsible gambling mention prezent | "Juega responsablemente" / "Gamble responsibly" / echivalent local | Prezent pe LP + cel putin 1 creative | Absent din orice material public |
| 9 | Tracking compliant (no PII leak) | Verificare ca tracking-ul nu transmite date personale fara consimtamant | GDPR/local compliant, no PII in URLs | PII in tracking URLs sau cookies fara consent |
| 10 | Creative compliance per canal | Verificare ca fiecare creative respecta regulile canalului (M-119) | Compliance check per canal documentat | Orice creative nepotrivit canalului |

**Scor final**: ___/10 — GO / NO-GO

**La NO-GO**: Lista exacta a punctelor failed + actiuni corective + pas de revenire (ex: "Punct 6 FAIL → revenire Pas 3 M-116 pentru rewrite creative").

**Tool**: Claude Opus — compliance audit; registre publice autoritati (ASJ, BCLB, SJC); M-116 (materiale), M-119 (canal compliance).
**Output**: Document `compliance-audit-{GEO}-{campanie}.md` cu tabel 10-punct completat + scor + decizie GO/NO-GO.
**Checkpoint**: Scor 10/10 per campanie. Document semnat (timestamp + operator). Zero campanii lansate cu scor sub 10/10.

### Pas 6: DIRECT DEAL ESCALATION

Dupa 30-60 zile cu volum dovedit pe platforma:

**(a) Prep Pitch:**
- Compileaza date: volum FTD livrat, CR per GEO, calitate trafic (retentie jucatori), canale utilizate
- Benchmark-eaza contra industrie (CPA mediu per GEO, rev-share standard)
- Identifica leverage points: GEO exclusiv, volum crescator, calitate superioara

**(b) Negociere:**
- Contact account manager cu pitch structurat (per M-112)
- Target: rev-share +5-10% peste standard SAU CPA bonus 20-30%
- Negociaza: geo-exclusive terms, higher cap, weekly payout (vs monthly), dedicated landing pages

**(c) Formalizare:**
- Document deal direct cu termeni clari (payout, frecventa, exclusivitate, durata)
- Setup tracking separat pentru deal direct vs standard
- Calendar review: re-negociere la 90 zile

**Tool**: Claude Opus — pitch generation + negotiation strategy; M-112 (commission negotiation).
**Output**: Document `deal-direct-{platforma}-{operator}.md` cu termeni negociati + tracker status.
**Checkpoint**: Deal direct activ cu termeni documentati. Payout improvement minim +10% vs standard. Re-negotiation scheduled la 90 zile.

## 3. Cortex Logging

```json
{
  "text": "M-118 Gambling Compliance & Platform Selection — Framework conformitate legala, selectie platforme gambling-specifice, audit 10-punct GO/NO-GO, deal evaluation CPA vs rev-share per GEO",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-118",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "gambling-compliance",
    "pipeline": "A",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "gambling", "compliance", "platforms", "training", "Peru", "Kenya", "Chile", "ASJ", "BCLB", "SJC", "income-access", "softswiss"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- Pre-lansare orice campanie gambling (obligatoriu)
- La onboarding platforma noua gambling
- La intrare pe GEO nou cu vertical gambling
- La schimbare reglementari locale

### WHEN
- La fiecare GEO/oferta noua gambling — Pas 1-5 complet
- La fiecare platforma noua — Pas 2-3 complet
- La fiecare deal nou — Pas 4 complet
- Trimestrial: re-audit GEO legal status (Pas 1) + re-validare compliance (Pas 5)
- La orice sanctiune publicata de ASJ/BCLB/SJC — re-audit imediat

### HOW (violation detection)
- Campanie lansata fara compliance audit 10/10 (Pas 5) → **VIOLATION CRITICA** (shutdown imediat)
- Oferta selectata doar din retele generice fara check gambling-specific (Pas 2) → **VIOLATION** (blocker Pas 3)
- Deal acceptat fara calcul EV pe ambele modele (Pas 4) → **VIOLATION** (re-evaluare obligatorie)
- GEO legal status map mai veche de 90 zile → **WARNING** (re-audit in 7 zile)
- Materiale fara disclaimer regulator local → **VIOLATION CRITICA** (retragere imediata materiale)
- Platforma Tier 3 utilizata fara due diligence suplimentar → **VIOLATION** (audit escalat)
- Operator fara licenta activa verificata → **VIOLATION CRITICA** (zero trafic pana la rezolvare)

### CONNECT
- M-002 (Program Evaluation) — framework evaluare programe afiliat
- M-006 (CPA Networks) — completeaza cu platforme gambling-specifice
- M-007 (Offer Selection) — criterii selectie oferte, adaptat gambling
- M-112 (Commission Negotiation) — tactici negociere deal direct la Pas 6
- M-114 (GEO Scoring) — scoring piete, input la Pas 1
- M-116 (Localization Engine) — materiale localizate ce necesita compliance
- M-119 (TikTok + Native Ads) — compliance per canal la Pas 5 punct 10
- M-120 (WhatsApp/SMS Funnel) — funnel compliance gambling
- M-121 (Server-Side Tracking) — tracking setup la Pas 3

### VERIFY
1. Matrice GEO compliance existenta cu toate 8 dimensiunile completate si status atribuit — verificare document `geo-legal-status-{GEO}.md` existent si datat < 90 zile
2. Minimum 2 platforme gambling Tier 1 cu cont activ si tracking functional — verificare conturi active + test postback
3. Compliance audit 10/10 documentat per campanie inainte de orice trafic — verificare document `compliance-audit-{GEO}-{campanie}.md` cu scor 10/10 si timestamp
4. EV calculat pe ambele modele (CPA + rev-share) per oferta — verificare document `deal-evaluation-{platforma}-{oferta}.md` cu calcule complete
5. Zero campanii active pe GEO cu status ROSU — cross-check matrice GEO vs campanii active in Voluum
6. Toate deal-urile directe cu re-negotiation scheduled — verificare calendar review la 90 zile per deal

### MODEL ROUTING
- **Claude Opus**: Analiza legala GEO, compliance audit 10-punct, calcul EV, pitch generation, negotiation strategy (complex reasoning + legal nuance)
- **Claude Sonnet**: Checklist-uri aplicare, tracking setup, format conversions, status updates (cost optimization)

## 5. Dependente

- **Tools**: Registre publice autoritati reglementare (ASJ, BCLB, SJC), Income Access, SoftSwiss, EveryMatrix, Claude Opus, Voluum, Google Search
- **Proceduri input**: M-114 (GEO scoring — piete prioritare), M-006 (CPA networks — baza retele generice)
- **Proceduri framework**: M-002 (program evaluation), M-007 (offer selection), M-112 (commission negotiation), M-121 (server-side tracking)
- **Proceduri adjacent**: M-116 (localization engine — materiale ce necesita compliance), M-119 (TikTok + native — compliance per canal), M-120 (WhatsApp/SMS funnel)
- **Data sources**: Registre publice ASJ (mincetur.gob.pe), BCLB (bclb.go.ke), SJC (sjc.cl), operator T&C, industry benchmarks
- **Output consumed by**: M-116 (compliance requirements per GEO), M-119 (canal compliance check), M-120 (funnel compliance), M-121 (tracking compliance)

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| GEO compliance audit pass rate | 100% pre-lansare (10/10) | Per campanie | compliance-audit doc |
| Platforme gambling Tier 1 active | Min 2 per GEO | Lunar | Platform dashboard |
| Timp pana la deal direct | < 60 zile de la primul FTD | Per platforma | Deal tracker |
| Clawback rate din compliance issues | 0% | Lunar | Platform payout reports |
| Rev-share vs CPA EV accuracy | ±15% vs actual la 6 luni | Trimestrial | EV calc vs actual revenue |
| GEO legal status freshness | < 90 zile | Trimestrial | geo-legal-status docs timestamp |
| Compliance re-audit post-sanctiune | < 48h de la publicare sanctiune | Per eveniment | Audit trail |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6)
- [x] Pasi numerotati secvential (1-6) cu Tool, Output (artifact name), si Checkpoint (criteriu masurabil) per pas
- [x] Tools specificate per pas (nu doar lista generica)
- [x] KPIs masurabile cu target numeric, frecventa, si sursa
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Dependente listate (tools + proceduri input/framework/adjacent + data sources + output consumed by)
- [x] Metrics cu target, frecventa si sursa masurare (7 metrici)
- [x] Autoritati legale cu nume complet per GEO (ASJ, BCLB, SJC — full names)
- [x] Compliance audit 10-punct GO/NO-GO ca tabel structurat (Pas 5)
- [x] Matrice GEO compliance cu status VERDE/GALBEN/ROSU
- [x] Decision framework CPA vs rev-share cu conditii si rationale
