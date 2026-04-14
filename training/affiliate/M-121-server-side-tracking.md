---
type: procedure
created: 2026-03-17
status: active
slug: m-121-server-side-tracking
tags: [procedure, nexus]
---

# Server-Side Tracking & First-Party Data Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Implementare tracking server-side (S2S) si strategie first-party data pentru atribuire corecta in era post-cookie (2026+), cu salted hashing pentru PII si hybrid tracking architecture.

---

## 1. Problema

M-008 configureaza Voluum cu tracking cookie-based standard. Post iOS 17+ si Chrome cookie deprecation, tracking-ul cookie-based pierde 20-40% din conversii (date sub-raportate). Server-side tracking + click ID passthrough + salted hash identifiers + cohort modeling = standard 2026. Fara procedura: date de atribuire incorecte, decizii de optimizare bazate pe date incomplete, ROI aparent mai mic decat real → kill prematur campanii profitabile.

**Situatii acoperite:**
- Migrare de la cookie-based la server-side tracking
- Setup Voluum S2S (server-to-server) postback cu click ID passthrough
- First-party data collection compliant cu salted hashing
- Cross-device attribution (deterministic + probabilistic)
- Cookie fallback pentru micro-conversions pe browsere compatibile
- Validare si reconciliation continua (Voluum vs operator)

**Ce se intampla fara procedura:**
- 20-40% conversii pierdute din tracking → decizii bazate pe date incomplete
- Kill prematur campanii profitabile (ROI aparent mai mic decat real)
- Cookie-based tracking pe iOS Safari = 100% loss pe 3rd party cookies
- PII stocat in plaintext sau cu hash nesigur → data breach risk
- Fara reconciliation → discrepante operator vs tracker nedetectate luni de zile
- Cross-device conversions complet invizibile → sub-estimare performanta mobile campaigns

## 2. Procedura

### Pas 1: AUDIT TRACKING CURENT

Evalueaza setup-ul existent (M-008) si cuantifica pierderile din cookie blocking:

1. **Cookie loss estimation**: compara conversii Voluum cookie-based vs conversii confirmate pe operator dashboard. Delta = cookie loss estimate
2. **Device split per GEO**: Kenya 85% mobile (Android dominant), Peru 78% mobile, Chile 72% mobile → mobile = high privacy impact
3. **Browser breakdown**: iOS Safari (100% 3rd party cookie block), Chrome (ITP partials → full phase-out 2026), Firefox (ETP strict)
4. **Channel loss ranking**: TikTok > Meta > Google > Native pentru cookie attribution issues (TikTok cel mai afectat)
5. **Current S2S status**: exista postback URLs configurate? Sunt active sau dormant?

**Tool**: Voluum reports (conversion comparison), operator dashboards, Google Analytics device reports, M-008 (current tracking config).
**Output**: Document `tracking-audit-{GEO}.md` cu gap report: % conversii pierdute per canal, per device, per browser. Estimare revenue impact.
**Checkpoint**: Gap report completeaza toate 5 punctele. Estimare loss in % si in revenue ($). Cel putin 7 zile de date analizate pentru statistical significance.

### Pas 2: VOLUUM S2S MIGRATION

Configureaza tracking server-to-server cu click ID passthrough. Aceasta este arhitectura core care elimina dependenta de cookies.

**S2S Tracking Architecture — Click Flow:**

```
+------------------+     +-------------------+     +--------------------+     +------------------+     +------------------+
|   Traffic Source  |     |    Voluum Tracker  |     |   Landing Page     |     |    Operator       |     |  Voluum Postback  |
|   (Meta/TikTok/  | --> |                   | --> |                    | --> |   (Betting Site)  | --> |   Endpoint        |
|    Google/SMS)    |     |   Generates:      |     |   Receives:        |     |   Receives:       |     |   Receives:       |
|                   |     |   - click_id      |     |   - click_id (URL) |     |   - click_id via  |     |   - click_id      |
|   Passes:        |     |   - visitor_id    |     |   - source params  |     |     registration  |     |   - payout        |
|   - ad_id        |     |   - cost data     |     |                    |     |     link subID    |     |   - transaction_id|
|   - campaign_id  |     |   - geo/device    |     |   Passes to        |     |                   |     |   - event type    |
|   - creative_id  |     |                   |     |   operator link:   |     |   At conversion:  |     |                   |
|                   |     |   Logs:           |     |   - click_id as    |     |   fires S2S POST  |     |   Logs:           |
|                   |     |   - click event   |     |     subID param    |     |   to Voluum with  |     |   - conversion    |
|                   |     |   - all params    |     |   - aff_id         |     |   click_id +      |     |   - attribution   |
|                   |     |                   |     |   - offer_id       |     |   payout + txn_id |     |   - revenue       |
+------------------+     +-------------------+     +--------------------+     +------------------+     +------------------+
     CLICK                   TRACK + REDIRECT          PASS-THROUGH              CONVERT + FIRE          ATTRIBUTE + LOG
```

**Data passed at each step:**
| Step | From → To | Data Passed | Method |
|------|-----------|-------------|--------|
| 1. Click | Source → Voluum | ad_id, campaign_id, creative_id, cost | URL parameters (redirect) |
| 2. Track | Voluum → LP | click_id, visitor_id, geo, device, source | URL parameters (redirect) |
| 3. Pass | LP → Operator | click_id (as subID), aff_id, offer_id | URL parameters (registration link) |
| 4. Convert | Operator → Voluum | click_id, payout, transaction_id, event_type | S2S HTTP POST (server-side) |
| 5. Attribute | Voluum internal | Matches click_id to original click | Server-side lookup (no cookies) |

**Setup steps:**
1. Activeaza Voluum S2S postback URLs per oferta (Settings → Postback URLs → Generate)
2. Verifica ca click ID este generat automat per click (Voluum default: `{clickid}` macro)
3. Configureaza LP sa paseze click ID la operator via URL parameter (LP registration link include `subid={clickid}`)
4. Furnizeaza operator-ului postback URL template: `https://postback.voluum.com/postback?cid={clickid}&payout={payout}&txid={txid}`
5. Operator configureaza fire postback S2S la Voluum la fiecare conversie event (registration, FTD, deposit)

**Rezultat:** atribuire fara cookies, functioneaza pe orice browser/device/OS.

**Tool**: Voluum (S2S postback config), operator partner dashboard, M-008 (existing tracking setup).
**Output**: Document `s2s-config-{GEO}-{operator}.md` cu postback URLs, click ID macros, LP parameter mapping, operator integration status.
**Checkpoint**: Test click E2E complet (click → LP → operator → postback → Voluum conversie logged cu click_id match). Minimum 3 test conversii confirmate. Postback response code 200 pe toate testele.

### Pas 3: FIRST-PARTY DATA COLLECTION CU SALTED HASHING

Setup colectare date first-party pe LP-uri proprii cu hashing securizat pentru PII.

**Data collection framework:**
1. **Form opt-in** (email/WhatsApp/telefon) cu consimtamant explicit (conform M-120 Pas 3)
2. **localStorage/sessionStorage** per user session (click_id, source, timestamp, page_views)
3. **Salted hash identifiers** pentru cross-session matching

**IMPORTANT — Salted Hashing (audit finding):**

Plain SHA256 pe email/phone este VULNERABIL la rainbow table attacks. Un atacator cu o baza de date de email-uri comune poate pre-computa hash-urile si match-ui.

Cerinta OBLIGATORIE: foloseste HMAC-SHA256 cu secret key (salt) sau bcrypt pentru hashing PII:
- **HMAC-SHA256**: hash = HMAC(secret_key, email_normalized) — secret key stocat server-side, NICIODATA in client code
- **bcrypt** (alternativa): mai lent = mai sigur, dar nu permite cross-system matching fara re-hash
- **Recomandare**: HMAC-SHA256 pentru cross-session/cross-device matching (permite consistent hash cu acelasi key), bcrypt pentru storage-only (authentication)

**Normalizare pre-hash:**
- Email: lowercase, trim whitespace, remove dots in gmail (john.doe → johndoe)
- Phone: E.164 format (+51XXXXXXXXX Peru, +254XXXXXXXXX Kenya, +56XXXXXXXXX Chile)

**Compliance:**
- Stocheaza PII doar cu consimtamant explicit documentat
- Delete la cerere in timeframe legal (Peru: 10 zile, Kenya: 30 zile, Chile: 15 zile)
- Encrypt PII at rest (AES-256 minim)
- NU folosi browser fingerprinting — grey area legal in LATAM 2026, risc sanctiuni
- Secret key (salt) pentru HMAC: rotat trimestrial, stocat in secrets manager (nu in code, nu in config files)

**Tool**: Claude Opus (compliance review, hashing architecture design), M-118 (compliance framework), M-120 (opt-in mechanism).
**Output**: Document `first-party-data-{GEO}.md` cu data collection schema, hashing implementation spec (HMAC-SHA256), normalization rules, compliance checklist, key rotation schedule.
**Checkpoint**: Hashing implementat ca HMAC-SHA256 (nu plain SHA256). Secret key stocat in secrets manager. Test: hash acelasi email de 2 ori → identic. Hash fara key → diferit. PII deletion tested E2E (request → confirm deleted in 48h).

### Pas 4: CROSS-DEVICE ATTRIBUTION

Pentru userii care click pe mobil si convertesc pe desktop (sau invers) — estimare 15-25% din conversii pe piete mobile-first.

**Attribution methods (in ordine de acuratete):**

| Metoda | Acuratete | Cerinte | Best for |
|--------|-----------|---------|----------|
| Deterministic (operator confirm) | ~99% | Operator fire-eaza postback cu user_id cross-device | High-volume operators |
| First-party login match | ~95% | User se inregistreaza cu acelasi email/telefon pe ambele devices | Funnels cu registration |
| Probabilistic (Voluum) | ~70% | IP + user agent + timestamp proximity | Fallback universal |

**Setup:**
1. **Deterministic**: activeaza cross-device postback cu operator-ul (operator confirma ca user X pe mobile = user X pe desktop via internal ID)
2. **First-party**: match hashed email/phone (HMAC-SHA256 din Pas 3) cross-session
3. **Probabilistic**: activeaza cross-device reports in Voluum (Settings → Attribution → Cross-device matching ON)

**Tool**: Voluum (cross-device reports, attribution settings), operator API (deterministic matching).
**Output**: Document `cross-device-config-{GEO}.md` cu metode active per operator, expected match rates, fallback chain.
**Checkpoint**: Cross-device report functional in Voluum. Cel putin una din metodele deterministic sau first-party activa per operator. Test: simulare cross-device conversion (click mobile → convert desktop) — atribuit corect.

### Pas 5: HYBRID TRACKING (S2S + COOKIE FALLBACK)

Nu dezactiva complet cookie-based tracking — foloseste-l complementar pentru micro-conversions.

**Architecture dual:**

| Layer | Metoda | Scop | Data Captured |
|-------|--------|------|---------------|
| Primar | S2S postback | Conversii (registration, FTD, deposit) | click_id, payout, txn_id, event_type |
| Secundar | Cookie + JS pixel | Micro-conversions (page views, scroll, time on LP) | page_url, scroll_depth, time_on_page, click_heatmap |
| Tertiar | First-party data | Cross-session identity | hashed_email, hashed_phone, session_count |

**Setup:**
1. **S2S = PRIMAR** pentru atribuire conversii (Pas 2)
2. **Cookie = SECUNDAR** pentru behavior tracking pe LP (scroll depth, time on page, click heatmap)
3. **JavaScript pixel** pe LP pentru micro-conversion tracking
4. **Consent banner** per GEO conform legislatie locala (din M-118):
   - Peru: banner + opt-in checkbox
   - Kenya: banner informativ + opt-out option
   - Chile: banner + opt-in explicit

Configurare: Voluum dual mode — S2S conversions + JS pixel behavior. Ambele raporteaza in acelasi dashboard.

**Tool**: Voluum (dual mode config), consent banner tool (CookieYes/Osano), M-118 (compliance per GEO).
**Output**: Document `hybrid-tracking-{GEO}.md` cu architecture diagram, consent banner config, dual mode setup, data flow per layer.
**Checkpoint**: S2S postback functional (din Pas 2). JS pixel firing corect pe LP (verificat in browser DevTools). Consent banner afișat pre-tracking pe prima vizita. Micro-conversion data apare in Voluum reports.

### Pas 6: VALIDATION + ONGOING MONITORING

Validare initiala si monitoring continuu pentru integritate tracking:

**Validare initiala (one-time):**
1. Trimite 10 test conversii prin fiecare canal (Meta, TikTok, SMS, WhatsApp) si verifica ca apar corect in Voluum cu atribuire S2S
2. Verifica click_id match pe fiecare test conversion (click_id in Voluum click log = click_id in postback)
3. Cross-device test: 3 conversii simulate cross-device → atribuite corect

**Monitoring continuu:**

| Check | Frecventa | Threshold | Action daca fail |
|-------|-----------|-----------|-----------------|
| Voluum vs operator conversion delta | Saptamanal | <5% | Investigare postback failures |
| Postback fail rate | Daily | <2% | Check endpoint uptime, retry logic |
| Monthly reconciliation (factura vs Voluum) | Monthly | ±3% | Escalare la operator + audit |
| Cross-device match rate | Monthly | >30% | Review attribution methods |
| Consent banner compliance | Trimestrial | 100% pages | Fix missing banners |
| HMAC key rotation | Trimestrial | Key age <90 days | Rotate key, re-hash active records |

**Alerta automata:**
- Postback fail rate >2% → Slack/email alert → investigare endpoint
- Voluum vs operator delta >5% → weekly report flag → manual reconciliation
- Zero conversii 24h pe campanie activa → immediate alert → check postback + operator

**Tool**: Voluum (reports, alerts), operator dashboards, Slack (alerting), M-008 (tracking baseline).
**Output**: Document `monitoring-playbook-{GEO}.md` cu monitoring schedule, alert config, escalation paths, reconciliation template.
**Checkpoint**: Toate 10 test conversii atribuite corect (10/10). Monitoring alerts configurate si testate (trigger manual fiecare alert). Reconciliation template completat cu date reale prima luna. Postback fail rate <2% pe primele 7 zile.

## 3. Cortex Logging

```json
{
  "text": "M-121 Server-Side Tracking & First-Party Data — Implementare tracking S2S, salted hashing PII (HMAC-SHA256), hybrid architecture si reconciliation continua pentru atribuire corecta post-cookie",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-121",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "tracking-attribution",
    "pipeline": "A",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "tracking", "server-side", "first-party-data", "privacy", "s2s", "salted-hash", "training"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (campaign setup / tracking audit)
- La fiecare campanie noua pe orice GEO
- La onboarding operator nou (postback setup obligatoriu)

### WHEN
- La fiecare campanie noua (S2S obligatoriu din Day 1)
- La schimbare operator sau oferta (new postback URLs)
- Lunar: audit tracking + reconciliation
- Trimestrial: HMAC key rotation + compliance review

### HOW (violation detection)
- Campanie lansata fara S2S postback configurat → **VIOLATION CRITICA**
- LP fara consent mechanism (banner lipsa) → **VIOLATION CRITICA**
- PII stocat cu plain SHA256 (fara salt/HMAC) → **VIOLATION CRITICA** (security risk)
- PII stocat in plaintext → **VIOLATION CRITICA** (data breach risk)
- Monthly reconciliation skip → **VIOLATION**
- Postback fail rate >5% neadresat >48h → **VIOLATION**
- HMAC key nerotit >90 zile → **WARNING**
- Cross-device attribution dezactivat → **WARNING**
- Browser fingerprinting folosit → **VIOLATION** (legal grey area)

### CONNECT
- **M-008** — Voluum setup (extins cu S2S la Pas 2)
- **M-116** — LP tracking localizat per GEO
- **M-118** — Compliance framework (consent, PII, GDPR equivalent)
- **M-119** — TikTok/native tracking integration (canal cu cel mai mare cookie loss)
- **M-120** — WhatsApp/SMS funnel tracking per mesaj (subIDs)

### VERIFY
1. S2S postback functional E2E — test 10 conversii, 10/10 atribuite corect in Voluum
2. Postback response code 200 pe toate testele — verificat in Voluum postback log
3. HMAC-SHA256 implementat (nu plain SHA256) — verificat: hash fara key produce rezultat diferit
4. Consent banner afisat pe toate LP-urile inainte de orice tracking — verificat manual pe 3 browsere
5. Monthly reconciliation delta <3% (Voluum vs operator factura) — verificat cu date prima luna
6. Cross-device attribution activ cu cel putin o metoda deterministic/first-party — verificat in Voluum settings
7. HMAC secret key stocat in secrets manager, nu in code/config — verificat locatie storage

### MODEL ROUTING
- **Claude Opus**: Tracking audit, gap analysis, compliance review, architecture design, reconciliation analysis
- **Claude Sonnet**: Postback URL generation, monitoring alert config, format checks (cost optimization)

## 5. Dependente

- **Tools**: Voluum (S2S postbacks, dual mode, cross-device), Google Tag Manager server-side (optional), CookieYes/Osano (consent), Slack (alerting), Claude Opus (audit)
- **Proceduri input**: M-008 (Voluum tracking setup — baseline), M-118 (compliance framework — consent requirements)
- **Proceduri framework**: M-120 (WhatsApp/SMS funnel tracking per mesaj), M-116 (LP tracking localizat)
- **Proceduri adjacent**: M-119 (TikTok/native — highest cookie loss channel), M-117 (competitive intelligence — tracking benchmarks)
- **Data sources**: Voluum documentation (S2S setup), operator API docs (postback integration), privacy legislation per GEO
- **Output consumed by**: M-120 (S2S postback per mesaj funnel), M-116 (tracking setup la Pas 5), M-008 (upgrade path)

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| Atribuire accuracy (Voluum vs operator) | >95% (delta <5%) | Saptamanal | Voluum reports vs operator dashboard |
| Postback fail rate | <2% | Daily | Voluum postback log |
| Cross-device match rate | >30% | Monthly | Voluum cross-device report |
| PII leak incidents | 0 | Continuous | Security audit log |
| Monthly reconciliation delta | ±3% | Monthly | Voluum vs operator factura |
| Consent banner coverage | 100% LP-uri | Trimestrial | Manual audit |
| HMAC key age | <90 zile | Trimestrial | Secrets manager log |
| Cookie loss recovery (pre vs post S2S) | >20% conversii recuperate | Post-migration (one-time) | Voluum before/after comparison |
| Test conversion success rate | 10/10 | Per operator onboarding | Voluum test log |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6 + Checklist)
- [x] Pasi numerotati secvential (1-6) cu Tool, Output si Checkpoint per pas
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] VERIFY cu 7 verificari concrete cu metoda specifica de verificare
- [x] KPIs masurabile cu target numeric, frecventa si sursa (9 metrici)
- [x] S2S tracking architecture flow diagram cu data passed at each step (text table + visual)
- [x] Salted hashing explicit (HMAC-SHA256, nu plain SHA256) cu normalization rules si key rotation
- [x] Dependente listate (tools + proceduri input/framework/adjacent + data sources + output consumed by)
- [x] Monitoring playbook cu thresholds, frecventa si action plan
- [x] Conexiuni cu proceduri existente validate (M-008, M-116, M-118, M-119, M-120)
- [x] Hybrid tracking architecture (S2S primar + cookie secundar + first-party tertiar) documentat
