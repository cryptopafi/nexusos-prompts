---
type: procedure
created: 2026-03-20
status: active
slug: m-004-affiliate-landing-page
tags: [procedure, nexus]
---

# M-004 — Affiliate Landing Page Construction

**Domeniu**: affiliate | **Sub-domeniu**: landing-page
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Constructia unei landing page de affiliate optimizata pentru conversie, cu bonus offer unic, lead capture, A/B testing integrat, si optimizare mobile-first. Direct linking fara landing page = CR cu 40-60% mai mic si zero control asupra relatiei cu vizitatorul. Pe SMS traffic (100% mobil), landing page sub 3 secunde load time e obligatorie — altfel bounce rate depaseste 70%.

**SMSads Context**: Pe SMS, user-ul clickeaza pe link de pe telefon mobil pe retea 3G/4G. Landing page-ul trebuie sa fie sub 100KB total, load <2s, un singur CTA vizibil fara scroll. Formatul ideal SMS LP: logo + 1 headline + 3 bullets + CTA button mare. Zero navigatie, zero footer, zero blog. Exemple: MarathonBet Bolivia LP = logo + "Bonus 100% primer deposito" + buton verde "REGISTRATE AHORA". AvaTrade LP = "Invierte desde $10" + form simplu (email + telefon).

**Ce se intampla fara procedura:**
- Direct linking la oferta affiliate → CR 40-60% mai mic (user nu e pre-sold)
- Landing page lenta (>3s load) → 53% abandon pe mobil (Google data 2025)
- Lipsa lead capture → fiecare vizitator e o oportunitate unica, nerecuperabila
- Lipsa A/B testing → rularea perpetua a unui LP mediocru
- LP fara bonus offer → zero diferentiere fata de alti afiliati care promoveaza aceeasi oferta

---

## Pasi

### Pas 1: ALEGERE BUILDER SI HOSTING RAPID

Selecteaza builder in functie de nevoie:
- **WordPress + Elementor** (recomandat): flexibil, cost scazut ($0-15/luna), plugin ecosystem mare. Hosting obligatoriu rapid: Cloudways ($14/luna, TTFB <200ms) sau Hostinger ($3/luna, acceptabil pentru start)
- **Unbounce** ($99/luna): A/B testing nativ, drag-and-drop, dar cost ridicat. Ideal cand testezi 5+ variante simultan
- **ClickFunnels** ($147/luna): funnel complet (LP + checkout + upsell), overkill pentru single LP affiliate
- **Carrd.co** ($19/an): minimalist, rapid, perfect pentru SMS LP-uri simple (1 pagina, 1 CTA). Recomandat initial pentru SMSads

**Criterii obligatorii**: TTFB <200ms, SSL gratuit (Let's Encrypt), CDN inclus (Cloudflare minim), mobile-responsive built-in, integrare email provider.

**Eroare frecventa**: Hosting shared la $3/luna cu TTFB >1s = landing page incarca in 4-5s pe mobil 3G Bolivia = 70%+ bounce. Investitia de $14/luna in hosting rapid se recupereaza din prima conversie.

### Pas 2: DESIGN STRUCTURA PAGINA — FLOW DE CONVERSIE

Structura landing page optimala (ordinea conteaza):

**Above the fold (vizibil fara scroll pe mobil)**:
1. **Headline**: adreseaza pain point-ul principal SAU beneficiul #1. Max 8 cuvinte. Exemplu: "Gana dinero real apostando desde tu celular"
2. **Sub-headline**: amplifica promisiunea. Max 15 cuvinte
3. **CTA #1** (button proeminent): text actional, culoare contrastanta, minim 48px inaltime pe mobil

**Below the fold**:
4. **Problem statement**: 2-3 randuri care amplifica problema pe care produsul o rezolva
5. **Product introduction**: prezentare scurta a solutiei (produsul affiliate)
6. **3-5 beneficii** in format bullet points (cu iconite vizuale)
7. **Social proof**: testimoniale, numar utilizatori, rating stele
8. **Bonus offer** (diferentiatorul tau unic — vezi Pas 3)
9. **CTA #2** (button identic cu CTA #1)
10. **FAQ** (3-5 intrebari frecvente — reduce obiectiile)
11. **CTA #3** (final)

**Regula 3 CTA**: above the fold + dupa social proof + la final. Nu mai mult de 3 — excesul deruteaza.

### Pas 3: CREARE BONUS OFFER UNIC (DIFERENTIATOR OBLIGATORIU)

Bonusul diferentiaza landing page-ul tau de alti afiliati care promoveaza aceeasi oferta. Optiuni:

**Tipuri de bonus per vertical:**
| Vertical | Bonus Type | Exemplu | Valoare Perceputa |
|----------|-----------|---------|-------------------|
| Gambling | Strategii betting | "Ghid 50 pagini: Strategii Sportive 2026" | $47 |
| Trading | Trading templates | "10 Setup-uri Forex Testate pe AvaTrade" | $97 |
| Software/SaaS | Tutorial video | "Masterclass 2h: Cum sa faci X cu [Product]" | $197 |
| Health/Nutra | Meal plan + workout | "Plan alimentar 30 zile + exercitii" | $29 |
| App installs | Cheatsheet | "Ghid rapid: Primii pasi in [App]" | $15 |

**Reguli bonus**: relevant pentru produs (nu random), valoare perceputa 2-3x pretul produsului, livrabil digital instant (PDF, video link, access grup), creat de tine (nu download de pe internet — copyright issues).

**Livrare bonus**: prin email dupa opt-in (creaza si lista de email — M-005). Nu livra bonusul pe landing page direct — pierzi lead-ul.

### Pas 4: SCRIERE COPY FRAMEWORK AIDA + PAS

Doua framework-uri combinate:

**AIDA (structura generala)**:
- **Attention**: headline care opreste scrollul — intrebare, statistica surpriza, sau promisiune bold
- **Interest**: fapte/statistici care amplifica problema
- **Desire**: beneficii concrete cu proof (nu features — beneficii)
- **Action**: CTA clar cu urgenta reala (nu fake)

**PAS (pentru landing page-uri de problemsolving)**:
- **Problem**: descrie problema vivid (user-ul se recunoaste)
- **Agitate**: amplifica consecintele problemei nesolvate
- **Solution**: prezinta produsul ca solutia + bonus-ul tau

**Reguli de copy SMS LP**:
- Paragrafuri scurte: max 2-3 propozitii
- Bold pe key phrases (nu pe totul)
- Un singur link/CTA per sectiune (mai multe deruteaza)
- Limba nativa GEO (spaniola peruana pentru Peru, NU spaniola europeana)
- Zero jargon tehnic — scrie la nivel de clasa a 8-a

**Eroare frecventa**: Copy focusat pe features ("Are 47 de functii!") in loc de beneficii ("Economisesti 10 ore pe saptamana"). Features nu vand — beneficiile vand.

### Pas 5: ADAUGARE SOCIAL PROOF REAL

Elemente de social proof (in ordinea eficientei):
1. **Testimoniale cu nume si foto reale** (cele mai puternice — cere vendorului sau colecteaza de la cumparatori)
2. **Numar de utilizatori/cumparatori** ("Folosit de 50,000+ oameni in 12 tari")
3. **Rating stele** (4.5+ stele cu numar de reviews)
4. **Media logos** ("As seen on Forbes, Inc, Entrepreneur" — doar daca real)
5. **Screenshots rezultate** (earnings, analytics, before/after)
6. **Trust badges**: SSL, "30-day money back guarantee", logos payment methods

**Daca nu ai testimoniale proprii**: foloseste cele de pe sales page-ul vendorului (cu atribuire: "Sursa: [Product] official page"). Alternativ: fa-ti propriul review/test si posteaza rezultatul.

**Pentru gambling/betting**: INTERZISE testimoniale cu sume castigate specifice in multe jurisdictii. Foloseste social proof de tip "Peste 1 milion de utilizatori" + rating Trustpilot. Verifica M-118 compliance per GEO.

### Pas 6: EMBED AFFILIATE LINKS CU TRACKING SI CLOAKING

Setup link-uri:
1. **Link cloaking** (WordPress: Pretty Links sau ThirstyAffiliates): transforma `uglylonglink.com/aff?id=123&s1=xxx` in `yoursite.com/go/product-name/`
2. **Parametri tracking Voluum/BeMob**: s1=traffic_source, s2=creative_id, s3=lp_variant, s4=geo, s5=device
3. **UTM parameters** (Google Analytics paralel): utm_source=sms, utm_medium=affiliate, utm_campaign=product_name
4. **Toate CTA-urile duc la acelasi tracking URL** (nu link-uri diferite pe aceeasi pagina)
5. **Test obligatoriu**: click pe fiecare CTA → verifica in tracker ca click-ul apare → verifica ca redirect-ul ajunge la oferta corecta

**Eroare frecventa**: Link-uri affiliate cu parametri tracking diferiti pe fiecare CTA → date fragmentate in tracker. Un singur tracking URL per landing page, repetat pe toate CTA-urile.

### Pas 7: SETUP LEAD CAPTURE FORM ABOVE THE FOLD

Formular opt-in integrat:
- **Campuri**: doar email (sau email + prenume maxim). Fiecare camp in plus reduce opt-in rate cu 20%
- **Headline form**: "Primeste [Bonus Name] GRATUIT" (nu "Aboneaza-te la newsletter")
- **Button text**: "TRIMITE-MI BONUSUL" (nu "Submit" — nimeni nu vrea sa "submita" nimic)
- **Pozitie**: above the fold pe desktop, imediat dupa headline pe mobil
- **Provider**: ConvertKit (cel mai bun pentru affiliates, $15/luna), MailerLite (gratuit pana la 1000 contacte), AWeber

**Flow**: opt-in → email automat cu bonus download → email #2 la 24h cu link afiliat + recommandare produs → secventa M-005.

**Privacy**: GDPR compliance — checkbox "Accept terms" daca trafic EU (Romania). Bolivia/Peru/Kenya nu au GDPR echivalent strict, dar adauga privacy policy link pe formular.

### Pas 8: A/B TESTING SISTEMATIC

Testeaza doua versiuni — un singur element diferit pe test:

**Prioritate de testare (in ordine)**:
1. Headline (cel mai mare impact: 20-40% diferenta CR)
2. CTA button (text + culoare: 10-25% impact)
3. Bonus offer (tipul de bonus: 15-30% impact)
4. Layout (above fold structure: 10-20% impact)

**Setup**: split traffic 50/50 folosind Voluum/BeMob (rotation feature) sau Unbounce (built-in). Minim 200 vizitatori per varianta inainte de a declara castigator. Semnificanta statistica: diferenta CR >15% relativ.

**Kill rule**: Dupa 500 vizitatori per varianta, daca diferenta CR <5% → ambele variante sunt echivalente, alege cea mai simpla.

### Pas 9: OPTIMIZARE VITEZA MOBILE (TARGET: <2 SECUNDE)

Optimizare critica pentru SMS traffic (100% mobil, adesea 3G):
- **Imagini**: WebP format, max 50KB per imagine, lazy loading below the fold
- **CSS/JS**: inline critical CSS, defer non-critical JS, elimina plugin-uri inutile
- **Fonturi**: max 1 font extern (Google Fonts), sau system fonts only pentru viteza maxima
- **Total page weight**: sub 100KB ideal, sub 200KB acceptabil
- **Testare**: Google PageSpeed Insights (target: 90+ mobile score), WebPageTest (3G Slow preset)
- **CDN**: Cloudflare (gratuit) — activare obligatorie pentru toate GEO-urile

**Eroare frecventa**: WordPress cu 15 plugin-uri active + tema cu page builder heavy = 3MB page, 6s load pe 3G. Solutie: tema lightweight (GeneratePress/Astra), max 5 plugin-uri, cache plugin (WP Super Cache gratuit).

### Pas 10: COMPLIANCE SI DISCLOSURE

Elemente obligatorii:
- **FTC Disclosure** (US traffic): "This page contains affiliate links. I may earn a commission at no extra cost to you." — vizibil, nu ascunsa in footer
- **Gambling disclaimer** (pe GEO-urile target): "El juego puede crear adiccion. Juega responsablemente. +18." (Peru), "Gambling is addictive. Play responsibly. 18+." (Kenya)
- **Privacy Policy**: pagina separata, link din footer
- **Terms of Service**: pagina separata, link din footer
- **Cookie notice**: daca folosesti cookies de tracking (obligatoriu EU/Romania)

---

## Verificare

- [ ] Landing page live si functionala pe mobile?
- [ ] Load time <2s pe 3G (verificat PageSpeed Insights)?
- [ ] Bonus offer creat si livrabil prin email?
- [ ] Lead capture form activ si conectat la email provider?
- [ ] A/B test configurat cu minim 2 variante?
- [ ] Toate CTA-urile au tracking link identic si functional?
- [ ] Compliance disclosure prezent (FTC + GEO-specific)?
- [ ] Social proof adaugat (minim 2 tipuri)?
- [ ] Mobile speed score >= 90 pe PageSpeed?
- [ ] Privacy Policy si Terms of Service publicate?

---

## Instrumente

| Tool | Rol |
|------|-----|
| WordPress + Elementor / Carrd.co / Unbounce | Builder landing page |
| Cloudways / Hostinger | Hosting rapid (TTFB <200ms) |
| Cloudflare | CDN gratuit + SSL |
| ConvertKit / MailerLite / AWeber | Email provider lead capture |
| Pretty Links / ThirstyAffiliates | Link cloaking WordPress |
| Voluum / BeMob / RedTrack | Tracking clicks + A/B split |
| Google PageSpeed Insights | Verificare viteza mobile |
| Canva | Creare imagini featured + banners |

---

## Note

- Depinde de M-002 (programul affiliate selectat determina continutul paginii).
- Este prerequisite pentru M-005 (email funnel) si M-113 (content marketing).
- Re-evaluare: la fiecare 30 zile — actualizeaza bonus, refresh testimoniale, testeaza headline nou.
- Pentru SMS traffic, LP-ul minimal (logo + headline + CTA) poate performa mai bine decat LP-ul lung. Testeaza AMBELE variante.
- Landing page-uri separate per GEO (limba nativa + compliance local) — nu o singura pagina pentru toate GEO-urile.
