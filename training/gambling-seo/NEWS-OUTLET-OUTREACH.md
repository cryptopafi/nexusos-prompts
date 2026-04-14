---
type: procedure
created: 2026-03-20
status: active
slug: news-outlet-outreach
tags: [procedure, nexus]
---

# TRAIN-SEO-011: NEWS-OUTLET-OUTREACH — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate + Editorial Link Building + LATAM/Africa News Market -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Link Building / Editorial Placements / Authority Building -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-011
**Scope**: Identificarea, contactarea și negocierea plasamentelor de conținut sponsorizat pe news outlets cu DR 60-85 în piețele Peru, Kenya și Chile pentru construirea autorității prin backlink-uri dofollow de la domenii de știri cu trust ridicat Google.

---

## 1. Problema

News outlets cu DR 60-85 sunt cel mai subevaluat activ în gambling SEO. Google acordă trust implicit site-urilor de știri (NewsGuard, Google News index) — un backlink dofollow de pe un news outlet regional în Peru sau Kenya transmite autoritate comparabilă cu un link de pe un site autoritar clasic, dar la un cost de 3-7x mai mic ($300-800 vs $2,000-5,000). Problema: outreach-ul nestructurat produce rate de răspuns sub 5% și negocieri prost calibrate care lasă bani pe masă sau plătesc suprapreț.

Această procedură rezolvă trei eșecuri tipice:
1. **Prospecting slab** — contactarea site-urilor care vând link-uri public (Google-penalizate) sau care au policies anti-gambling
2. **Email-uri generice** — rată de răspuns 2-3% vs 15-25% cu personalizare corectă
3. **Negociere naivă** — acceptarea primului preț citat (de regulă 40-60% mai mare decât floor-ul real)

**Business Application**: Gambling SEO Project (Leo, 2026-03-04) — construirea unui portofoliu de 20-40 plasamente news per piață (Peru/Kenya/Chile) pentru a susține parasiti SEO și money site-uri.

---

## 2. Procedura

### Pas 1: Prospect List Building — Identificarea Outlet-urilor Țintă

#### Metoda 1: Google Dorking Direct

Caută în Google cu acești operatori pentru fiecare piață țintă:

**Peru (Spanish)**:
```
"advertise with us" site:.pe news
"write for us" site:.pe casino OR apuestas
"contenido patrocinado" site:.pe noticias
"publicidad" "contacto" site:.pe gambling OR casino
```

**Kenya (English)**:
```
"advertise with us" site:.ke news
"sponsored content" site:.ke casino OR betting
"guest post" site:.ke gambling
"write for us" site:.ke sports betting
```

**Chile (Spanish)**:
```
"advertise with us" site:.cl noticias
"contenido patrocinado" site:.cl casino
"publicidad" site:.cl apuestas OR juegos online
"escríbenos" site:.cl gambling
```

Pentru fiecare rezultat, deschide site-ul și verifică manual: există pagina "Advertise" / "Contact" / "Publicidad"? Dacă da — adaugă la lista raw.

#### Metoda 2: Ahrefs Competitor Backlink Mining

Ia 3-5 parasiți sau money sites care rankează deja în top 5 pentru keyword-ul tău seed în piața țintă.

Exemplu pentru Peru: dacă `mejores-casinos-peru.com` rankează #3 → în Ahrefs → Site Explorer → introdu domeniu → Backlinks → filtrează: **Link type: Dofollow | DR Source: 50-90 | Traffic: 10K+**.

Exportă rezultatele. Filtrează manual pentru:
- Domenii care conțin cuvinte: `news`, `noticias`, `diario`, `periodico`, `times`, `post`, `herald`, `tribune`, `daily`, `gazette`
- Exclud: directories, aggregators, PBN-uri (pattern: articole rare, design vechi, fără social media)

Aceasta produce lista cea mai valoroasă — outlet-uri care **deja** au acceptat conținut gambling de la competiție.

#### Metoda 3: BHW Marketplace Research

Pe BlackHatWorld (blackhatworld.com) → Services → Link Building → caută `casino news placement Peru` / `Kenya news links` / `Chile gambling placement`.

Notează vendori cu:
- Rating 4.8+ și minim 50 reviews
- Samples de site-uri livrate (verifică în Ahrefs că nu au traffic drops recente)
- Preț listat (referință pentru negocieri directe — sub prețul BHW = succes)

#### Criteriile de Calificare (Screening Grid)

Înainte de a adăuga un outlet la lista finală, completează acest grid pentru fiecare:

| Criteriu | Threshold | Tool |
|----------|-----------|------|
| Domain Rating (DR) | 60-85 (sweet spot) | Ahrefs |
| Monthly Organic Traffic | 100,000+ vizitatori | Ahrefs / SimilarWeb |
| Traffic Trend (12 luni) | Stabil sau crescând | Ahrefs → Traffic History |
| Outbound Links (OBL) pe site | Sub 500 total | Ahrefs → Linked Domains |
| Google News Indexed | Preferat (nu obligatoriu) | Google: `site:outlet.com` cu News tab |
| Anti-gambling policy explicită | ABSENT | Caută în ToS / Editorial Policy |
| Google Penalty istoric | ABSENT (fără traffic drops bruște >40%) | Ahrefs → Traffic History |

**Dimensiunea listei țintă**: Minim 100 outlet-uri calificate per piață pentru a genera 20-40 plasamente (rată conversie estimată 20-40%).

---

### Pas 2: Prospecting Data — Ce Colectezi Înainte de Contact

Pentru fiecare outlet calificat, documentează într-un spreadsheet:

| Coloana | Ce notezi |
|---------|-----------|
| Site URL | URL complet homepage |
| DR | Valoare Ahrefs |
| Monthly Traffic | Valoare Ahrefs |
| Contact Email | Găsit în pagina Advertise/Contact (preferă editor@ / ads@ / content@) |
| Contact Name | Dacă găsit (Editor, Ad Manager, Content Director) |
| Recent Article Link | 1 articol relevant pe care îl vei cita în email |
| Article Topic | Topicul articolului citat (bază pentru personalizare) |
| Has "Advertise" Page | DA / NU |
| Language | EN / ES / Swahili |
| Estimated Price Range | Referință BHW sau estimare (DR×10 USD ca proxy inițial) |

**Tool recomandat**: Google Sheets cu aceste coloane. Separă sheet-uri pe piețe (Peru / Kenya / Chile).

---

### Pas 3: Email Outreach — Template-uri Verbatim

Personalizarea emailului este esențială. Rata de răspuns cu template generic: 2-4%. Cu personalizare (citat articol specific + nume): 15-25%.

#### Template A — English (Kenya / sites cu content EN)

**Subject line**: `Sponsored Content Opportunity — [Their Exact Site Name]`

```
Hi [First Name / "there" dacă nu ai numele],

I came across your piece on [specific article title or topic — ex: "sports betting regulations in Kenya"]
at [exact article URL] — really insightful coverage of [one specific detail from the article].

We're looking to place sponsored content on quality publications covering [country] audiences.
Your readership and editorial quality look like a strong fit.

Do you accept sponsored articles or paid content placements? If so, could you share your
editorial guidelines and pricing?

A bit about us: we produce informational content about online gaming and sports betting —
written to match your editorial standard, not promotional fluff.

Thanks for your time,

[Your branded name — ex: "Marcus / Content Partnership Team"]
[Branded email — ex: marcus@[yourbrand].com]
[Website URL]
```

#### Template B — Spanish (Peru / Chile)

**Subject**: `Oportunidad de Contenido Patrocinado — [Nombre Exacto del Sitio]`

```
Hola [Nombre / "equipo editorial"],

Vi su artículo sobre [tema específico del artículo] en [URL exacta del artículo] —
excelente cobertura de [un detalle específico del contenido].

Estamos buscando publicar contenido patrocinado en medios de calidad que lleguen
a audiencias en [país]. Su publicación encaja perfectamente con lo que buscamos.

¿Aceptan artículos patrocinados o colocaciones de contenido pago? Si es así,
¿podría compartir sus lineamientos editoriales y tarifas?

Producimos contenido informativo sobre juegos online y apuestas deportivas —
redactado para cumplir con sus estándares editoriales, no contenido puramente publicitario.

Muchas gracias por su tiempo,

[Nombre branded — ex: "Carlos / Equipo de Alianzas de Contenido"]
[Email branded — ex: carlos@[tubrand].com]
[URL del sitio]
```

#### Template C — Follow-Up (după 7 zile fără răspuns — o singură dată)

**Subject**: `Re: Sponsored Content Opportunity — [Site Name]` (reply la emailul inițial)

```
Hi [Name / "there"],

Just following up on my note from last week regarding sponsored content placement.

If this isn't something you're currently offering, no worries at all — happy to be
removed from your list.

If timing is off but you'd be open to it in the future, I'd love to keep the
conversation open.

Either way, thanks for your time.

[Same signature]
```

**Regulă strictă**: Maxim 1 follow-up per contact. Nu mai trimite după cel de-al doilea email ignorat.

#### Personalizare Obligatorie (nu trimite fără acestea)

- `[First Name]` — dacă nu ai, folosește "Hi there" sau "Hola equipo"
- `[specific article URL]` — articol real, citit, relevant (nu homepage)
- `[one specific detail]` — arată că ai citit efectiv articolul (un fapt, statistică sau unghiul editorial)
- `[country]` — piața specifică, nu "international audiences"

---

### Pas 4: Negocierea Plasamentului

#### Structura Negocierii

**Situația 1: Ei citează un preț**

Dacă citează $2,000 pentru un articol:

```
"Thank you for the pricing info! For our first collaboration, we'd love to start
with 2 articles — would you be able to do $1,000 per article if we commit to both
upfront? We're looking to build a long-term relationship if the first pieces perform well."
```

Logica: $2,000 × 2 = $4,000 → oferi $1,000 × 2 = $2,000 total. Ei câștigă volum, tu câștigi 50% discount. Rata de acceptare: ~60-70%.

**Situația 2: Nu au preț listat**

```
"Wonderful — we typically work with budgets of $[400-600 pentru DR60-70] per placement
for initial collaborations. Does that range work for your team?"

[Dacă spun că e prea mic]:

"We completely understand. Could we meet in the middle at $[600-800]?
We'd love to start with 2 pieces if that works."
```

**Situația 3: Ei cer conținut promotional, tu vrei editorial**

```
"We actually prefer informational-style content — more like a guide or explainer
("How Online Casino Regulations Work in [Country]") rather than a direct product review.
This tends to perform better organically for both of us. Would that format work for you?"
```

#### Termenii Non-Negociabili (Cere Explicit)

1. **Dofollow link** — "We do require dofollow links — is that something you can accommodate?"
2. **Permanent placement** — "Would the article remain live indefinitely, or is there a time limit?"
3. **No "Sponsored" label în URL sau title** — ("sponsored-content-casino" în URL = Google îl devalorizează)
4. **Author bio cu link** — bonus, nu obligatoriu

Dacă insistă pe nofollow sau pe "Sponsored" în titlu prominent → reevaluează ROI (link-ul pierde 70-80% din valoare SEO).

#### Metode de Plată

**Preferate**:
- Wire transfer (SWIFT) — standard business, trasabilitate normală
- Crypto (USDT/BTC) — preferat pentru discreție, tot mai acceptat de outlet-uri mici
- Wise / Transferwise — pentru outlet-uri mici care evită banking internațional complex

**De evitat**:
- PayPal — lasă paper trail clar "gambling" în descriere, risc de dispute/chargeback, flagging
- Credit card direct — același motiv

**Invoice**: Cere întotdeauna invoice pe "content creation services" sau "editorial services" — nu "link building" sau "casino advertising".

---

### Pas 5: Content Brief pentru News-Style Articles

Conținutul livrat trebuie să treacă de editorial review și să nu pară publicitate explicită.

#### Specificații Tehnice

- **Lungime**: 800-1,200 cuvinte (nu mai mult — news outlets editează prea mult dacă trimiți 2,500)
- **Format**: Informational / Explainer ("Ghid" / "Cum funcționează" / "Ce trebuie să știi")
- **Titlu recomandat**: Nu include brand-ul direct în titlu
  - ✅ "Online Casino Regulations in Peru: What Players Need to Know in 2026"
  - ✅ "Sports Betting in Kenya: A Complete Guide for New Players"
  - ❌ "Best Casino Peru — Play at [Brand] Today"
- **Link placement**: 1-2 dofollow links, poziționate natural în paragraf (nu în ultimul paragraf)
- **Anchor text mix**:
  - 1× branded anchor (ex: "CasinoPerú.com")
  - 1× partial match keyword (ex: "mejores casinos online en Peru")
  - Evită exact match anchor ca text principal (over-optimization signal)
- **Structură internă**: H2 + H3 headers, fără liste excesive (pare spam), paragrafe scurte (2-4 rânduri max pentru news style)
- **Disclosure**: Dacă cer "Sponsored Content" label — acceptabil dacă e doar tag metadata, nu în titlu/URL

#### Unghiuri Editorial Preferate per Piață

**Peru**:
- "Cómo elegir un casino online seguro en Perú — Guía 2026"
- "Regulación de apuestas online en Perú: lo que necesitas saber"
- "Los mejores métodos de pago para casinos online en Perú"

**Kenya**:
- "Online Betting in Kenya: Licensing, Safety and What Players Should Know"
- "How to Choose a Trusted Online Casino in Kenya — 2026 Guide"
- "M-Pesa and Online Gambling: A Complete Payment Guide for Kenyan Players"

**Chile**:
- "Casinos Online en Chile: Marco Legal y Guía para Jugadores 2026"
- "Cómo depositar en un casino online desde Chile de forma segura"
- "Bonos de Casino Online en Chile: Qué son y cómo funcionarlos"

---

### Pas 6: Volume Strategy și Timeline

#### Săptămâna 1 — Launch

- **Luni**: Finalizează lista de 100+ outlet-uri calificate per piață (folosind Pas 1-2)
- **Marți**: Trimite batch 1 — 50 emailuri (25 EN pentru Kenya, 25 ES pentru Peru/Chile)
- **Miercuri-Vineri**: Monitorizează inbox, răspunde la replies în max 4 ore

#### Săptămâna 2 — Follow-Up și Negociere

- **Luni** (ziua 7 după batch 1): Trimite follow-up pentru cei care nu au răspuns
- **Marți**: Trimite batch 2 — 50 emailuri noi
- **Restul săptămânii**: Negociezi termenii cu cei care au răspuns pozitiv

#### Săptămâna 3-4 — Content Delivery

- Pentru fiecare outlet confirmat: livrezi draft în 48-72 ore de la confirmare
- Plata: 50% la confirmare, 50% la publicare (sau 100% upfront dacă outlet-ul e verificat anterior)
- După publicare: verifică în Ahrefs că link-ul e indexat și dofollow (1-2 săptămâni pentru indexare)

#### Targeturi per Piață

| Piață | Emailuri trimise | Răspunsuri estimate (15%) | Plasamente finale (50% din răspunsuri) |
|-------|-----------------|--------------------------|----------------------------------------|
| Peru | 100 | 15 | 7-8 |
| Kenya | 100 | 15 | 7-8 |
| Chile | 100 | 15 | 7-8 |
| **Total** | **300** | **45** | **20-25** |

---

### Pas 7: ROI Tracking și Breakeven Analysis

#### Formula ROI per Plasament

```
COST: $700/articol (medie negociată, DR70 outlet)

TRAFFIC IMPACT (estimat, 90 zile post-publicare):
  - Boost organic traffic pe parasite/money site: +150-250 vizitatori/lună
  - Conversie estimată: 2% (industrie gambling standard)
  - Conversii lunare: 3-5 utilizatori noi
  - Commission medie per utilizator activ: $50-80 (revenue share)
  - Venit lunar per plasament: $150-400

BREAKEVEN:
  - $700 cost ÷ $200 venit mediu lunar = 3.5 luni
  - Breakeven conservator (dacă traficul e sub estimare): 5-6 luni

POST-BREAKEVEN (lunar, pe perioadă nedeterminată):
  - 1 articol × $200/lună = $200 pasiv
  - 20 articole × $200/lună = $4,000/lună pasiv
  - 40 articole × $200/lună = $8,000/lună pasiv
```

#### Spreadsheet Tracking — Coloane Obligatorii

| Coloana | Ce înregistrezi |
|---------|-----------------|
| Outlet URL | URL publicat |
| DR | DR la data publicării |
| Cost Plătit | USD efectiv |
| Data Publicare | YYYY-MM-DD |
| Link URL | URL exact al articolului publicat |
| Anchor Text | Textul folosit |
| Link Target | Unde pointează (parasite URL / money site) |
| Indexare Confirmată | DA/NU (verificat în Google: `link:target-url`) |
| Traffic Delta (30 zile) | Creștere trafic pe target după publicare |
| Conversii Generate | Dacă ai tracking (UTM params) |
| Venit Lunar Estimat | Calcul pe baza conversiilor |
| Luni până Breakeven | Cost ÷ Venit Lunar |

#### Red Flags Post-Publicare

- Outlet-ul modifică link-ul în nofollow fără notificare → contactează imediat, cere refund sau corectare
- Articolul e șters în primele 60 de zile → politica de "permanent" placement a fost ignorată → dacă ai plătit și nu rectifică, documentează pentru dispute
- Traffic drop brusc pe outlet (>40% în Ahrefs) → posibilă penalizare Google → monitorizeaza și evită link-uri ulterioare de pe acel domeniu

---

## 3. Cortex Logging

### Ce se Salvează Obligatoriu

**La finalizarea fiecărui plasament**:
```
Skill salvat: "News Outlet Placement — [Outlet URL] DR[X], $[cost], [piață]"
Tag-uri: gambling-seo, news-outreach, link-building, [piata]
Conținut: URL articol, anchor text, link target, cost, data, venit estimat
```

**La descoperirea unui outlet de calitate excepțională** (DR80+, traffic 500K+, acceptă gambling):
```
Skill salvat: "Premium News Outlet Identificat — [URL], DR[X], [piață]"
Tag-uri: gambling-seo, premium-outlet, [piata]
Conținut: Toate metricile, contact găsit, preț negociat
```

**La identificarea unui vendor BHW verificat**:
```
Skill salvat: "BHW Vendor Verificat — [username], [specialitate], prețuri"
Tag-uri: gambling-seo, bhw, vendor
Conținut: Username, tipuri de site-uri, DR range, prețuri, metode plată acceptate
```

### Format Cortex Entry

```json
{
  "type": "SKILL",
  "title": "News Outlet Placement #[N]",
  "date": "YYYY-MM-DD",
  "tags": ["gambling-seo", "news-outreach", "link-building", "[piata]"],
  "data": {
    "piata": "[Peru/Kenya/Chile]",
    "outlet": "[URL]",
    "dr": "[X]",
    "traffic_monthly": "[X]",
    "cost_usd": "[X]",
    "link_url": "[URL articol]",
    "link_target": "[URL target]",
    "anchor_text": "[text]",
    "indexat": "DA/NU",
    "data_confirmare_index": "YYYY-MM-DD",
    "venit_estimat_lunar": "[X]",
    "breakeven_luni": "[N]",
    "note": "[observații]"
  }
}
```

---

## 4. Enforcement Loop

### WHERE / WHEN / HOW / CONNECT / VERIFY

- **WHERE**: Google Sheets (prospect list + ROI tracker) + Ahrefs (DR/traffic monitoring) + Email inbox (outreach responses)
- **WHEN**: Săptămânal (luni) — outreach metrics. Lunar (1st) — ROI review. La fiecare plasament publicat — verificare link.
- **HOW**: Spreadsheet tracking cu coloanele din Pas 2 și Pas 7. Ahrefs alerts pe outlet-urile cu plasamente active. Email check zilnic în primele 2 săptămâni post-outreach.
- **CONNECT**: Input din CASINO-KEYWORD-RESEARCH.md (anchor text targets). Output spre ROI tracking global gambling-seo. Escalare la Genie dacă red flag apare.
- **VERIFY**: VK-uri obligatorii la fiecare ciclu (vezi mai jos).

### VK-uri Obligatorii

```
VK-NEWS-OUT-001: [News Outreach Cycle] — {N} emailuri trimise, {N} răspunsuri ({X}% rate), {N} plasamente finalizate, cost mediu ${X}/placement
VK-NEWS-OUT-002: [News Placement Live] — {Outlet URL} DR{X}, link dofollow confirmat Ahrefs, cost ${X}, anchor "{text}"
```

### Verificări Săptămânale (Every Monday)

- [ ] Câte emailuri trimise săptămâna trecută? (target: 50+)
- [ ] Câte răspunsuri primite? (target: 15%+ rată răspuns)
- [ ] Câte negocieri active? (target: progres pe toate răspunsurile pozitive)
- [ ] Câte plasamente publicate în ultima săptămână?
- [ ] Toate plasamentele publicate au link-ul indexat și dofollow confirmat?
- [ ] Cortex actualizat cu toate plasamentele noi?

### Verificări Lunare (Every 1st of Month)

- [ ] Traffic delta per articol publicat (Ahrefs — a crescut traficul pe target?)
- [ ] Conversii generate (dacă ai UTM tracking activ)
- [ ] ROI actual vs estimat (este breakeven-ul pe track?)
- [ ] Outlet-uri cu traffic drop brusc (penalizare potențială — scoate din future campaigns)
- [ ] Actualizare spreadsheet cu venituri lunare reale

### Escalare Probleme

**Outlet nu răspunde după follow-up**: Marchează ca "No Response" în spreadsheet, treci la următorul. Nu insista.

**Outlet vrea conținut total promotional**: Explică formatul informational. Dacă refuză, skip — risc editorial review negativ care poate scădea valoarea link-ului.

**Link devine nofollow post-publicare**: Email imediat: *"Hi [Name], I noticed the link in our article appears to have changed to nofollow — could your team take a look? We agreed on dofollow placement."* Dacă nu corectează în 7 zile → refund request sau escalare.

**Outlet primește penalizare Google** (traffic drop >40% în Ahrefs): Nu mai construi link-uri ulterioare pe acel domeniu. Link-urile existente nu le pierzi, dar valoarea lor scade semnificativ.

---

## 5. Dependențe

### Proceduri Anterioare Necesare

- **TRAIN-SEO-001 — Casino Market Analysis**: Piața țintă trebuie să fie selectată și validată înainte de a construi lista de outlet-uri
- **TRAIN-SEO-003 — Parasite Platform Selection**: Trebuie să știi exact unde pointează link-urile (URL parasite sau money site finalizat)

### Proceduri Paralele (pot rula simultan)

- **TRAIN-SEO-004 — PBN Construction**: News outreach și PBN pot rula în paralel — surse de autoritate diferite, nu se exclud
- **TRAIN-SEO-002 — Expired Domain Acquisition**: Domenii expirate pot fi folosite ca "brand" website pentru author bio și credibilitate

### Proceduri Ulterioare Recomandate

- **TRAIN-SEO-012 — Guest Post Campaign** (dacă există): News outreach este high-authority, guest posts pe niche blogs sunt mid-authority — combined = stack de autoritate diversificat
- **TRAIN-SEO-013 — Link Velocity Management** (dacă există): Nu construi toate cele 20-40 link-uri simultan — distribuie pe 3-4 luni pentru pattern natural

### Tool-uri Necesare

| Tool | Utilizare | Cost Estimat |
|------|-----------|--------------|
| Ahrefs | Competitor backlink mining, DR/traffic check, monitoring | $99-199/lună |
| Google Sheets | Prospect list, ROI tracking | Gratuit |
| Hunter.io | Găsire email contact dacă nu e public | $49/lună sau free (50 searches) |
| Email client (branded domain) | Outreach — nu Gmail personal | $12/an domeniu + $6/lună Google Workspace |
| BlackHatWorld (BHW) | Research vendori, backup dacă direct outreach e lent | Gratuit (cont standard) |

### Branded Email — Setup Obligatoriu

Emailurile de outreach NU se trimit de pe `@gmail.com` sau `@yahoo.com`. Setup minim:

1. Cumpără domeniu branded (ex: `contentpartnership.pe` sau `mediapublishing.co.ke`) — $12/an
2. Google Workspace Business Starter ($6/lună) sau Zoho Mail (gratuit)
3. Emailul trebuie să pară un content agency, nu un afiliat SEO

---

**DISCLAIMER**: Această procedură este material educațional pentru SEO și marketing afiliat. Execuția trebuie să respecte legislația locală privind publicitatea pentru gambling din fiecare piață (Peru: MINCETUR, Kenya: BCLB, Chile: Ley 21.456/SCJ). Utilizatorul este responsabil pentru verificarea conformității legale înainte de implementare. Conținutul sponsorizat trebuie etichetat corespunzător conform legilor locale de publicitate.

*Procedură creată: 2026-03-04 | Domeniu: Gambling SEO | Piețe: Peru / Kenya / Chile*
*Bazată pe metodologia Floate + research gambling-seo-resources-2026.md*
