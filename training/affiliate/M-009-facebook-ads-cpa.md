---
type: procedure
created: 2026-03-20
status: active
slug: m-009-facebook-ads-cpa
tags: [procedure, nexus]
---

# M-009 — Facebook Ads for CPA Offers

**Domeniu**: affiliate | **Sub-domeniu**: facebook-ads-cpa
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Configurarea si rularea campaniilor Facebook Ads pentru oferte CPA, de la setup Business Manager pana la scaling profitabil cu optimizare continua. Facebook Ads este sursa de trafic cu cel mai avansat algoritm de targetare dar si cea mai stricta politica de advertising — direct linking la CPA offers e interzis, gambling/nutra/dating sunt restrictionate. Procedura asigura setup tehnic corect si metodologie sistematica de testare/scaling fara ban.

**SMSads Context**: Facebook Ads nu este sursa primara SMSads (SMS carrier = primar), dar este canal complementar puternic. Combo: Facebook Ads capture lead → email/SMS nurture → conversie pe oferta CPA. Facebook Lookalike audiences bazate pe converters din SMS traffic pot amplifica volumul 5-10x. De asemenea, Facebook retargeting pe vizitatorii LP-urilor SMS care nu au convertit = a doua sansa de conversie la cost redus.

**Ce se intampla fara procedura:**
- Direct linking la CPA offer pe Facebook → ban cont in <24h
- Scaling agresiv (>20%/zi) → learning phase resetata → CPA creste 2-3x
- Audienta prea larga (10M+ people) → Facebook nu optimizeaza eficient → cost ridicat
- Pixel neinstalat → zero date de conversie → Facebook nu poate optimiza
- Creative-uri neconforme → ad rejected → cont flagged → eventual ban

---

## Pasi

### Pas 1: CREARE FACEBOOK BUSINESS MANAGER SI AD ACCOUNT

Setup complet:
1. business.facebook.com → Create Account
2. Adauga metoda de plata (card credit/debit — nu prepaid, risc de rejection)
3. Verifica identitatea business (daca ceruta — Upload doc oficial)
4. Seteaza timezone (timezone GEO target — nu timezone personala) si currency (USD)
5. Creaza ad account dedicat per nisa/vertical (nu un cont pentru toate)
6. Adauga 2FA pe Business Manager (obligatoriu — fara 2FA, recovery imposibil dupa hack)

**Eroare frecventa**: Un singur ad account pentru toate verticalele. Daca gambling ad account e banned, pierzi si contul cu oferte legit. Conturi separate = izolare risc.

### Pas 2: INSTALARE FACEBOOK PIXEL CU CONVERSII

Setup Pixel:
1. Events Manager → Create Pixel → copiaza Pixel ID
2. Instaleaza base code pe landing page header (sub `<head>`)
3. Configureaza Standard Events:
   - `ViewContent` pe landing page (toți vizitatorii)
   - `Lead` pe opt-in form submit
   - `InitiateCheckout` pe click CTA catre oferta
   - `Purchase` la conversie (fire via Voluum pixel passthrough)
4. Testare: Facebook Pixel Helper extension Chrome → verifica ca events fire corect

**Integrare Voluum**: Voluum poate fire Facebook Pixel la conversie (postback → pixel). Setup: Voluum → Campaign → Facebook pixel integration → paste Pixel ID → selecteaza event (Purchase).

### Pas 3: SELECTIE OBIECTIV CAMPANIE

| Obiectiv | Cand il folosesti | Cost |
|----------|------------------|------|
| **Conversions** | Ai 50+ conversii/saptamana pe pixel | Cel mai mic CPA la scala |
| **Traffic** | Start — sub 50 conversii, nu ai date | CPC mai mic, dar CPA mai mare |
| **Lead Generation** | Capture email/phone direct in Facebook | Bun pentru list building |

**Regula**: Start cu Traffic → acumuleaza 50+ conversii → migreaza la Conversions. Nu folosi Brand Awareness sau Engagement pentru CPA — irelevante.

### Pas 4: CONSTRUIRE AUDIENTA (3 STRATEGII PARALELE)

**Strategia 1: Interest Targeting**
- 3-5 interese relevante per ad set (nu prea multi — diluie audienta)
- Age range specific ofertei (gambling: 21-55, trading: 25-45, health: 30-60)
- Audienta target: 500K-2M (nu prea mica <100K, nu prea mare >5M)
- Creaza 2-3 ad sets cu audiente diferite pentru testare

**Strategia 2: Lookalike Audiences (post 100 conversii)**
- Upload customer list (emails/phones converters din SMS traffic) → Create Lookalike
- Start cu 1% Lookalike (cel mai similar) → scale la 2% si 5%
- GEO-specific: Lookalike Bolivia 1% ≠ Lookalike Peru 1%

**Strategia 3: Custom Audiences (retargeting)**
- Website visitors (pixel-based) — ultimele 7/14/30 zile
- Video viewers (50%, 75%, 95% watched)
- Engaged (liked, commented, shared posts)
- EXCLUDE converters (nu cheltui pe oameni care deja au cumparat)

### Pas 5: CREARE AD CREATIVE-URI (MINIM 3 VARIANTE)

Tipuri de creative ordonate dupa performance:
1. **Video ad** (15-30 sec): hook in primele 3 secunde, demonstratie produs, CTA la final. Best performer pe mobile
2. **Image ad**: produs in context de utilizare, text overlay minim, culori contrastante cu feed-ul
3. **Carousel**: 3-5 slides cu beneficii diferite, fiecare cu CTA

**Copy angles (testeaza minim 3):**
- Pain point: "Obosit de [problema]? [Produs] rezolva in [timp]"
- Aspiration: "Imagineaza-te [rezultat dorit]. Cu [Produs], e posibil."
- Curiosity: "De ce 50,000 de oameni au ales [Produs] luna asta?"
- Social proof: "[Nume] a [rezultat]. Asta e povestea lui."
- FOMO: "Oferta se incheie [data]. [N] locuri ramase."

**Reguli Facebook Ads gambling/restricted:**
- NU promite castiguri garantate
- NU afisa sume de bani in creative (se poate interpreta ca gambling ad)
- Foloseste pre-sell approach: "Afla cum sa..." nu "Castiga $1000..."
- Disclaimer obligatoriu in ad text daca promovezi gambling cu aprobare

### Pas 6: DESIGN PRE-SELL LANDING PAGE (OBLIGATORIU)

Facebook INTERZICE direct linking la oferte CPA. Pre-sell page intermediara:

**Formate acceptate:**
- **Articol editorial** (500-800 cuvinte): informativ, educativ, cu CTA catre oferta la final
- **Quiz/survey**: 3-5 intrebari care pre-califica vizitatorul → redirect la oferta per rezultat
- **Story format**: "Cum am [rezultat] in [timp] cu [abordare]" → CTA catre produs
- **Comparison**: "[Produs A] vs [Produs B] — care e mai bun?" → CTA catre winner

**Reguli compliance Facebook:**
- Pagina trebuie sa fie functionala (nu redirect automat)
- Continut real (nu doar CTA)
- Nu claim-uri false sau testimoniale fabricate
- Disclaimer vizibil daca e necesar (gambling, health, finance)
- Trebuie sa respecte Facebook Advertising Policies complet

### Pas 7: SETARE BUGET DE TESTARE

Buget phase:
- **Test phase**: $20-50/zi per ad set (3-5 ad sets = $60-250/zi total)
- **Learning phase**: nu modifica NIMIC in primele 24h (Facebook optimizeaza)
- **Regula 20%**: nu creste/scade bugetul cu mai mult de 20% pe zi (altfel resetezi learning phase)
- **Minim spend per ad set**: 2x payout-ul ofertei inainte de decizie kill/scale

### Pas 8: LANSARE SI MONITORIZARE 24-48 ORE

Metrici de urmarit:

| Metrica | Target | Sub Target = |
|---------|--------|--------------|
| CTR (link clicks) | >1.0% | Creative slaba → testeza altele |
| CPC | <$0.50 (T2/T3 GEO) | Audienta prea competitiva |
| LP CR (vizitatori → CTA click) | >15% | LP slaba → redesign |
| CPA | <0.7x payout | Profitabil |
| Frequency | <3.0 | Peste 3 = ad fatigue |

**NU face modificari in primele 24 ore** — learning phase. Exceptie: clear loss (CTR <0.3% dupa 1000 impressions = creative total nefunctionala → kill).

### Pas 9: KILL RULES STRICTE

| Situatie | Actiune |
|----------|---------|
| Spend = 2x payout, zero conversii | KILL ad set imediat |
| CPA > 1.5x payout dupa 2x payout spend | KILL |
| CTR <0.5% dupa 5000 impressions | KILL creative |
| Frequency >4 | Roteste creative (ad fatigue) |
| CTR bun + LP CR bun + zero conversii | Problema e OFERTA, nu ad-ul → testeza alta oferta cu aceeasi audienta |

### Pas 10: SCALING CASTIGATORI (3 METODE)

**Metoda 1: Vertical Scaling (slow, stabil)**
- Creste bugetul pe ad sets existente cu max 20% la fiecare 48h
- Ex: $50/zi → $60/zi → $72/zi → $86/zi
- AVANTAJ: stabil, nu resetezi learning
- DEZAVANTAJ: lent

**Metoda 2: Horizontal Scaling (rapid)**
- Duplica ad set winner cu audiente noi (alte interese, alte demographics)
- Pastreaza creative si LP identice
- AVANTAJ: rapid, testeaza audiente noi
- DEZAVANTAJ: unele audiente nu performeaza

**Metoda 3: Lookalike Scaling (cel mai puternic)**
- Upload converters → Create 1% Lookalike → ruleaza cu winning creative
- Scale: 1% → 2% → 5% (fiecare nivel mai mare = audienta mai larga, CR mai mic)
- AVANTAJ: audienta pre-calificata de Facebook AI
- DEZAVANTAJ: necesita 100+ converters pentru Lookalike de calitate

**Regula universala**: nu scala mai mult de 20%/zi. Scaling agresiv resetează learning phase → CPA creste → profit dispare.

---

## Verificare

- [ ] Business Manager + Ad Account configurat cu 2FA?
- [ ] Pixel instalat si functional (verificat cu Pixel Helper)?
- [ ] Minim 3 creative-uri create (video/image/carousel)?
- [ ] Pre-sell landing page live (no direct linking)?
- [ ] Audienta definita (500K-2M, interests relevante)?
- [ ] Buget test setat ($20-50/ad set)?
- [ ] Kill rules definite si respectate?
- [ ] Monitoring 24-48h completat cu metrici documentate?
- [ ] Scaling gradual (max 20%/48h)?
- [ ] Compliance check: zero claim-uri false, disclaimer prezent?

---

## Instrumente

| Tool | Rol |
|------|-----|
| Facebook Business Manager | Platform advertising |
| Facebook Pixel + Events Manager | Tracking conversii |
| Facebook Pixel Helper (Chrome ext) | Verificare pixel |
| Voluum / BeMob | Tracking si pixel passthrough |
| Canva / CapCut | Creare creative-uri (image/video) |
| Pre-sell landing page builder | WordPress/Unbounce/Carrd |
| Facebook Ad Library | Spy competitori |

---

## Note

- Depinde de M-007 (oferte CPA selectate) + M-008 (tracking Voluum).
- Facebook Ads este COMPLEMENTAR SMS traffic-ului, nu inlocuitor.
- Gambling ads pe Facebook necesita pre-approval per GEO — verifica Facebook Advertising Policies.
- Ad account ban = pierderea datelor pixel. Backup: exporta Custom Audiences regulat.
- LATAM GEOs (Bolivia, Peru, Chile) au CPM $2-5 (foarte ieftin vs US $15-30) — oportunitate de testare la cost redus.
