---
type: procedure
created: 2026-03-22
status: active
slug: parasite-page-build
tags: [procedure, nexus]
---

# TRAIN-SEO-005: PARASITE-PAGE-BUILD — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Julian Goldie Parasite SEO + Charles Floate Content Optimization -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Parasite SEO / Content Construction / On-Page Optimization -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-005
**Scope**: Construirea completă a unei parasite pages optimizate pentru nișa casino/gambling — de la keyword research la go-live — targetând piețele Peru, Kenya și Chile cu affiliate revenue maxim.

---

## 1. Problema

Parasite SEO pentru gambling exploatează autoritatea domeniilor terțe (Medium, Quora, Reddit, platforme news, site-uri edu/gov cu user-generated content) pentru a ranka rapid în SERP-uri competitive fără a construi autoritate de domeniu de la zero. Dar 90% din tentativele de parasite pages eșuează nu din cauza link building-ului, ci din cauza on-page slab: conținut prea scurt, fără structură, fără keyword integration corectă, fără rich media, fără optimizare locală pentru piața țintă. Rezultatul: pagini care nu intră în top 20, link budget irosit pe pagini care nu convertesc.

Această procedură asigură execuția corectă în ordine — mai întâi conținut 90%, apoi linkuri 10% — cu workflow AI complet (ICP → RAG → Draft → Humanize → Localize), setup affiliate cu link masking, și checklist go-live cu Force Index obligatoriu înainte de orice cheltuială pe linkuri.

**Piețe țintă principale**: Peru (español) · Kenya (English + Swahili touches) · Chile (español chileno)
**Business Application**: Gambling SEO project Leo — generare trafic organic rankabil rapid pe piețe LATAM + Africa cu conversii affiliate casino.

---

## 2. Procedura

### Pas 1: KEYWORD RESEARCH — SELECTARE TARGET PER PIAȚĂ

Identifică keyword-ul primar și setul de co-occurring keywords înainte de orice altceva.

**1.1 — Tool stack:**
- Ahrefs / SEMrush: KD (Keyword Difficulty), search volume lunar, SERP analysis
- Google Keyword Planner: volume local per țară (filtrează: Peru / Kenya / Chile)
- SurferSEO: SERP average word count, co-occurring keywords, Content Score benchmark

**1.2 — Criterii de selecție keyword primar:**
- KD < 35 pentru site-uri noi / parasite pages pe domenii cu DA < 40
- KD < 55 pentru parasite pe domenii high-authority (Medium DA 95+, Quora DA 90+)
- Search volume local: minim 500/lună (Peru/Chile) sau 300/lună (Kenya — piață mai mică)
- Intent: "best casinos [țară]", "online casino [țară] real money", "casino bonus [țară]", "casino legal [țară]"

**1.3 — Structura keyword set per pagină:**
- 1 keyword primar (ex: "mejores casinos online Peru 2026")
- 3-5 keyword variații (ex: "casinos online Peru", "casino online Peru bonos", "jugar casino online Peru")
- 5-10 co-occurring keywords din top 10 SERP (extrase cu SurferSEO sau manual din primele 3 rezultate)
- 3-5 long-tail pentru FAQ section (ex: "es legal el casino online en Peru", "como depositar en casino online Peru")

**1.4 — Regula anului în titlu:**
- Include ÎNTOTDEAUNA anul curent în title tag și H1: "Mejores Casinos Online Peru 2026"
- Freshness signal puternic în nișa gambling — Google favorizează conținut actualizat

---

### Pas 2: SELECȚIE PLATFORMĂ PARASITE

Alege platforma parasite în funcție de KD al keyword-ului țintă și disponibilitatea posting-ului.

**2.1 — Tier 1 (DA 90+) — pentru KD 40-65:**
| Platformă | DA | Limbă | Disponibilitate |
|-----------|-----|-------|----------------|
| Medium.com | 95 | EN/ES | Free, indexare rapidă |
| LinkedIn Articles | 98 | EN/ES | Free, necesită cont activ |
| Quora Spaces | 90 | EN/ES | Free, indexare lentă |
| Reddit (r/relevant) | 91 | EN | Risc ban — nu pentru primary) |
| HubPages | 82 | EN | Free, acceptă casino content |
| Vocal.media | 78 | EN | Free |

**2.2 — Tier 2 (DA 50-89) — pentru KD 20-45:**
- Site-uri news locale cu guest post opțiune
- Platforme blog din țara țintă (Peru: Peru21 blogs, Kenya: Nairobi News guest)
- Directoare de afaceri cu profil extins

**2.3 — Criterii de selecție:**
- Platforma acceptă conținut gambling? (verifică ToS — unele platforme interzic explicit)
- Indexare rapidă? (testează cu URL deja publicat — caută în Google: "site:platforma.com")
- Permite link-uri externe? (unele platforme adaugă nofollow automat — verifică cu tool-uri)
- Cont existent sau creare necesară? (phantom brand per piață — vezi Pas 6)

---

### Pas 3: RESEARCH — ICP CREATION (Gemini Pro)

Înainte de a scrie un cuvânt, creează ICP (Ideal Customer Profile) pentru piața țintă.

**3.1 — Prompt Gemini Pro pentru ICP:**
```
You are a market research expert. Create a detailed ICP (Ideal Customer Profile) for online casino players in [ȚARĂ TARGET]. Include:
- Demographics (age, gender, income level, education)
- Pain points when choosing an online casino (trust issues, payment methods, language)
- Interests and behaviors (sports betting, slot preferences, live casino)
- Preferred devices (mobile % vs desktop — critical for [ȚARĂ])
- Local payment methods used (M-Pesa for Kenya / Yape/Plin for Peru / Webpay for Chile)
- Language nuances (formal vs informal, local slang for gambling)
Output as structured JSON with reasoning per field.
```

**3.2 — ICP obligatoriu per piață:**
- **Kenya**: 70%+ mobile trafic — conținut TREBUIE optimizat mobile-first. M-Pesa dominant. English local + Swahili expressions familiare.
- **Peru**: Español neutru cu touches locale. Yape/Plin/BCP ca metode plată. Tineri 22-35 ani. Sport betting popular (fútbol).
- **Chile**: Español chileno — "po", "weon" în ton informal opțional. Webpay/Khipu. Casino tradițional vs online — educa diferența.

---

### Pas 4: RAG RESEARCH (Gemini Pro + Web)

Construiește un dataset factual despre piața casino din țara țintă — acest dataset alimentează AI content generation.

**4.1 — Prompt RAG Research:**
```
Research the online casino market in [ȚARĂ] in 2026. Provide:
1. Legal/regulatory status (is online gambling legal? Which authority regulates it?)
2. Top 5 licensed operators active in [ȚARĂ] (name, license, bonus offers)
3. Most popular payment methods for online gambling (with % usage if available)
4. Average player demographics
5. Key competitor pages currently ranking for "[KEYWORD PRIMAR]" — list their word count, structure, unique angles
6. Current promotions/bonuses from top operators (for freshness signals)
7. Any recent news about gambling regulation changes in [ȚARĂ] (2025-2026)
```

**4.2 — Date esențiale de inclus în conținut:**
- Status legal exact (ex: Peru — Ministerio de Comercio Exterior y Turismo reglementează; Kenya — Betting Control and Licensing Board)
- Top 3 operatori affiliate (cu comisioane verificate) + 2 non-affiliate (pentru credibilitate)
- Metode plată locale cu detalii (cum funcționează depunerea cu M-Pesa pas cu pas)
- Orice schimbare legislativă recentă (semnalizează autoritate topică)

---

### Pas 5: AI CONTENT DRAFT — STRUCTURA COMPLETĂ

Generează draft-ul cu structura standard pentru casino review page.

**5.1 — Structura obligatorie (9 secțiuni):**

```
[TITLE] — Keyword primar + An curent (ex: "Mejores Casinos Online Peru 2026")

[INTRO — 150-200 cuvinte]
- Hook cu context local (ex: "En Peru, más de 2 millones de jugadores...")
- Promisiunea paginii (ce va găsi cititorul)
- CTA primar above-the-fold (buton afiliat — produsul #1)

[TOP 3 CASINOS — PRODUSE AFFILIATE PRIMELE]
- Casino #1 (cel mai bun comision afiliat) — include rating vizual, bonus highlight, CTA
- Casino #2
- Casino #3
[Notă: non-affiliate products MERG LA FINAL sau în "alte opțiuni"]

[REVIEW DETALIAT PER CASINO — 3-4 paragrafe per casino]
- Prezentare generală + licență
- Bonusuri și promoții (cu termene și condiții clare)
- Jocuri disponibile (slots, live casino, sports betting)
- Metode de plată (cu focus pe metoda locală dominantă)

[COMPARISON TABLE]
- Bonus welcome | Metode plată | Licență | Rating | CTA

[PAYMENT METHODS SECTION]
- Ghid specific per piață:
  Kenya: M-Pesa step-by-step deposit/withdrawal
  Peru: Yape / Plin / BCP transfer
  Chile: Webpay / Khipu / transferuri bancare

[LEGAL/REGULATORY SECTION]
- Status legal gambling în [ȚARĂ]
- Autoritatea de reglementare + linkul oficial
- Ce înseamnă "casino licențiat" pentru jucătorul local
- Disclaimer responsabilitate (obligatoriu)

[FAQ SECTION — 5-7 întrebări]
- Targetează long-tail queries identificate în Pas 1.3
- Format: Q&A schema markup (Schema.org FAQPage)

[CONCLUSION + CTA FINAL]
- Summary beneficii top 3 casinos
- CTA repetit (buton afiliat)
- Link către resurse responsible gambling
```

**5.2 — Parametri de lungime:**
- Minimum absolut: 2,500 cuvinte
- Target optim: match SERP average (verificat cu SurferSEO — tipic 3,000-4,500 pentru gambling)
- Minimum 10 H2 headings (Google vede structura ca semnal de autoritate)
- 5-10 imagini integrate (screenshots casino, comparison tables vizuale, payment method logos)
- 5-10 link-uri (mix intern + extern — linkuri externe către autorități oficiale gambling, nu către competitori)

---

### Pas 6: UMANIZARE CONȚINUT — SUPER AI PROMPT (Claude Opus)

After generating the draft, run humanization pipeline. AI content detectabil = risc penalizare Google.

**6.1 — Super AI Prompt (aplicat pe draft-ul complet sau per secțiune):**
```
I have created a piece of content on [KEYWORD] for our company. I need you to make this content more engaging for a user, no fluff, remove cringe, increase the humour levels where appropriate but not throughout the content. Write for and as a human, use natural, conversational dialogue and use stemming, lemmatization, n-grams and zipf's law. The target audience is [ICP din Pas 3]. Keep all factual information intact. Do not change casino names, bonus amounts, or regulatory details.
```

**6.2 — Lista completă AI marker words — NICIODATĂ nu folosi:**
landscape, realm, navigating, tailored, unveil, transformative, encompass, dynamic, ecosystem, confluence, facilitate, engaging, quest, solutions, delving, significant, specific, numerous, craft, glean, harness, intricacies, deep, grappling, journey, diving, crucial, meticulous, bespoke, ever-changing, ultimately, leverage, unlock, unveil, furthermore, moreover, additionally, it's worth noting, it's important to, in conclusion, in summary, in the realm of, a testament to, the world of, when it comes to, in today's world, rest assured, I cannot and will not, I need to be direct

**6.3 — Verificare post-humanizare:**
- Rulează conținut prin Originality.ai sau GPTZero — target: < 20% AI score
- Dacă score > 20%: reaplică Super AI Prompt pe paragrafele flagged
- Adaugă 2-3 erori gramaticale minore deliberate (virgulă lipsă, "qui" în loc de "que" ocazional în spaniolă) — semnal uman autentic

**6.4 — Adaptare ton regional:**
- **Peru**: Español standard cu expressions locale ("pues", "oye", referințe la fútbol local)
- **Kenya**: English conversational, poate include "sawa" (OK), referințe la Premier League (popular), M-Pesa integration naturală în text
- **Chile**: Español chileno, poate folosi "oye po", referințe la fútbol chileno sau cultura locală

---

### Pas 7: ON-PAGE OPTIMIZATION (SurferSEO)

Optimizarea on-page reprezintă 90% din succesul parasite page. Nu publica fără acest pas.

**7.1 — SurferSEO Content Editor:**
- Deschide Content Editor pentru keyword primar + locație țară
- Target Content Score: 70+ (optim: 75-85)
- Verifică: word count vs average, keyword density, missing co-occurring keywords, heading structure
- Adaugă keyword-urile lipsă natural în text (nu keyword stuffing — integrare contextuală)

**7.2 — Checklist on-page:**
- [ ] Title tag: keyword primar + an curent (max 60 caractere)
- [ ] Meta description: keyword + beneficiu clar + CTA (max 155 caractere)
- [ ] H1: identic sau aproape identic cu title tag
- [ ] Minimum 10 H2: secțiunile din structura §5.1
- [ ] Keyword primar: apare în primele 100 cuvinte (intro)
- [ ] Keyword density: 1-2% (nu mai mult — risc over-optimization)
- [ ] Co-occurring keywords: toate prezente în text
- [ ] Alt text imagini: include keyword pe minim 2 imagini
- [ ] Internal links: minim 2 (dacă platforma permite)
- [ ] External links: minim 2 (autoritati reglementare oficiale, nu competitori)
- [ ] Schema markup: FAQPage pentru secțiunea FAQ
- [ ] Freshness signal: "Updated [Luna] [An]" vizibil în intro sau sub titlu

**7.3 — Rich media obligatoriu:**
- Screenshot #1: Homepage casino #1 (cu bonus vizibil)
- Screenshot #2: Interface mobil (relevant mai ales pentru Kenya)
- Screenshot #3: Pagina de plată cu metoda locală
- Screenshot #4: Games lobby (diversitate evidențiată)
- Screenshot #5+: Comparison table vizuală (poate fi creat în Canva)
- Imagini optimizate: compresate (max 150KB), format WebP dacă platforma permite, alt text setat

---

### Pas 8: SETUP AFFILIATE — LINK MASKING ȘI PHANTOM BRAND

Affiliate links vizibile = comisioane furate de competitori + tracking failure. Toate linkurile TREBUIE mascate.

**8.1 — Phantom brand per piață:**
- Creează un domeniu branduit per piață țintă (ex: `mejorcasinoperu.com`, `topcasinokenya.com`, `casinoschile.net`)
- Cost: $10-15/an per domeniu
- Email dedicat per phantom brand (nu email personal)
- Scopul: profesionalism + link masking + izolare risc

**8.2 — Link masking cu Rebrandly ($15/mo):**
- Conectează domeniul phantom la Rebrandly
- Creează short links pentru fiecare link afiliat: `mejorcasinoperu.com/betway` → redirect către link afiliat Betway Peru
- Beneficii: tracking centralizat, link-uri schimbabile fără a edita conținutul, aspect profesional
- Alternativă gratuită: Pretty Links plugin (dacă ai WordPress pe domeniu)

**8.3 — Aplicarea simultană la 5-15 programe affiliate:**
- Identifică programele affiliate acceptate per țară:
  - Peru: Betway, 1xBet, Bet365, PokerStars, Betsafe (verifică că acceptă trafic din Peru)
  - Kenya: SportPesa, Betika, Betway Kenya, 1xBet Kenya, Betin
  - Chile: Betsson, 888 Casino, Betsafe, Bet365 Chile, PokerStars
- Aplică simultan la minimum 5 programe per piață (rate de aprobare: ~60-70%)
- Rețele: ShareASale, CJ Affiliate, Awin, Income Access (specializat gambling)
- Timeline aplicare → aprobare: 3-14 zile (aplică înainte de a publica conținutul)

**8.4 — Plasarea linkurilor în conținut:**
- Produse affiliate: TOP paragraphs (primele 3 H2 după intro) — reader-ul convertit timpuriu
- Produse non-affiliate sau reviews neutre: BOTTOM (ultimele 2 secțiuni)
- Fiecare casino menționat are minimum 1 CTA button cu link mascat
- Nu include mai mult de 1 link per 300 cuvinte (risc spam score)

---

### Pas 9: DISCLAIMER ȘI COMPLIANCE

Secțiune obligatorie — absența ei poate duce la ban de platformă sau probleme legale.

**9.1 — Disclaimer standard (include în footer/bas pagini):**
```
[EN] "This page contains affiliate links. We may receive a commission when you sign up through our links, at no additional cost to you. Gambling involves risk. Please gamble responsibly. 18+ only. Check local laws before playing."

[ES] "Esta página contiene enlaces de afiliados. Podemos recibir una comisión cuando te registras a través de nuestros enlaces, sin costo adicional para ti. El juego implica riesgos. Juega con responsabilidad. Solo mayores de 18 años. Verifica las leyes locales antes de jugar."
```

**9.2 — Link către resurse responsible gambling:**
- Includem link extern obligatoriu: Gamblers Anonymous, BeGambleAware, sau echivalent local
- Kenya: National Council for Problem Gambling Kenya
- Peru: Minjus responsabilidad social
- Chile: Jugarbien.cl (site oficial)

---

### Pas 10: FORCE INDEX — OBLIGATORIU ÎNAINTE DE LINK BUILDING

NU cheltui nimic pe linkuri înainte de a verifica dacă pagina rankează organic.

**10.1 — Submit imediat după publicare:**
1. Google Search Console → URL Inspection → "Request Indexing" (necesită verificare site pe platformă sau domeniu propriu)
2. Alternativă pentru platforme parasite: Google Search Console nu e disponibil direct — folosește:
   - Submit URL în Bing Webmaster Tools (indexare rapidă, cross-signals)
   - Speed-Links.net: 50 linkuri gratuite pentru conturi noi — submit URL nou imediat
   - Ping servicii: Pingler.com, Ping-o-Matic (gratuit)

**10.2 — Monitoring pre-link-building (7-14 zile):**
- Verifică în Google după 3-7 zile: `site:platforma.com/url-pagina` — confirmă indexare
- Caută manual keyword-ul primar — dacă pagina intră în top 30 organic = semnal excelent, linkuri vor amplifica
- Dacă pagina intră în top 10 fără linkuri = STOP link building, economisești tot bugetul
- Dacă pagina nu apare în top 50 după 14 zile = on-page are probleme → reauditează §7 înainte de linkuri

**10.3 — Decision tree post-monitoring:**
```
Rankează organic top 10? → NU adăuga linkuri, monitorizează
Rankează organic top 11-30? → 10-20 linkuri tier 2 (guest posts relevante)
Rankează organic top 31-50? → 20-40 linkuri tier 2 + 5 tier 1
Nu apare deloc după 14 zile? → Auditează on-page, nu cumpăra linkuri
```

---

### Pas 11: LINK BUILDING (post-Force-Index)

Executat NUMAI după Pas 10 și NUMAI dacă pagina nu rankează organic în top 10.

**11.1 — Strategie linkuri per tier:**
- **Tier 1 (2-5 linkuri)**: guest posts pe site-uri gambling/casino relevante pentru piața țintă (DA 30+, trafic real, not PBN)
- **Tier 2 (10-30 linkuri)**: web 2.0 properties (Blogger, WordPress.com, Weebly), social profiles, directory submissions cu gambling category
- **Tier 3 (50-100 linkuri)**: social signals, blog comments relevante, forum posts — amplifică tier 2

**11.2 — Anchor text distribution:**
- 30% exact match ("mejores casinos online peru")
- 30% partial match ("casinos online peru bonos")
- 20% branded (phantom brand name)
- 15% generic ("click aqui", "ver mas", "more info")
- 5% naked URL

**11.3 — Velocity și timing:**
- Nu construi mai mult de 5-10 linkuri tier 1 pe săptămână (pattern natural)
- Tier 2 și 3: pot fi mai agresive (50-100/săptămână) — par organice
- Monitor cu Ahrefs/Majestic: verifică că linkurile sunt indexate și nu cauzează spike suspect

---

### Pas 12: FRESHNESS MAINTENANCE (lunar/trimestrial)

Paginile gambling rankate se dezindexează rapid fără semnale de freshness.

**12.1 — Update lunar (30 minute):**
- Actualizează data "Updated [Luna] [An]" în titlu/intro
- Adaugă 1-2 statistici noi despre piața locală (Google: "online gambling statistics [țară] 2026")
- Actualizează bonusurile și promoțiile curente (schimbă bonus amounts dacă operatorii au schimbat)
- Publică din nou pe platforme sociale (Medium, LinkedIn) — semnal engagement

**12.2 — Update trimestrial (2-3 ore):**
- Actualizează screenshots (homepage casino, bonus page, mobile interface)
- Adaugă secțiune FAQ nouă bazată pe search queries din GSC (dacă ai acces) sau din Google Autocomplete
- Verifică linkurile externe — dacă vreun operator a ieșit de pe piață, înlocuiește
- Re-run SurferSEO Content Editor — verifică Content Score după update-urile Google

---

## 3. Cortex Logging

```json
{
  "text": "Parasite page built: [KEYWORD] | Market: [PERU/KENYA/CHILE] | Platform: [PLATFORM] | Content: [N] words, Content Score: [N]/100 | Affiliates: [N] programs linked | Force Index: submitted | Organic rank check: [RESULT] | Link building: [STARTED/PAUSED/NOT NEEDED]",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PARASITE-PAGE-BUILD",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-005",
    "tags": ["gambling", "seo", "parasite_seo", "casino", "affiliate", "peru", "kenya", "chile", "content_marketing"],
    "has_enforcement_loop": true,
    "forge_version": "1.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție + Leo gambling SEO project review

### WHEN
La fiecare execuție a procedurii pentru orice piață sau keyword nou

### HOW (violation detection)
- Pagina publicată fără SurferSEO Content Score 70+ → violation on-page
- Link building pornit fără Force Index check (Pas 10) → violation cost irosit
- Conținut sub 2,500 cuvinte → violation on-page critică
- Affiliate links NEMASCATE (fără Rebrandly/phantom brand) → violation business critică
- Conținut publicat fără disclaimer responsabilitate gambling → violation compliance
- Produse affiliate plasate la BOTTOM în loc de TOP → violation conversion
- AI marker words detectate în conținut final → violation quality
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:**
- Publicarea paginii fără Force Index check (Pas 10) -- link budget irosit fără confirmare indexare
- Lansarea link building pentru pagini care nu rankează deloc după 14 zile (auditează on-page mai întâi)
- Folosirea affiliate links nemascate (fără Rebrandly/phantom brand) -- comisioane furate + tracking failure
- Publicarea fără disclaimer responsible gambling -- risc ban platformă + probleme legale
- Plasarea produselor non-affiliate în TOP și affiliate în BOTTOM -- inversează conversion funnel

### CONNECT
- TRAIN-SEO-005 → enforcement SEO standard
- M-011 (Keyword Research Strategy) → input pentru Pas 1
- M-012 (On-Page SEO Optimization) → extinde §7
- M-013 (Off-Page SEO / Link Building) → extinde §11
- M-113 (Affiliate Content Marketing) → overlap cu §8 affiliate setup
- `procedure-health.json` → adaugă entry
- Training Mode → face parte din curriculum Gambling SEO (Leo)

### VERIFY — Checklist Go-Live

**PRE-PUBLISH:**
- [ ] Keyword research complet (primar + co-occurring + long-tail FAQ)?
- [ ] Platformă parasite selectată și cont activ?
- [ ] ICP creat per piață (Pas 3)?
- [ ] RAG research factual complet (legal status, top operatori, payment methods)?
- [ ] Draft complet cu toate 9 secțiuni din §5.1?
- [ ] Super AI Prompt aplicat + AI score < 20%?
- [ ] AI marker words eliminate din lista §6.2?
- [ ] Ton adaptat regional (Peru/Kenya/Chile)?
- [ ] SurferSEO Content Score 70+?
- [ ] Minimum 2,500 cuvinte?
- [ ] Minimum 10 H2 headings?
- [ ] 5-10 imagini cu alt text setat?
- [ ] 5-10 link-uri (mix intern/extern)?
- [ ] Title tag include keyword + an?
- [ ] Schema FAQPage adăugat?
- [ ] Phantom brand și domeniu creat?
- [ ] Toate affiliate links mascate cu Rebrandly?
- [ ] Produse affiliate plasate în TOP paragrafe?
- [ ] Disclaimer gambling responsabilitate inclus?
- [ ] Link responsible gambling resources inclus?

**POST-PUBLISH:**
- [ ] Force Index: URL submis (GSC / Speed-Links.net / Ping servicii)?
- [ ] Indexare confirmată (site: search) după 3-7 zile?
- [ ] Rank check manual după 7-14 zile?
- [ ] Decision tree §10.3 evaluat?
- [ ] Link building pornit SAU confirmat că nu e necesar?
- [ ] Calendar freshness update setat (lunar + trimestrial)?

**VK-uri obligatorii:**
1. `✅ [PROC] PARASITE-PAGE-BUILD | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Parasite Page Build Casino SEO — [MARKET]" | FORGE ✓ | rule: TRAIN-SEO-005 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Orchestrare execuție procedură | Sonnet (Genie) |
| ICP Creation (Pas 3) | Gemini Pro |
| RAG Research (Pas 4) | Gemini Pro + Web |
| Super AI Prompt humanizare (Pas 6) | Claude Opus |
| SurferSEO audit on-page | Manual / SurferSEO UI |
| Audit periodic procedură | Opus |
| Validare structură FORGE | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| M-011 (Keyword Research Strategy) | Metodologie completă keyword research (extinde Pas 1) |
| M-012 (On-Page SEO Optimization) | Checklist on-page detaliat (extinde Pas 7) |
| M-013 (Off-Page SEO / Link Building) | Strategie link building completă (extinde Pas 11) |
| M-113 (Affiliate Content Marketing) | Affiliate program selection + content strategy |
| SurferSEO (subscription) | SERP analysis, Content Score, co-occurring keywords |
| Ahrefs / SEMrush | Keyword difficulty, volume, competitor backlink analysis |
| Gemini Pro (Google AI Studio) | ICP creation + RAG research (Pas 3-4) |
| Claude Opus | Humanizare conținut (Super AI Prompt, Pas 6) |
| Rebrandly ($15/mo) | Link masking pentru toate affiliate links |
| Phantom brand domeniu ($10-15/an) | Branded link masking per piață |
| Cont platformă parasite activ | Publicare conținut (Medium / LinkedIn / HubPages etc.) |
| Affiliate program accounts | Income Access / ShareASale / CJ / Awin + programe locale |
| Google Search Console | Force Index + monitoring post-publish |
| Speed-Links.net | Free 50 linkuri indexare rapidă (conturi noi) |
| Canva | Comparison tables vizuale, screenshots annotate |
| Originality.ai / GPTZero | Verificare AI score post-humanizare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate pași procedură | 100% (toate 12 pași) |
| VK emisie | 2/2 per execuție |
| Word count pagină | Minim 2,500 (target: SERP average) |
| SurferSEO Content Score | 70+ (optim: 75-85) |
| H2 headings | Minim 10 |
| Imagini cu alt text | 5-10 per pagină |
| AI score (Originality.ai) | Sub 20% |
| Affiliate programs per piață | Minim 5 active |
| Affiliate links mascate | 100% (zero link-uri directe) |
| Force Index executat | 100% pagini publicate |
| Timp până la indexare | 3-7 zile (Medium/LinkedIn) / 7-14 (alte platforme) |
| Rank organic top 30 la 30 zile | Target per pagină nouă |
| Rank organic top 10 la 90 zile | Target cu link building minimal |
| Freshness update frecvență | Lunar (minim) |
| Disclaimer compliance | 100% pagini |
