---
type: procedure
created: 2026-03-20
status: active
slug: m-131-multi-touch-attribution
tags: [procedure, nexus]
---

# M-131 — Multi-Touch Cross-Channel Attribution Modeling

**Domeniu**: affiliate | **Sub-domeniu**: multi-touch-attribution
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Conectat la**: M-121 (S2S Tracking), M-119 (TikTok/Native Ads), M-120 (WhatsApp/SMS Funnel), M-117 (Competitive Intelligence)

---

## §1 — PROBLEMA

Operăm simultan pe canale multiple (TikTok + native ads + WhatsApp + SEO organic) pe aceleași GEO-uri cu aceleași oferte. Sistemul actual M-121 implementează atribuire single last-touch (ultimul click primar).

**Distorsiunile create de last-touch attribution**:
- TikTok (canal awareness, upper-funnel) apare sistematic ca non-converter → tăieri de buget premature
- Native retargeting (ultimul click înainte de conversie) primește 100% din credit → supra-investiție în lower funnel
- WhatsApp funnel: conversii influențate de reclame anterioare par "organice"
- SEO organic: jucători care au văzut un ad înainte apar ca achiziție organică gratuită

**Impactul financiar estimat**:
- La $5,000/lună cheltuieli publicitare cross-canal, misatribuirea costă ~20–35% eficiență pierdută
- Exemplu concret: TikTok inițiază 40% din journey-urile de conversie, dar primește 0 credit last-touch → tăiem cel mai bun canal de achiziție

**Provocarea tehnică**:
Legarea unui utilizator între TikTok (cookie-based), native ads (cookie), WhatsApp (număr telefon), și SEO (organic session) necesită un identificator persistent cross-canal.

---

## §2 — PROCEDURA

### Pas 1: SCHEMA UNIFIED CLICK ID

**Obiectiv**: Un identificator persistent care urmărește același utilizator de la primul touchpoint la conversie, indiferent de canal.

**Schema VID (Visitor ID)**:
```
Format: vid_{SHA256(email_or_phone)[:16]}_{unix_timestamp_first_touch}
Exemplu: vid_a3f9c2b1d8e47f12_1741200000
```

**Implementare per canal**:

| Canal | Sursă Identificator | Metoda de Captare | Stocare |
|-------|--------------------|--------------------|---------|
| TikTok Ads | TikTok Click ID (ttclid) | URL parameter → Voluum cv1 | Voluum custom variable 1 |
| Native Ads | Click ID propriu (ex: subid) | URL parameter → Voluum cv1 | Voluum custom variable 1 |
| WhatsApp Funnel | Număr telefon (opt-in) | SHA256(phone) la opt-in | Voluum custom variable 2 |
| SEO Organic | Session fingerprint | IP + UA hash (probabilistic) | First-party cookie 90 zile |

**Regula de prioritizare**:
1. Dacă există SHA256(email/phone) din WhatsApp opt-in → aceasta este sursa de adevăr (deterministică)
2. Altfel: VID bazat pe click_id din Voluum
3. Fallback: match probabilistic IP+UA (confidence <70% = marchează ca "unresolved")

**Configurare Voluum**:
- Custom Variable 1: `vid` (unified visitor ID)
- Custom Variable 2: `phone_hash` (SHA256 telefon de la WhatsApp)
- Custom Variable 3: `first_channel` (canalul primului touchpoint)
- Custom Variable 4: `path_sequence` (șirul de touchpoint-uri, ex: "tiktok>native>convert")

**Output**: `attribution-tracking-schema-v1.md` — documentație schema + configurație Voluum
**Checkpoint**: VID implementat pe toate canalele active; zero campanii fără cv1 populat la conversie

---

### Pas 2: LOGGING TOUCHPOINT-URI

**Obiectiv**: Înregistrarea fiecărui touchpoint în journey-ul utilizatorului, nu doar ultimul click.

**Date capturate per touchpoint**:
```
{
  "vid": "vid_a3f9c2b1d8e47f12_1741200000",
  "timestamp": "2026-03-05T14:32:00Z",
  "channel": "tiktok|native|whatsapp|seo",
  "touchpoint_type": "impression|click|page_view|message_open|registration|deposit",
  "creative_id": "TK-001-LATAM-Soccer",
  "campaign_id": "PE-MarathonBet-March",
  "geo": "PE",
  "session_id": "sess_xyz123",
  "is_conversion": false
}
```

**Implementare per canal**:

**TikTok**:
- Pixel TikTok: fire event la fiecare landing page view cu VID în custom_data
- Adaugă `ttclid` ca Voluum click parameter → mapat la cv1

**Native Ads (Taboola/Outbrain)**:
- Tracking pixel pe landing page cu subid = VID
- Postback la conversie cu VID

**WhatsApp**:
- La opt-in (număr telefon): generează phone_hash + VID, stochează în sheet/CRM
- Fiecare click pe link din WhatsApp: include VID ca URL parameter

**SEO Organic**:
- First-party cookie: dacă utilizatorul a mai vizitat site-ul (orice canal), VID-ul din cookie se propagă
- Landing page: verifică cookie → dacă există VID anterior, marchează touchpoint ca "seo_return"

**Stocare touchpoint-uri**:
- Opțiunea 1 (Minimum Viable): Google Sheets cu logging manual + formulas pentru path reconstruction
- Opțiunea 2 (Scale >500 conversii/lună): BigQuery — export Voluum → tabela `touchpoints`, query path analysis

**Output**: `touchpoint-log-{GEO}-{YYYY-MM}.csv` — log toate touchpoint-urile per lună per GEO
**Checkpoint**: logging activ pe minimum 3 canale; acoperire >80% din conversii cu cel puțin 2 touchpoint-uri înregistrate

---

### Pas 3: SELECȚIE MODEL ATRIBUIRE

**Implementăm 3 modele simultan** pentru comparație:

**Model 1: Last-Touch (Baseline Curent)**
```
Credit conversie = 100% ultimul touchpoint înainte de depozit
```
Avantaj: simplu, disponibil deja în M-121. Dezavantaj: sistematic greșit pentru canale de awareness.

**Model 2: Linear Attribution**
```
Credit per touchpoint = 100% / număr total touchpoint-uri din journey
Exemplu: TikTok → Native → Native → Convert → fiecare primește 25%
```
Avantaj: echitabil, nu favorizează nici upper, nici lower funnel. Dezavantaj: nu reflectă că touchpoint-urile recente sunt mai influente.

**Model 3: Time-Decay Attribution (Recomandat pentru Gambling)**
```
Credit(t) = w(t) / Σw(t') × 100%
w(t) = e^(λ × (t - t_conversion))
λ = ln(2) / half_life
Half-life recomandat gambling: 7 zile
```
Explicație: touchpoint-urile de acum 2 săptămâni primesc jumătate din creditul unui touchpoint de ieri. Motivat de comportamentul jucătorilor de gambling: intenția de conversie se construiește în zile, nu săptămâni.

**Calculul practic (Google Sheets)**:
```
Dacă journey: TikTok (ziua 0) → Native (ziua 5) → Native (ziua 7) → Convert (ziua 7)
w_TikTok = e^(-0.099 × 7) = 0.50
w_Native1 = e^(-0.099 × 2) = 0.82
w_Native2 = e^(-0.099 × 0) = 1.00
Total w = 2.32
Credit TikTok = 21.6%, Credit Native1 = 35.3%, Credit Native2 = 43.1%
```

**Decizie model principal**: Time-Decay cu half-life = 7 zile pentru decizii de buget. Last-touch menținut ca referință comparativă.

**Output**: `attribution-model-comparison-{YYYY-MM}.md` — credite per canal per model + deviații față de last-touch
**Checkpoint**: toate 3 modelele calculate lunar; time-decay folosit ca bază pentru decizii de buget

---

### Pas 4: ANALIZA CĂILOR DE CONVERSIE

**Obiectiv**: Identificarea celor mai frecvente secvențe de touchpoint-uri care duc la conversie.

**Metodologie**:
1. Extrage toate journey-urile cu conversie (FTD) din luna curentă
2. Normalizează: elimină duplicatele consecutive (Native → Native → Native = Native×3)
3. Grupează prin path distinct și contorizează frecvența
4. Calculează revenue mediu per path

**Template Analiză Căi**:

| Cale de Conversie | Frecvență | % din Total | Revenue Mediu | Last-Touch Credit | Linear Credit | Time-Decay Credit |
|-------------------|-----------|-------------|---------------|-------------------|---------------|-------------------|
| TikTok → Native → Convert | 45 | 28% | $180 | Native 100% | TK 50% / Nat 50% | TK 21% / Nat 79% |
| SEO → WhatsApp → Convert | 38 | 24% | $210 | WhatsApp 100% | SEO 50% / WA 50% | SEO 29% / WA 71% |
| Native → Native → Convert | 31 | 19% | $145 | Native 100% | Nat 50% / Nat 50% | Nat 40% / Nat 60% |
| WhatsApp → Convert (direct) | 25 | 16% | $190 | WA 100% | WA 100% | WA 100% |
| SEO → Native → WhatsApp → Convert | 21 | 13% | $240 | WA 100% | 33/33/33% | SEO 12% / Nat 28% / WA 60% |

**Insight-uri cheie de extras**:
- **Canal inițiator (Upper Funnel)**: care canal apare mai des ca primul touchpoint
- **Canal de închidere (Lower Funnel)**: care canal apare mai des imediat înainte de conversie
- **Valoarea asistenței**: canale care apar frecvent în mijlocul journey-ului fără a fi primul sau ultimul
- **Journey length**: câte touchpoint-uri în medie înainte de conversie per GEO

**Regula critică**: Nu tăia niciodată un canal care inițiază >30% din căile de conversie, chiar dacă pare cu conversie redusă la last-touch.

**Output**: `path-analysis-{GEO}-{YYYY-MM}.md` — top 10 căi de conversie + insight-uri upper/lower funnel
**Checkpoint**: analiză căi generată lunar per GEO activ; canale cu assist >30% marchează explicit în raport

---

### Pas 5: FRAMEWORK REALOCARE BUGET

**Obiectiv**: Traducerea modelului time-decay în decizii concrete de alocare buget.

**Workflow lunar** (executat după Pas 3 și Pas 4):

**Pas 5.1: Calculează contribuția reală per canal (time-decay)**
```
Contribuție Canal = Σ(Credit Time-Decay × Revenue Conversie)
```
Per canal, per GEO, per lună.

**Pas 5.2: Compară cu atribuirea last-touch curentă**
```
Delta Attribution = (Time-Decay Revenue - Last-Touch Revenue) / Last-Touch Revenue × 100%
```

**Pas 5.3: Identifică discrepanțe semnificative**
Dacă `|Delta| > 20%` pentru orice canal → recomandare de realocare.

**Matricea de decizie**:

| Canal | Spend Curent | Last-Touch Rev | Time-Decay Rev | Delta | Decizie |
|-------|--------------|----------------|----------------|-------|---------|
| TikTok | $1,500 | $800 (sub-atribuit) | $2,100 | +163% | **CREȘTE buget +30%** |
| Native | $2,000 | $3,500 (supra-atribuit) | $2,800 | -20% | Menține, monitorizează |
| WhatsApp | $500 | $1,200 | $1,000 | -17% | Menține |
| SEO | $1,000 | $0 (organic) | $400 (assist) | N/A | Continuă investiția |

**Reguli de realocare**:
1. Nu tăia niciun canal care asistă >30% din conversii (indiferent de last-touch revenue)
2. Crești bugetul canalului inițiator dacă time-decay arată contribuție >2× spend actual
3. Realocările maxime: ±30% per canal per lună (evită oscilații mari)
4. Re-testează după 45 zile înainte de o nouă ajustare

**Output**: `budget-reallocation-{GEO}-{YYYY-MM}.md` — tabel decizie + rațional + acțiuni concrete
**Checkpoint**: realocare evaluată lunar; nicio tăiere de canal fără analiza path assist; modificări documentate cu rațional

---

### Pas 6: CERINȚE COOPERARE OPERATOR

**Obiectiv**: Match deterministic cross-device și cross-session pentru creșterea acurateței attribution.

**Ce solicităm de la operatori** (verifica înainte de setup):

| Cerință | Operator Suportă? | Alternativă Dacă Nu |
|---------|-------------------|---------------------|
| User ID unic în postback S2S | Obligatoriu (M-121 Pas 3) | Nu există — condiție minimă |
| Cross-device user_id match | Ideal | Match probabilistic IP+UA |
| Transaction history per click_id | Recomandat | Export manual lunar |
| Registration timestamp în postback | Obligatoriu | Nu există — condiție minimă |

**Verificare per operator** (documentează în `operator-attribution-compatibility.md`):
1. Intră în setările postback S2S ale operatorului
2. Confirmă că trimite: `{click_id}`, `{user_id}`, `{transaction_id}`, `{amount}`, `{timestamp}`
3. Testează cu Voluum: trimite test conversion, verifică toate câmpurile populate
4. Notează: care operatori NU suportă cross-device user_id → marchează attribution ca "probabilistic" pentru aceștia

**Notă complexitate**: matching-ul cross-device fără cooperarea operatorului este estimat la 60–75% acuratețe pentru gambling mobile. Investiția în infrastructura deterministic (BigQuery + proper user_id matching) este justificată la scale >1,000 FTD/lună.

**Output**: `operator-attribution-compatibility.md` — tabel compatibilitate per operator + gaps identificate
**Checkpoint**: compatibilitate verificată pentru toți operatorii activi; limitările documate explicit înainte de luarea deciziilor de buget

---

## §3 — COMPLEXITATE & IMPLEMENTARE GRADUALĂ

**Versiunea Minimum Viable (până la 500 conversii/lună)**:
- Google Sheets cu logging manual al touchpoint-urilor (Pas 2 simplificat)
- Formule pentru calculul time-decay în spreadsheet
- Path analysis manual (top 5 căi, nu exhaustiv)
- Timp necesar: ~4 ore/lună

**Versiunea Scale (>500 conversii/lună)**:
- BigQuery: export automat Voluum → tabela `touchpoints` (schedule zilnic)
- SQL queries pentru path analysis exhaustiv
- Dashboard Looker Studio cu credit per canal în timp real
- Timp setup: ~40 ore (configurare inițială)
- Cost: ~$20-50/lună BigQuery

---

## §4 — CORTEX KNOWLEDGE BASE

```json
{
  "procedure_id": "M-131",
  "title": "Multi-Touch Cross-Channel Attribution Modeling",
  "domain": "affiliate",
  "sub_domain": "multi-touch-attribution",
  "version": "1.0",
  "status": "active",
  "connects_to": ["M-121", "M-119", "M-120", "M-117"],
  "complexity": "HIGH",
  "min_viable_version": "Google Sheets pana la 500 conversii/luna",
  "scale_version": "BigQuery + Looker Studio la >500 conversii/luna",
  "output_files": [
    "attribution-tracking-schema-v1.md",
    "touchpoint-log-{GEO}-{YYYY-MM}.csv",
    "attribution-model-comparison-{YYYY-MM}.md",
    "path-analysis-{GEO}-{YYYY-MM}.md",
    "budget-reallocation-{GEO}-{YYYY-MM}.md",
    "operator-attribution-compatibility.md"
  ],
  "key_parameters": {
    "attribution_model_primary": "time-decay (half-life 7 zile)",
    "attribution_model_baseline": "last-touch (M-121 curent)",
    "reallocation_trigger": "Delta >20% intre modele pentru orice canal",
    "protect_assist_threshold": "canale cu assist >30% conversii nu se taie",
    "reallocation_max_change": "±30% per canal per luna"
  },
  "tags": ["attribution", "multi-touch", "tiktok", "native", "whatsapp", "seo", "budget-allocation", "gambling"]
}
```

---

## §5 — CONEXIUNI INTER-PROCEDURI

| Procedură | Tip Conexiune | Detaliu |
|-----------|---------------|---------|
| **M-121** | Extinde | M-131 adaugă multi-touch pe lângă last-touch din M-121; click_id din M-121 este baza pentru VID |
| **M-119** | Feedback buget | Analiza time-decay din M-131 alimentează deciziile de spend TikTok/native din M-119 |
| **M-120** | Logging WhatsApp | Touchpoint-urile WhatsApp din M-120 se înregistrează în schema M-131 |
| **M-117** | Context competitive | Competitive intelligence M-117 poate informa benchmark-uri pentru journey length tipic pe nișă |

---

## §6 — METRICI DE SUCCES

| Metric | Target | Metodă Măsurare |
|--------|--------|-----------------|
| Acoperire VID la conversii | >80% din FTD-uri cu VID valid | Raport Voluum cv1 populated |
| Journey-uri cu >1 touchpoint | >50% din conversii | Path analysis lunar |
| Delta buget realocat corect | Verificat la 45 zile post-realocare | ROI per canal pre vs post |
| Canale over/under-atribuite identificate | Toate canalele active analizate lunar | Attribution model comparison |

---

## §7 — VIOLĂRI HOW (Interzis)

- **VIOLAȚIE**: Tăierea bugetului TikTok/SEO bazat exclusiv pe last-touch revenue fără analiza path assist → risc pierdere canal inițiator principal
- **VIOLAȚIE**: Decizii de buget >30% shift fără minimum 30 zile de date multi-touch → modificare prematură
- **VIOLAȚIE**: Ignorarea incompatibilității operatorului cu cross-device matching la interpretarea attribution → concluzii greșite din date incomplete

---

---

## §8 — ENFORCEMENT LOOP (META-H-002)

**WHERE**: La planificarea lunară de buget cross-canal (Pas 5) + la activarea oricărui canal nou

**WHEN**:
- La activarea primului canal non-last-touch (TikTok/SEO): setup schema VID obligatoriu (Pas 1)
- Prima zi a fiecărei luni: actualizare touchpoint log + calculare modele atribuire (Pas 3) + analiză căi (Pas 4)
- La fiecare decizie de realocare buget >30% pentru orice canal: analiză path assist obligatorie
- La atingerea pragului >500 conversii/lună: migrare la BigQuery (Pas 2 Scale)

**HOW — Violations critice**:
- **VIOLAȚIE CRITICĂ**: Tăierea bugetului TikTok/SEO bazat exclusiv pe last-touch revenue fără analiza path assist → risc pierdere canal inițiator principal
- **VIOLAȚIE CRITICĂ**: Decizii de buget cu shift >30% per canal fără minimum 30 zile de date multi-touch
- **VIOLAȚIE**: Ignorarea incompatibilității operatorului cu cross-device matching la interpretarea attribution → concluzii greșite
- **VIOLAȚIE**: Canal cu assist >30% din conversii tăiat fără justificare documentată

**CONNECT**:
- **M-121** (S2S Tracking): M-131 extinde M-121; click_id din M-121 este baza pentru VID schema
- **M-119** (TikTok/Native Ads): analiza time-decay din M-131 alimentează deciziile de spend TikTok/native
- **M-120** (WhatsApp/SMS Funnel): touchpoint-urile WhatsApp din M-120 se înregistrează în schema M-131
- **M-117** (Competitive Intelligence): benchmark journey length din M-117 contextualizează analizele de path

**VERIFY**:
- [ ] VID configurat în Voluum (cv1, cv2, cv3, cv4) pentru toate canalele active?
- [ ] Acoperire VID >80% din FTD-uri cu VID valid (raport Voluum cv1)?
- [ ] Cele 3 modele de atribuire calculate lunar (last-touch + linear + time-decay)?
- [ ] Compatibilitate operator verificată (`operator-attribution-compatibility.md` actualizat)?
- [ ] Nicio decizie de realocare buget fără `budget-reallocation-{GEO}-{YYYY-MM}.md` generat?
- [ ] Re-testare la 45 zile după fiecare realocare executată?

**VK-uri obligatorii**:
- `✅ [PROC] M-131 | GEO: {GEO} | luna: {YYYY-MM} | conversii acoperite: {X}% | delta-max: {X}% | realocare: {DA/NU}`
- `✅ [CORTEX] "M-131 Attribution {GEO} {YYYY-MM}" | rule: TRAIN-S-001 | v1.0`

**MODEL ROUTING**:
- **Claude Opus** (claude-opus-4-8): interpretare discrepanțe semnificative (delta >30%), analiza căilor de conversie (Pas 4 — insight-uri strategice), setup schema inițial (Pas 1)
- **Claude Sonnet**: calcule lunare modele atribuire (Pas 3), generare tabele output, formatare rapoarte budget reallocation

---

*Procedură creată conform standardului FORGE v1.2 — Genie Slim Core*
