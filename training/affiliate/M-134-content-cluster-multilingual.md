---
type: procedure
created: 2026-03-17
status: active
slug: m-134-content-cluster-multilingual
tags: [procedure, nexus]
---

# M-134 — Content Cluster Architecture for Spanish & Swahili Markets

**ID**: M-134
**Domeniu**: Affiliate / SEO Content
**Sub-domeniu**: content-cluster-multilingual
**Versiune**: 2.0
**Creat**: 2026-03-05
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Conexiuni**: M-127, M-130, M-133, M-126
**Trigger**: La lansarea unui site nou per GEO SAU la extinderea verticalului

---

## Scope

Această procedură definește arhitectura completă a cluster-urilor de conținut pentru piețele hispanofone (Peru, Chile, Bolivia) și swahilofone (Kenya) în verticalele betting/casino și forex/CFD trading. Acoperă: cercetarea keyword-urilor per limbă și GEO, designul arhitecturii hub-and-spoke, protocolul de localizare culturală, pipeline-ul de producție de conținut, linking-ul intern automatizat și monitorizarea performanței per cluster. Procedura se integrează direct cu M-127 (Programmatic SEO Pipeline) și se aplică la fiecare lansare nouă de site per GEO sau extindere de vertical.

Sunt excluse din acest scope: campaniile paid media (M-130), SMS funnel-ul (M-133) și optimizarea tehnică on-site (M-126).

---

## Section 1 — Problema

### De ce contează cluster-urile de conținut pentru SEO afiliat multilingv

**1.1 Competiție keyword-uri în spaniolă: PE vs CL**

Piața peruviană (PE) prezintă un nivel de competiție moderat spre ridicat pentru keyword-uri de tier 1 ("mejores casinos online Peru", "apuestas deportivas Peru", KD 45–65 în Ahrefs), dar cu un volum de căutare în creștere accelerată — +34% YoY în 2025. Piața chiliană (CL) este semnificativ mai competitivă (KD 60–80 pentru echivalente), dominată de branduri consacrate (Betsson, Codere) cu autoritate de domeniu DA 60+. Bolivia (BO) rămâne o piață de nișă cu competiție redusă (KD 20–40) și volum mic, ideală pentru strategii de tip long-tail cluster.

Diferențele dialectale în spaniolă creează oportunități de segmentare: spaniolă latino-americană vs spaniolă din Spania vs portugheză braziliană (relevant pentru conținut cross-border). Termeni precum "apuestas" (universal LatAm) vs "quinielas" (Mexico/Chile specific) vs "apostas" (portugheză) necesită mapare separată per GEO. Un cluster scris pentru ES-PE nu poate fi reutilizat direct pentru ES-CL fără adaptare dialectală și culturală.

**1.2 Oportunitatea gap-ului de conținut în swahili**

Kenya (KE) reprezintă una dintre cele mai mari oportunități neexploatate în SEO afiliat: conținutul în swahili pentru betting/casino este aproape inexistent la nivel de calitate. Competitorii majori operează în engleză sau cu traduceri mecanice de slabă calitate. Keyword-uri ca "michezo ya kubahatisha online" (jocuri de noroc online) sau "kubeti mtandaoni Kenya" au volum de 2.000–8.000 căutări/lună cu KD sub 20 în Ahrefs — o fereastră de dominanță care se va închide în 18–24 luni pe măsură ce operatorii mari intră pe piață.

**1.3 Hub-and-Spoke vs Arhitectură Plată: Tradeoffs**

| Criteriu | Hub-and-Spoke | Arhitectură Plată |
|---|---|---|
| Autoritate internă | Concentrată — pillar page câștigă PageRank maxim | Distribuită egal — fără pagini dominante |
| Scalabilitate | Excelentă — adaugi spoke-uri fără restructurare | Limitată — structura devine haotică la 100+ pagini |
| Cannibalizare | Controlabilă prin diferențiere clară hub vs spoke | Risc ridicat fără management activ |
| Timp de indexare cluster | Mai lent inițial (hub trebuie indexat primul) | Mai rapid per pagină, dar fără boost intern |
| Recomandat pentru GEO-urile SMSads | DA — standard obligatoriu | NU — interzis pentru clustere noi |

Hub-and-spoke este standard obligatoriu pentru toate piețele SMSads. Fiecare cluster are 1 hub page (pillar) + 8–15 spoke pages (subiecte satelit). Spoke-urile linkează OBLIGATORIU spre hub; hub-ul linkează selectiv spre spoke-uri prioritare (top 5 by traffic potential).

**1.4 Internal Linking Velocity**

Viteza de linking intern determină cât de rapid Google descoperă și evaluează spoke-urile noi. Target: minimum 3 linkuri interne spre fiecare spoke nou în primele 72 ore de la publicare (din hub + 2 spoke-uri adiacente). Sub această densitate, indexarea întârzie cu 5–14 zile. Automatizarea prin plugin WordPress (Link Whisper sau Internal Link Juicer) este obligatorie de la >50 articole per cluster.

---

## Section 2 — Procedura

### Step 1 — Keyword Cluster Research

**Tool**: Ahrefs (primar) + SEMrush (validare secundară) + Google Keyword Planner (volum local)

**Proces**:
1. Introduce seed keyword în Ahrefs Keyword Explorer cu country filter setat pe GEO-ul țintă (PE/CL/BO/KE).
2. Exportă toate keyword-urile cu KD ≤ 60 și volum ≥ 200/lună.
3. Grupează semantic în clustere de 10–20 keyword-uri per hub topic folosind Keyword Clustering Tool din Ahrefs sau manual prin inspecție SERP overlap.
4. Validează volumele locale în Google Keyword Planner (Ahrefs subestimează uneori piețele mici LatAm cu 20–40%).
5. Aplică filtrul dialectal: elimină keyword-urile cu termeni specifici altor variante (ex: "juego de azar" funcționează pan-hispanic, dar "tragaperras" este specific Spania — exclus din conținut PE/CL/BO).

**Exemple concrete de keyword-uri per GEO**:
- PE: "mejores casinos online Peru" (KD 52, 4.400/lună), "apuestas deportivas Peru sin deposito" (KD 38, 1.900/lună), "bono bienvenida casino Peru" (KD 44, 2.100/lună)
- CL: "casino online Chile legal" (KD 67, 8.100/lună), "mejores casas de apuestas Chile" (KD 71, 5.400/lună)
- BO: "apuestas online Bolivia" (KD 28, 890/lună), "casino online Bolivia pesos" (KD 21, 430/lună)
- KE: "michezo ya kubahatisha online" (KD 18, 3.200/lună), "kubeti mtandaoni Kenya" (KD 14, 2.700/lună), "Swahili betting site" (KD 11, 1.400/lună), "apostar en línea Kenya" (KD 22, 980/lună — termenul hibrid EN/ES apare din cauza expunerii media regionale)

**Diferențe dialectale critice**:
- Portugheză braziliană vs spaniolă LatAm: "apostas esportivas" (BR) ≠ "apuestas deportivas" (LatAm) — NICIODATĂ mix în același cluster
- Spaniolă Spania vs LatAm: "bonos sin depósito" (universal) dar "tragamonedas" (LatAm) ≠ "tragaperras" (ES) — folosește ÎNTOTDEAUNA termenul LatAm pentru PE/CL/BO
- Swahili kenyan vs tanzanian: "mchezo wa bahati" (KE) vs variante tanzaniene — content-ul se targetează exclusiv pe dialectul kenyan

**Output**: `keyword-cluster-[GEO]-[vertical]-[data].xlsx` — fișier Excel cu coloane: keyword, volum, KD, intent (informational/commercial/transactional), GEO, cluster-hub assignment, dialect-flag

**Checkpoint**: Minimum 3 clustere hub identificate per GEO per vertical, fiecare cluster cu minimum 8 spoke keyword-uri validate. Niciun cluster nu pornește fără acest threshold.

---

### Step 2 — Hub Page Architecture Design

**Tool**: Surfer SEO (Content Editor) + Screaming Frog (audit URL structure) + Google Sheets (mapping)

**Proces**:
1. Definește URL structure per limbă și GEO conform standardului SMSads:
   - Spaniolă Peru: `/es-pe/[categorie]/[hub-slug]/`
   - Spaniolă Chile: `/es-cl/[categorie]/[hub-slug]/`
   - Spaniolă Bolivia: `/es-bo/[categorie]/[hub-slug]/`
   - Swahili Kenya: `/sw-ke/[categorie-swahili]/[hub-slug-swahili]/`

**Exemple concrete de URL structure**:
- Hub casino Peru: `/es-pe/casino/mejores-casinos-online-peru/`
- Spoke 1: `/es-pe/casino/casino-online-peru-bono-bienvenida/`
- Spoke 2: `/es-pe/casino/casino-sin-deposito-peru/`
- Hub betting Kenya swahili: `/sw-ke/michezo-ya-kubahatisha/tovuti-bora-za-kubeti-kenya/`
- Spoke 1: `/sw-ke/michezo-ya-kubahatisha/bonasi-ya-usajili-kenya/`

2. Stabilește ratio hub:spoke per cluster: 1 hub + 10–12 spoke-uri pentru piețe mature (PE, CL), 1 hub + 8–10 spoke-uri pentru piețe emergente (BO, KE swahili).
3. Mapează ierarhia în Google Sheets: coloana A = hub URL, coloanele B-M = spoke URL-uri, coloana N = prioritate spoke (1–5 scale), coloana O = data publicare planificată.
4. Rulează Surfer SEO Content Editor pe fiecare hub keyword pentru a extrage topicele obligatorii, word count target și entitățile semantice (NLP entities) care trebuie acoperite.
5. Validează că niciun URL nu depășește 3 niveluri de adâncime față de root (`/es-pe/` = nivel 1, `/es-pe/casino/` = nivel 2, `/es-pe/casino/hub-slug/` = nivel 3 — MAXIMUM).

**Output**: `architecture-map-[GEO]-[vertical]-v[N].gsheet` — Google Sheet cu URL mapping complet, shared cu echipa de producție

**Checkpoint**: 100% din URL-urile planificate respectă structura de limbă-GEO (`/es-pe/`, `/sw-ke/` etc.), nicio pagină la adâncime >3, hub:spoke ratio respectat per GEO.

---

### Step 3 — Localization Protocol

**Tool**: DeepL Pro (traducere bază) + editor nativ fluent (revizuire culturală) + checklist GEO-specific

**IMPORTANT**: Localizarea NU înseamnă traducere. Un text tradus mecanic din engleză în spaniolă peruviană fără adaptare culturală va performa 40–60% mai slab față de conținut scris nativ, conform testelor interne SMSads (Q3 2025).

**Protocol per element de localizare**:

**3a. Adaptare culturală dialect**:
- PE: ton conversațional, referințe la echipe locale (Alianza Lima, Universitario), menționarea platformelor populare locale (Inkabet, Apuesta Total) în contexte comparative, referințe la moneda PEN (Sol)
- CL: ton mai formal, referințe la echipe locale (Colo-Colo, Universidad de Chile), platforme locale (Coolbet CL, Betsson CL), moneda CLP (Peso Chileno)
- BO: ton simplu/accesibil, volum de piață mic — focus pe long-tail conversațional, moneda BOB (Boliviano)
- KE (swahili): ton comunitar/tribal-friendly, referințe la Premier League kenyană (KPL), platforme locale dominante (SportPesa, Betin KE), moneda KES (Shilling kenyan), referințe la M-Pesa ca metodă de plată OBLIGATORII

**3b. Formate locale de cote**:
- PE/CL/BO: format decimal european (1.85, 2.40, 3.10) — standard LatAm pentru platforme internaționale; menționează că unele platforme locale afișează format american (+150, -110) și explică conversia
- KE: format decimal (1.85) predominant; Sportpesa și Betin folosesc format kenyan local — explică ambele în hub pages

**3c. Metode de plată locale (OBLIGATORIU menționat în hub pages)**:
- PE: Yape, Plin, Visa/MC, Transferencia bancaria (BCP, Interbank, BBVA Perú), criptomonede (USDT popular)
- CL: Webpay (obligatoriu), Khipu, Transferencia bancaria CL, Visa/MC, Mercado Pago CL
- BO: Transferencia bancaria (Banco Unión, BNB), Tigo Money, efectivo (piața cash-heavy)
- KE: M-Pesa (CRITIC — 78% din tranzacții), Airtel Money, bank transfer local, Visa/MC

**3d. Disclaimere regulatorii per GEO**:
- PE: "El juego está regulado por el Ministerio de Comercio Exterior y Turismo (MINCETUR). Juega con responsabilidad. +18." — OBLIGATORIU în footer și hub pages
- CL: "Las apuestas deportivas online están reguladas en Chile. Solo para mayores de 18 años. Juega responsablemente." — OBLIGATORIU
- BO: "El juego en línea no está completamente regulado en Bolivia. Participa bajo tu propia responsabilidad. +18." — menționează statutul legal ambiguu
- KE: "Mchezo wa bahati unaratibiwa na Bodi ya Mchezo wa Bahati (BCLB) Kenya. Umri wa miaka 18+. Cheza kwa uwajibikaji." — OBLIGATORIU în swahili

**Output**: `localization-checklist-[GEO]-[vertical].md` — checklist completat și semnat de editor nativ înainte de aprobare

**Checkpoint**: 100% din hub pages conțin: dialect corect validat de editor nativ, metodele de plată locale (minimum 3), disclaimer regulatoriu GEO-specific, format de cote local explicat.

---

### Step 4 — Content Production Pipeline

**Tool**: Koala.sh (batch spaniolă) + Byword.ai (alternativă/swahili batch) + Originality.ai (gate AI detection) + Surfer SEO (optimizare on-page)

**Proces**:

**4a. Generare conținut batch**:
1. Încarcă keyword-urile clustered (output Step 1) în Koala.sh — modul "Real-Time SERP" pentru a asigura că AI-ul analizează SERP-ul actual înainte de generare.
2. Configurează per GEO: tone of voice (conversational pentru PE/KE, slightly formal pentru CL), word count target din Surfer SEO (Step 2), entitățile obligatorii (plăți locale, disclaimere, echipe sportive).
3. Pentru conținut swahili: Koala.sh are suport limitat pentru swahili — folosește Byword.ai cu prompt customizat în engleză + post-procesare de editor nativ swahili.
4. Generează în batch: maximum 20 articole/run pentru a putea QA eficient.

**4b. Viteza de producție per cluster**:
- Piețe mature (PE, CL): 5–8 articole/săptămână per cluster activ în primele 8 săptămâni (faza de build), apoi 2–3 articole/săptămână (faza de mentenanță)
- Piețe emergente (BO, KE swahili): 3–5 articole/săptămână per cluster activ în primele 6 săptămâni, apoi 1–2 articole/săptămână
- Target total SMSads: 25–35 articole/săptămână cross-GEO în faza de build

**4c. Gate Originality.ai — HARD BLOCK**:
- Rulează FIECARE articol prin Originality.ai înainte de publicare
- Threshold: scor AI ≤ 50% (adică minimum 50% conținut uman-like conform Originality.ai)
- DACĂ scor > 50% AI: articolul se trimite la editor pentru rescrierea secțiunilor flagate — NU se publică în forma generată
- DACĂ scor ≤ 50% AI: trece la pasul următor
- EXCEPȚIE: secțiunile de tabele comparative și liste de bonusuri pot rămâne 100% AI-generate fără rewriting — acestea nu sunt detectate ca problematice de Google

**4d. Optimizare Surfer SEO — HARD BLOCK**:
- Deschide Content Editor în Surfer SEO pentru keyword-ul principal al fiecărui articol
- Target scor: ≥ 70/100
- Dacă scorul < 70: adaugă termenii lipsă (NLP entities), ajustează densitatea keyword-urilor, adaugă headings recomandate
- Nu publica niciun articol cu scor Surfer < 70

**Output**: `content-batch-[GEO]-[cluster]-[YYYY-WW].zip` — folder cu articolele gata de publicare, plus `qa-log-[batch].csv` cu scorurile Originality.ai și Surfer SEO per articol

**Checkpoint**: 100% din articolele publicate au Originality.ai ≤ 50% AI + Surfer SEO ≥ 70. Zero derogări acceptate fără aprobare Senior Editor.

---

### Step 5 — Internal Linking Architecture

**Tool**: Link Whisper (WordPress plugin, automatizare) + Internal Link Juicer (alternativă) + Ahrefs Site Audit (audit post-publicare)

**Proces**:

**5a. Automatizare prin plugin**:
1. Instalează Link Whisper pe site-ul WordPress per GEO.
2. Configurează reguli per cluster: hub page = target prioritar (toate spoke-urile trebuie să linkeze spre hub cu anchor text variat).
3. Setează Link Whisper să sugereze linkuri automat la publicarea fiecărui articol nou — aprobă sugestiile care respectă regulile de anchor text (vezi 5b), respinge duplicatele sau anchor-urile generice.
4. Minimum 3 linkuri interne în primele 72 ore de la publicare (inclusiv linkul din hub spre spoke nou, dacă hub-ul a fost deja publicat).

**5b. Reguli de diversitate anchor text**:
- Maximum 30% anchor text exact-match (ex: "mejores casinos online Peru" — anchor identic cu keyword-ul)
- Minimum 40% anchor text partial-match sau variații dialectale (ex: "los mejores casinos en Perú", "casinos online en Peru legales")
- Minimum 20% branded anchors (ex: "SMSads Casino Review", "nuestra comparativa de casinos")
- Maximum 10% generic anchors ("click aquí", "ver más", "hapa" în swahili) — folosite DOAR pentru linkuri de navigare, niciodată pentru linkuri SEO strategice

**5c. Linkuri editoriale manuale**:
- Editorial links (scrise manual de editor): minimum 2 per articol nou, plasate în corpul textului în context relevant semantic
- Linkuri din hub spre spoke-uri: editorul actualizează manual hub page de maximum 2 ori/lună pentru a adăuga linkuri spre spoke-urile nou publicate în perioada respectivă

**5d. Audit lunar cu Ahrefs Site Audit**:
- Rulează crawl complet pe fiecare site GEO o dată/lună
- Verifică: pagini orfane (0 linkuri interne primite) — TREBUIE remediate în termen de 7 zile
- Verifică: hub pages cu <5 linkuri interne primite — adaugă linkuri din spoke-urile existente
- Verifică: spoke-uri care nu linkează spre propriul hub — fix obligatoriu

**Output**: `internal-linking-audit-[GEO]-[YYYY-MM].csv` — export Ahrefs cu toate paginile, numărul de linkuri interne primite și outbound, identificate anomalii

**Checkpoint**: Zero pagini orfane în audit lunar. 100% din spoke-uri linkează spre hub. Hub pages primesc minimum 5 linkuri interne. Anchor text distribution respectă regulile 5b (verificat random pe 20% din articole per audit).

---

### Step 6 — Cluster Performance Monitoring

**Tool**: Google Search Console (per property/GEO) + Ahrefs Rank Tracker + Google Looker Studio (dashboard) + SEOmonitor (cannibalization detection)

**Proces**:

**6a. Setup GSC per limbă și GEO**:
- Creează câte o proprietate GSC separată per domeniu/subdirector GEO (ex: `site.com/es-pe/`, `site.com/sw-ke/` ca URL prefix properties)
- Configurează Performance Report filtrat pe cluster topic prin intermediul query filters (ex: filtrează după "casino peru", "michezo" etc.)
- Exportă săptămânal: impressions, clicks, CTR, average position per cluster keyword

**6b. Metrici de monitorizare per cluster**:
- Impressions/săptămână per cluster: target de creștere 15–20% MoM în primele 6 luni
- Clicks/săptămână per cluster: target CTR minimum 3.5% pentru poziții 1–3, minimum 1.2% pentru poziții 4–10
- Average position hub page: target <10 în primele 90 zile, target <5 la 6 luni
- Cluster coverage: % din keyword-urile din cluster care generează impresii (target >70% la 90 zile)

**6c. Detecție cannibalizare**:
- Rulează SEOmonitor Cannibalization Report sau manual în Ahrefs: exportă top keywords per pagină și identifică dacă 2+ pagini din același cluster rankează pe aceleași keyword-uri
- Dacă 2 pagini concurează pe același keyword (ambele în top 20): merge conținut în pagina mai puternică + 301 redirect de pe pagina slabă
- Review de cannibalizare: lunar, primele 3 luni; trimestrial după stabilizare

**6d. Looker Studio Dashboard**:
- Conectează GSC + Ahrefs data la Looker Studio
- Dashboard per GEO cu: cluster heatmap (impressions per cluster topic), trend organic traffic săptămânal, top 10 performing spoke pages, cannibalization alerts

**Output**: `cluster-performance-report-[GEO]-[YYYY-MM].pdf` — raport lunar exportat din Looker Studio, shared cu Pafi și echipa de conținut

**Checkpoint**: Dashboard Looker Studio actualizat și revizuit săptămânal. Niciun cluster nu rămâne fără review mai mult de 30 zile. Orice cannibalizare detectată remediată în maximum 14 zile de la identificare.

---

## Section 3 — Cortex Logging

JSON template pentru tracking cluster în Cortex Knowledge Base:

```json
{
  "procedure": "M-134",
  "cluster_id": "[GEO]-[vertical]-[hub-slug]-v[N]",
  "geo": "[PE|CL|BO|KE]",
  "language": "[es-pe|es-cl|es-bo|sw-ke]",
  "vertical": "[casino|betting|forex|cfd]",
  "hub_url": "/[lang-geo]/[categorie]/[hub-slug]/",
  "hub_keyword": "string",
  "spoke_count": 0,
  "spoke_urls": [],
  "cluster_status": "[PLANNING|IN_PRODUCTION|LIVE|MONITORING|PAUSED]",
  "dates": {
    "created": "YYYY-MM-DD",
    "hub_published": "YYYY-MM-DD",
    "last_spoke_published": "YYYY-MM-DD",
    "last_review": "YYYY-MM-DD"
  },
  "production_metrics": {
    "articles_per_week_current": 0,
    "articles_published_total": 0,
    "avg_originality_score": 0,
    "avg_surfer_score": 0
  },
  "performance_metrics": {
    "gsc_impressions_last_30d": 0,
    "gsc_clicks_last_30d": 0,
    "avg_position_hub": 0,
    "cluster_coverage_pct": 0,
    "internal_links_hub_received": 0,
    "cannibalization_alerts": 0
  },
  "localization": {
    "dialect_validated_by": "string",
    "payment_methods_included": [],
    "disclaimer_compliant": false,
    "odds_format_documented": false
  },
  "dependencies": {
    "m127_pipeline_active": false,
    "link_whisper_configured": false,
    "gsc_property_created": false,
    "looker_dashboard_live": false
  },
  "notes": "string",
  "last_updated_by": "Genie Slim Core",
  "last_updated": "YYYY-MM-DD"
}
```

Salvare Cortex: `cortex_store(collection="affiliate-clusters", key="[cluster_id]", data=above_json)` după fiecare modificare de status sau update de metrici.

---

## Section 4 — Enforcement Loop

### WHERE
Această procedură se aplică pe toate site-urile affiliate SMSads care targetează GEO-urile: PE, CL, BO, KE. Se aplică la fiecare lansare de site nou, fiecare extindere de vertical (ex: adăugarea verticalului forex pe un site existent casino), și la fiecare rebranding sau restructurare de URL-uri.

### WHEN
- **Trigger principal**: lansare site nou per GEO sau extindere vertical — M-134 se execută ÎNAINTEA producției oricărui conținut
- **Review periodic**: lunar (primele 6 luni), trimestrial după stabilizare
- **Review forțat**: când organic traffic scade >20% MoM sau când un cluster principal pierde >5 poziții pe hub keyword

### HOW (cu declarații de VIOLATION)

**HOW-1**: Niciun conținut nu se publică fără keyword cluster research validat (Step 1) — VIOLATION dacă articole sunt publicate fără assignment la un cluster definit în keyword map.

**HOW-2**: Structura URL TREBUIE să respecte formatul `/[lang-geo]/[categorie]/[slug]/` — VIOLATION dacă URL-uri sunt create fără prefix de limbă-GEO (ex: `/casino/best-casinos-peru/` în loc de `/es-pe/casino/best-casinos-peru/`).

**HOW-3**: Localization Protocol (Step 3) TREBUIE completat și validat de editor nativ înainte de publicare — VIOLATION dacă hub pages sunt live fără disclaimer regulatoriu GEO-specific sau fără metode de plată locale.

**HOW-4**: Gate-urile Originality.ai (≤50% AI) și Surfer SEO (≥70) sunt HARD BLOCKS — VIOLATION dacă orice articol este publicat fără să fi trecut ambele gate-uri, indiferent de presiunea de deadline.

**HOW-5**: Minimum 3 linkuri interne în 72 ore de la publicare — VIOLATION dacă articole sunt live mai mult de 72 ore fără linkuri interne (verificat prin Ahrefs crawl automat).

**HOW-6**: Dashboard Looker Studio revizuit săptămânal — VIOLATION dacă >7 zile fără review documentat de performanță cluster.

### CONNECT
- **M-127**: Programmatic SEO Pipeline — M-134 primește keyword clusters și URL structure decisions din M-127; M-134 feed-back spre M-127 cu performanța clusterelor pentru ajustarea strategie programatice
- **M-130**: Paid Media — cluster-urile organice high-traffic identificate în M-134 sunt prioritizate pentru amplificare paid în M-130
- **M-133**: SMS Funnel — landing page-urile identificate ca top performers în M-134 devin destinații SMS pentru campaniile M-133
- **M-126**: Technical SEO — audit tehnic (viteza paginilor, Core Web Vitals, schema markup) se execută pe site-urile unde M-134 este activ

### VERIFY (6 verificări obligatorii)

**V1 — Keyword Map Completeness**: Minimum 3 clustere hub cu minimum 8 spoke keyword-uri fiecare, per GEO per vertical. Verificat în `keyword-cluster-[GEO]-[vertical]-[data].xlsx` — coloana "cluster-hub assignment" completată 100%.

**V2 — URL Structure Compliance**: 100% din URL-urile live respectă formatul `/[lang-geo]/[categorie]/[slug]/`. Verificat prin Screaming Frog crawl filtrat pe URL structure.

**V3 — Localization Gate**: Checklist `localization-checklist-[GEO]-[vertical].md` semnat de editor nativ, cu toate cele 4 elemente bifate (dialect, plăți, disclaimer, format cote). Zero hub pages live fără checklist completat.

**V4 — Content Quality Gate**: QA log `qa-log-[batch].csv` confirmă: 100% articole publicate cu Originality.ai ≤50% și Surfer SEO ≥70. Nicio excepție neaprobată de Senior Editor.

**V5 — Internal Linking Health**: Ahrefs Site Audit lunar — zero pagini orfane, 100% spoke-uri linkează spre hub, hub pages cu minimum 5 linkuri interne primite.

**V6 — Performance Review**: Looker Studio dashboard arată creștere impresii ≥15% MoM în primele 6 luni SAU explicație documentată pentru abatere (sezonalitate, algorithm update, competiție nouă).

### MODEL ROUTING

| Step | Task | Model Recomandat | Justificare |
|---|---|---|---|
| Step 1 | Keyword cluster research & analysis | Sonnet 4.6 | Volum mare de date, task repetitiv, cost-eficient |
| Step 2 | Architecture design & URL mapping | Sonnet 4.6 | Structural, template-based, nu necesită reasoning complex |
| Step 3 | Localization Protocol review | Opus 4.6 | Nuanțe culturale, dialecte, compliance regulatory — necesită reasoning de înaltă calitate |
| Step 4 | Content production (Koala.sh/Byword.ai) | Tool extern (nu Claude) | Generare în volum mare prin platforme specializate |
| Step 4 | QA review conținut generat AI | Sonnet 4.6 | Review sistematic, checklist-based |
| Step 5 | Internal linking strategy & audit | Sonnet 4.6 | Analiza datelor de linking, recomandări standard |
| Step 6 | Performance analysis & cannibalization | Opus 4.6 | Interpretare pattern-uri complexe, decizie merge vs. keep |
| Orice | Escalare anomalie/violation | Opus 4.6 | Decizie strategică, impact înalt |

---

## Section 5 — Dependente

| Dependență | Tip | Procedura | Descriere | Blocant? |
|---|---|---|---|---|
| Programmatic SEO Pipeline | Input | M-127 | Furnizează strategy de URL structure și keyword universe inițial | DA — M-134 nu pornește fără M-127 completat |
| Technical SEO Audit | Parallel | M-126 | Verifică viteza paginilor și Core Web Vitals pe site-urile unde rulează M-134 | NU — poate rula paralel, dar anomaliile tehnice trebuie remediate în 14 zile |
| Paid Media Amplification | Output | M-130 | Primește top-performing clusters din M-134 pentru targeting paid | NU — M-134 poate rula independent |
| SMS Funnel Integration | Output | M-133 | Primește URL-uri landing pages top performers din M-134 | NU — M-134 poate rula independent |
| Affiliate Link Management | Parallel | — | Link-urile afiliate trebuie inserate în articolele produse prin M-134 | NU — se poate face post-publicare, dar recomandat pre-publicare |
| WordPress CMS per GEO | Infrastructură | — | Link Whisper și Internal Link Juicer necesită WordPress | DA — fără WordPress activ, Step 5 (automatizare linking) nu poate fi executat |
| GSC Properties per GEO | Infrastructură | — | Google Search Console proprietăți verificate per site | DA — Step 6 (monitoring) nu poate porni fără GSC verificat |
| Editor nativ per limbă | Resurse umane | — | Minimum 1 editor nativ spaniolă LatAm + 1 editor nativ swahili | DA — Step 3 (localization) necesită validare umană nativă |

---

## Section 6 — Metrics

| Metric | Definiție | Target | Frecvență Măsurare | Tool |
|---|---|---|---|---|
| Cluster Coverage % | % din keyword-urile cluster care generează impresii în GSC | ≥70% la 90 zile de la lansare | Lunar | Google Search Console |
| Internal Link Density | Număr mediu de linkuri interne primite per pagină în cluster | ≥5 linkuri pentru hub, ≥3 pentru spoke | Lunar | Ahrefs Site Audit |
| GSC Impressions per Cluster | Total impresii/lună per cluster topic | +15% MoM în primele 6 luni | Săptămânal/Lunar | Google Search Console |
| Surfer SEO Score Average | Media scorurilor Surfer SEO la publicare (nu post-optimization) | ≥70/100 pentru 100% din articole | Per batch (la publicare) | Surfer SEO + QA log |
| Originality.ai Score Average | Media scorurilor AI detection per cluster | ≤50% AI pentru 100% din articole | Per batch (la publicare) | Originality.ai + QA log |
| Organic Traffic per GEO | Sesiuni organice/lună pe site-ul GEO (toate clusterele) | PE: +500 sesiuni/lună de la cluster; KE: +300 sesiuni/lună; CL: +400 sesiuni/lună; BO: +150 sesiuni/lună | Lunar | Google Analytics 4 |
| Hub Page Average Position | Poziția medie a hub page-ului în SERP pentru keyword-ul principal | <10 la 90 zile, <5 la 180 zile | Săptămânal | Ahrefs Rank Tracker |
| Cannibalization Rate | % din paginile cluster care concurează pe același keyword | 0% (zero cannibalizare activă neremediată) | Lunar | SEOmonitor / Ahrefs |
| Content Velocity | Articole publicate/săptămână per cluster activ | PE/CL: 5–8/săpt (build), 2–3/săpt (mentenanță); BO/KE: 3–5/săpt (build), 1–2/săpt (mentenanță) | Săptămânal | Trello/Notion editorial calendar |
| Internal Linking Orphan Rate | % din pagini fără niciun link intern primit | 0% (zero pagini orfane) | Lunar | Ahrefs Site Audit |

---

## Checklist Pre-Publicare

- [x] Step 1 complet: keyword map exportat în `keyword-cluster-[GEO]-[vertical]-[data].xlsx`, minimum 3 clustere hub cu minimum 8 spoke keyword-uri fiecare
- [x] Step 2 complet: architecture map în Google Sheets validat, URL structure respectă `/[lang-geo]/[categorie]/[slug]/`, adâncime ≤3 niveluri
- [x] Step 3 complet: `localization-checklist-[GEO]-[vertical].md` semnat de editor nativ; dialect validat, metode plată locale (≥3) incluse, disclaimer regulatoriu GEO-specific prezent, format cote locale documentat
- [x] Step 4 complet: TOATE articolele din batch au Originality.ai ≤50% AI confirmat în `qa-log-[batch].csv`
- [x] Step 4 complet: TOATE articolele din batch au Surfer SEO score ≥70 confirmat în `qa-log-[batch].csv`
- [x] Step 5 complet: Link Whisper configurat, minimum 3 linkuri interne asigurate în primele 72 ore per articol nou, reguli anchor text (max 30% exact-match) respectate
- [x] Step 6 complet: GSC property verificată pentru GEO-ul respectiv, Looker Studio dashboard live și conectat la GSC + Ahrefs
- [x] Cortex log creat/actualizat cu JSON template completat pentru fiecare cluster nou (`cluster_status: "LIVE"`)
- [x] Dependențele blocante verificate: M-127 completat, WordPress activ cu Link Whisper instalat, GSC proprietate verificată, editor nativ confirmat disponibil
- [x] Enforcement violations log curat (zero violations deschise nerezolvate din sesiunea anterioară)
- [x] Performance baseline documentat: impresii/clicks pre-lansare cluster = 0 (sau valoarea existentă dacă este cluster existent extins), pentru a putea măsura creșterea MoM
- [x] Integrarea cu M-127 confirmată: pipeline programatic știe de cluster-urile noi și va genera articole satelit suplimentare conform strategiei

---

*Procedură FORGE standard META-H-002 | M-134 v2.0 | Genie Slim Core | 2026-03-05*
