---
type: procedure
created: 2026-03-20
status: active
slug: aim-107-ai-email-marketing-personalization
tags: [procedure, nexus]
---

# AI Email Marketing Personalization at Scale — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Utilizarea AI pentru personalizarea email marketing la scară — de la segmentare comportamentală avansată și subject line optimization la generarea de variante de email per segment, secvențe automate cu logic branching, și performance analysis cu recomandări AI. Include prompt templates per tip de email, automation workflows, și ROI framework.

---

## 1. Problema

Email-urile generice au open rates de 15-20% și CTR sub 2%. Personalizarea reală (nu doar „Bună, [FIRSTNAME]") crește open rates la 25-40% și CTR la 4-8%, dar este imposibil de implementat manual la mii de contacte. AI permite trei niveluri de personalizare care anterior cereau echipe de 3-5 persoane: (1) segmentare granulară cu profile psihologice per segment, (2) copy adaptat per segment (nu variante de merge tag, ci emailuri fundamental diferite), (3) optimizare continuă bazată pe date (subject lines, timing, content). Un email marketer cu AI produce la nivelul unei echipe de 3 fără AI.

Situații acoperite:
- Campanie de email marketing pe o listă segmentată (broadcast)
- Secvență de onboarding/welcome (3-7 emailuri automate)
- Nurture sequence B2B (7-14 emailuri, 30-60 zile)
- Re-engagement campanie (>3 luni fără interacțiune)
- Email transacțional personalizat (post-purchase, winback, upsell, cross-sell)
- Cold email outbound (sales prospecting)
- Newsletter recurent cu personalizare de conținut

---

## 2. Procedura

### Pas 1: Segment — Segmentează lista pentru personalizare reală (nu cosmetică)

**Principiul cheie**: Personalizarea eficientă = emailuri fundamental diferite per segment, nu același email cu FIRSTNAME schimbat. Segmentarea determină ce scrii, nu cum formatezi.

**Identifică 5-8 segmente acționabile din CRM/ESP:**

| Criteriu | Segmente | Ce personalizezi |
|----------|----------|-----------------|
| Comportament achiziție | Cumpărat / Nu a cumpărat / Cumpărat recent / Dormant | Oferta, urgency level, social proof type |
| Valoare client | High-value (top 20%) / Mid / Low | Exclusivitate, loyalty rewards vs. volume deals |
| Stadiu funnel | Lead rece / Nurture / Client activ / Churning | Conținut educational vs. produs vs. retention |
| Engagement email | Very active (>50% open) / Active / Low / Inactive (>90 zile) | Frecvență, agresivitate CTA, re-engagement |
| Sursă achiziție | Organic / Paid / Referral / Event | Contextul primei interacțiuni |
| Produs/categorie | Categorie A / Categorie B / Multi-category | Cross-sell relevance, content topics |
| Industry/Role (B2B) | Decision maker / User / Influencer | Limbaj, pain points, ROI framing |

**Verificare date**: nu poți personaliza pe date inexistente. Audit rapid: ce câmpuri sunt populate >80% în CRM? Acelea sunt segmentele viabile. Restul = proiect de data enrichment separat.

### Pas 2: Profile — Construiește profilul psihologic per segment cu AI

Prompt pentru **Claude Sonnet** (segment profiling):
```
Ești un email marketing strategist cu 10 ani experiență.

CONTEXT: [Business — ce vinzi, pentru cine]

SEGMENTUL: [Nume segment — ex: „Clienți care au cumpărat o singură dată acum 2-3 luni"]

Construiește un profil detaliat al acestui segment:

1. STAREA MENTALĂ: Ce gândesc despre [brand/produs] acum? Ce simt?
   (ex: „Satisfăcuți dar nu loiali. Au uitat de noi. Nu au motivație de re-cumpărare.")

2. MOTIVAȚIE DE ACȚIUNE: Ce i-ar convinge să [acțiunea dorită — recumpărare/upgrade/etc.]?
   Top 3 triggere: [urgency? discount? produs nou? social proof? FOMO?]

3. OBIECȚII PROBABILE: De ce NU au [cumpărat din nou/upgradat/etc.]?
   Top 3 obiecții: [preț? nu au nevoie? au uitat? experiență slabă?]

4. TON RECOMANDAT: Cum ar trebui să le vorbim?
   [Familiar/personal / professional / educational / urgent / exclusive]

5. TIPURI DE EMAIL RELEVANTE: Ce tipuri de email-uri ar funcționa cel mai bine?
   [Product update / Case study / Discount / Content value / Event invite / Survey]

6. TIMING RECOMANDAT: Când să trimitem? Frecvența optimă?
   [Imediat / weekly / bi-weekly] [Dimineața/seara] [Ziua săptămânii]
```

### Pas 3: Subject Lines — Generează și testează sistematic per segment

Prompt pentru **ChatGPT GPT-4o** (subject line optimization):
```
Generează 15 subject lines pentru un email destinat segmentului:
[Descriere segment din Pasul 2]

Email type: [welcome / nurture / promo / re-engagement / announcement]
Oferta/mesajul: [ce comunici]
Brand: [numele]
Ton: [din Pasul 2 — recomandare]

Categorii de testat:

CURIOSITY (3): Creează gap informațional
Ex: „Am observat ceva despre contul tău..."

BENEFIT (3): Promite valoare directă
Ex: „Reducere 30% la [produs] — doar 48 ore"

PERSONAL (3): Simte personalizat (chiar dacă e la scară)
Ex: „[Firstname], ai ratat asta data trecută"

URGENCY (3): Creează presiune temporală
Ex: „Expiră la miezul nopții: [ofertă]"

QUESTION (3): Pune o întrebare relevantă
Ex: „Ai rezolvat [problemă] sau încă cauți?"

BONUS: 2 subject lines cu emoji strategic (1 emoji, la început sau final)
BONUS: 2 subject lines extra scurte (3-5 cuvinte)

Lungime optimă: 30-50 caractere (mobile preview friendly).
Preview text sugerat: 40-90 caractere per subject line.
```

**A/B Testing protocol:**
- Liste >500 contacte: OBLIGATORIU A/B test pe subject line
- Split: 15% grupul A, 15% grupul B, 70% winner (după 2-4 ore)
- Testează doar 1 variabilă (subject line) — nu schimba și body simultan
- Track: open rate (subject line test), CTR (body copy test)

### Pas 4: Email Body — Generează variante per segment (nu template unic)

**Template 1 — Welcome Email (new subscriber):**
```
Scrie un welcome email pentru [brand] destinat unui nou subscriber din [sursa — organic/paid/event].

SEGMENTUL: [din Pasul 2]
TON: [warm, personal, valuable — nu corporate]
OBIECTIV: [setează expectativa + oferă valoare imediată]

Structură:
1. GREETING: Personal, nu generic. Referă sursa de subscribe dacă o știi.
2. PROMISE: Ce va primi de la tine (frecvență, tip conținut, beneficiu)
3. INSTANT VALUE: 1 resursă/tip/insight valoros chiar acum (nu link generic la blog)
4. QUICK WIN: 1 acțiune pe care o poate face în 2 minute și să vadă rezultat
5. PS: Element personal sau bonus neașteptat

Lungime: 150-250 cuvinte. Fără paragraf mai lung de 3 propoziții.
Mobile-friendly: line breaks, bullet points, font > 14px.

NU include: 5 CTA-uri, wall of text, „Dear valued customer", links la 10 pagini.
```

**Template 2 — Nurture Email (educational):**
```
Scrie un nurture email pe tema „[subiect]" pentru segmentul [segment].
Acesta este emailul #[X] din secvența de [Y] emailuri.
Emailul anterior a fost despre: [subiect anterior].

SEGMENTUL: [din Pasul 2]
OBIECTIV: Educă + construiește autoritate + avansează spre [conversion action]

Structură:
1. OPENING: Referință la un pain point sau situație specifică segmentului (2 propoziții)
2. INSIGHT: 1 idee valoroasă, specifică, acționabilă (nu platitudine generală)
3. PROOF: Exemplu, case study, sau statistică care susține insight-ul
4. APPLICATION: Cum aplică cititorul asta concret, astăzi
5. BRIDGE: Leagă natural de [produsul/serviciul tău] fără a fi pushy
6. CTA: Soft CTA (learn more / read guide / reply with questions) — nu „BUY NOW"

Ton: helpful expert, nu salesman. Raport valoare/vânzare: 80/20.
```

**Template 3 — Re-engagement Email (dormant contacts):**
```
Scrie un re-engagement email pentru contacte inactive (>90 zile fără open/click).
Brand: [brand]. Produs: [produs].

Segmentul: contacte care au cumpărat/s-au abonat dar nu mai deschid emailuri.

Variante de abordare (generează 3):

VARIANTA A — „Te-am pierdut?":
Ton: personal, ușor vulnerabil. „Am observat că nu ne-ai mai citit. Am greșit cu ceva?"
Include: opțiune de preferințe (primește mai rar / doar promoții / dezabonare)

VARIANTA B — „Iată ce ai ratat":
Ton: FOMO pozitiv. Arată 2-3 lucruri concrete pe care le-a ratat (produs nou, feature, articol top).
Include: „cel mai popular [lucru] din ultimele 3 luni"

VARIANTA C — „Cadou de bun venit înapoi":
Ton: generos, no-strings. Oferă ceva valoros (discount, free resource, early access).
Include: urgency pe ofertă (48 ore)

Fiecare variantă: max 150 cuvinte. Subject line inclus.
CTA clar: un singur buton, o singură acțiune.
```

**Template 4 — Post-Purchase / Upsell:**
```
Scrie un email post-purchase trimis la [X zile] după achiziția produsului [produs].

Segmentul: clienți care au cumpărat [produs] (valoare: [preț]).

Structură:
1. CONFIRMATION de valoare: „Sperăm că [produs] te ajută cu [beneficiu]"
2. QUICK TIP: 1 mod de a obține mai mult din produs pe care majoritatea nu-l știu
3. SOCIAL PROOF: „Iată ce spun alți clienți care folosesc [produs]: [testimonial]"
4. CROSS-SELL natural: „Clienții care au ales [produs] folosesc adesea și [produs 2] pentru [beneficiu complementar]"
5. CTA: Low-pressure — „Descoperă [produs 2]" (nu „Cumpără acum")

Ton: helpful, post-purchase glow, nu agresiv.
```

### Pas 5: Sequence — Structurează secvențe automate cu logic branching

**Onboarding Sequence (7 emailuri, 30 zile):**

```
Ziua 0: Welcome + instant value + set expectations
         → IF clicks resource → Tag: „Engaged Early"
Ziua 2: Educational tip #1 (pain point principal)
         → IF opens → continue sequence
         → IF doesn't open → resend with new subject line (Ziua 3)
Ziua 5: Case study / social proof relevant
         → IF clicks → Tag: „Interest Signal"
Ziua 10: Objecția #1 adresată direct + FAQ
         → IF Engaged Early AND Interest Signal → fast-track to offer
Ziua 14: Soft offer / CTA principal
         → IF converts → exit sequence, enter „Customer" flow
         → IF doesn't convert → continue
Ziua 21: Value-add content (no pitch, pure value)
Ziua 30: Final email — direct offer + urgency/scarcity
         → IF doesn't convert → move to „Nurture Long" sequence (monthly)
```

**Generare secvență completă cu AI:**
```
Creează o secvență de [X] emailuri pentru [obiectiv] pe [Y] zile.

SEGMENTUL: [din Pasul 2]
BRAND: [din brief]
CONVERSION ACTION: [ce vrei ca cititorul să facă la final]

Pentru fiecare email, specifică:
1. Ziua de trimitere
2. Subject line (2 variante A/B)
3. Tipul emailului (educational / social proof / objecție / ofertă)
4. Email body complet
5. CTA specific
6. Branching logic (IF opens/clicks → ce se întâmplă)
7. Tag-uri de aplicat bazat pe comportament

REGULI:
- Emailurile trebuie să formeze un fir narativ coerent (nu 7 emailuri izolate)
- Raport valoare/pitch: 70/30 pe primele 5 emailuri, 40/60 pe ultimele 2
- Include minim 1 email „pure value" fără CTA de vânzare
- Include minim 1 email care adresează obiecția principală direct
```

### Pas 6: Technical — Implementare în ESP cu personalizare dinamică

**Klaviyo** ($20+/mo, e-commerce leader):
- Conditional content blocks: arată produse diferite per segment (browse history, purchase history)
- Dynamic product feeds: „Recomandate pentru tine" bazat pe AI de Klaviyo
- Predicted analytics: CLV prediction, churn risk score
- Flow branching: splits pe comportament (opened, clicked, purchased)

**ActiveCampaign** ($29/mo, B2B/services):
- Conditional content: `{% if contact.tag == „high-value" %}` blocks diferite
- Lead scoring: automatic scoring bazat pe email engagement + website visits
- CRM integration: sales pipeline triggers email sequences
- Automations map: vizual flow builder cu logic branching

**Mailchimp** ($13+/mo, entry-level):
- Merge tags: `*|FIRSTNAME|*`, `*|PRODUCT_NAME|*`
- Content blocks condiționate pe segment tag
- Send time optimization: AI alege ora optimă per contact
- A/B testing built-in pe subject line, content, send time

**Instantly** ($30/mo, cold email outbound):
- Spintax: `{Hi|Hey|Hello} {FIRSTNAME}` — variații automate
- Warmup: email warmup inclus (esențial pentru deliverability)
- Sequence builder: multi-step cu delays și conditions
- A/B testing: variante de email cu tracking per variantă

**Pre-launch checklist tehnic:**
- [ ] Merge tags testate (trimite test email la adresa proprie)
- [ ] Conditional blocks verificate pe fiecare segment
- [ ] Links funcționale (nu broken, cu UTM parameters)
- [ ] Unsubscribe link prezent și funcțional
- [ ] Mobile preview verificat (50%+ deschid pe mobil)
- [ ] Plain text version exists (pentru email clients fără HTML)
- [ ] SPF/DKIM/DMARC configured (deliverability)
- [ ] Preview text setat (nu doar subject line)

### Pas 7: Cold Email — AI-powered outbound prospecting (B2B)

**Prompt pentru cold email sequence (ChatGPT GPT-4o):**
```
Scrie o secvență de 4 cold emailuri pentru outbound B2B.

TARGET: [ICP — rol, company size, industrie]
PRODUS: [ce vinzi]
VALOARE: [ce problemă rezolvi și ce rezultat livrezi]
SOCIAL PROOF: [client similar, rezultat concret]

Email 1 (Ziua 0) — „PROBLEMA":
- Subject line: scurt, personal, nu sales-y (ex: „[Company] + [your company]?")
- Body: max 80 cuvinte. 1 observație specifică despre compania lor.
  1 problemă pe care probabil o au. 1 propoziție despre cum ai rezolvat-o pentru [client similar].
  CTA: „15 minute call?" (nu „schedule a demo")

Email 2 (Ziua 3) — „VALOARE":
- Follow-up cu 1 insight sau resursă relevantă (nu „just checking in")
- Include link la case study sau articol relevant
- CTA: „Worth exploring?"

Email 3 (Ziua 7) — „SOCIAL PROOF":
- Testimonial de la client similar din aceeași industrie sau rol
- Rezultat concret cu cifre
- CTA: „Similar results possible for [their company]?"

Email 4 (Ziua 14) — „BREAKUP":
- Ton: „I respect your time, this is my last email"
- Include 1 final reason/insight
- CTA: „If timing is better in Q[X], reply 'later' and I'll follow up then"

REGULI: Max 80 cuvinte per email. Zero buzzwords. Zero generic „I hope this finds you well".
Personalizare [COMPANY] și [FIRSTNAME] obligatorie. Plain text (no HTML templates).
```

### Pas 8: Analyze — Performance analysis cu AI post-campanie

La 48-72 ore post-campanie, exportă datele și analizează:

Prompt pentru **Claude Sonnet**:
```
Analizează performanța acestei campanii de email marketing:

CAMPAIGN: [nume]
SEGMENT: [segment/lista]
DATE SENT: [data]
LIST SIZE: [nr. contacte]

METRICS:
| Metric | Result | Benchmark |
| Open rate | [X%] | [Y%] |
| Click rate | [X%] | [Y%] |
| Unsubscribe rate | [X%] | [0.3%] |
| Bounce rate | [X%] | [2%] |
| Conversion rate | [X%] | [Y%] |
| Revenue attributed | [$X] | - |

A/B TEST RESULTS:
Subject A: „[text]" — Open: [X%]
Subject B: „[text]" — Open: [X%]

CLICK MAP (top links clicked):
1. [link] — [X clicks]
2. [link] — [X clicks]

Analiză:
1. Ce a mers bine și de ce? (corelează cu subject line, content, segment, timing)
2. Ce a mers sub benchmark și care sunt cauzele probabile?
3. Subject line winner: de ce a câștigat? Ce pattern sugerează?
4. Click map: ce conținut a interesat cel mai mult? Ce e ignorat?
5. Unsubscribe rate: e în parametri? Dacă nu, ce indică?
6. TOP 3 recomandări actionable pentru campania următoare
7. Suggestie: un A/B test de rulat data viitoare (ce variabilă)
```

### Pas 9: ROI Framework — Calculează rentabilitatea email AI

```markdown
# Email Marketing AI ROI Calculator

## Timp economisit per campanie:
| Activitate | Fără AI | Cu AI | Savings |
|-----------|---------|-------|---------|
| Segmentare & profiling | 2h | 30min | 1.5h |
| Subject line generation | 1h | 15min | 45min |
| Email copy (3 variante) | 3h | 45min | 2.25h |
| Sequence build (7 emails) | 8h | 2h | 6h |
| A/B test setup | 30min | 15min | 15min |
| Performance analysis | 1h | 15min | 45min |
| TOTAL per campaign cycle | 15.5h | 4h | 11.5h |

## Cost savings (la $50/h):
- Per campanie: $575 saved
- La 4 campanii/lună: $2,300/lună
- Annual: $27,600

## Revenue impact:
- Open rate improvement (25% → 35%): +40% mai mulți cititori
- CTR improvement (2% → 5%): +150% mai multe clicks
- Conversion improvement: varies, but typically +20-50% cu personalizare reală
- LTV improvement cu secvențe nurture: +15-25%

## Tool costs:
- Claude Pro + ChatGPT Plus: $40/mo
- ESP (Klaviyo/ActiveCampaign): $29-100/mo
- Make.com automation: $9/mo
- Total: $78-149/mo

## NET ROI: $2,151-2,222/mo sau $25,812-26,664/an
```

### Pas 10: Automation — End-to-end email workflow cu Make.com

**Make.com Email Campaign Pipeline:**
```
Scenario: AI Email Campaign Generator

Trigger: Google Sheet new row (campaign brief completed)

→ Module 1: HTTP → Claude API → Generate segment profiles (Pasul 2)
→ Module 2: HTTP → ChatGPT API → Generate 15 subject lines (Pasul 3)
→ Module 3: Iterator → per segment
  → Module 4: HTTP → ChatGPT API → Generate email body per segment (Pasul 4)
→ Module 5: HTTP → Claude API → QA check (brand voice, compliance)
→ Module 6: Google Docs → Create campaign document with all emails
→ Module 7: Slack → Notify: „Campaign ready for review. [X] emails, [Y] segments."

For sequences:
→ Module 8: HTTP → Claude API → Generate full sequence (Pasul 5)
→ Module 9: Google Sheet → Write sequence timeline + emails
→ Module 10: Slack → Notify with implementation checklist for ESP

Estimated time: 15-20 minutes automated vs. 8-15 hours manual
API cost: $2-5 per campaign
```

---

## 3. Cortex Logging

```json
{
  "text": "Email Marketing Personalization executat. Campanie: [X]. Segmente: [Y]. Emailuri generate: [Z per segment]. Subject lines A/B: [nr variante]. Open rate: [%]. CTR: [%]. Revenue: [$X]. Sequences configurate: [nr]. Cost AI: [$X].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-107",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["email-marketing", "personalization", "segmentation", "automation", "sequences", "cold-email", "ab-testing"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + orice campanie email sau configurare secvență automată

### WHEN
La fiecare campanie email sau setup de automatizare

### HOW (violation detection)
- Același email trimis la toată lista fără segmentare → violation
- Subject line unic fără A/B test pe liste >500 → violation
- Secvență automată fără logic branching (linear-only) → violation
- Analiză post-campanie sărită la 72 ore → violation
- Cold email fără warmup configured → violation critică (deliverability)
- Emailuri fără mobile preview check → violation
- Missing unsubscribe link → violation critică (legal)

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-106 → ad copy (continuitate mesaj paid → email nurture)
- AIM-111 → reporting (metrici email în dashboard)
- AIM-103 → content pipeline (content emails din blog posts)

### VERIFY
- [ ] Minim 3 segmente definite cu profiluri psihologice?
- [ ] Subject lines: 15+ generate, A/B test configurat?
- [ ] Email body diferit per segment (nu doar FIRSTNAME)?
- [ ] Personalizare dinamică testată pe email de test?
- [ ] Secvență automată cu logic branching?
- [ ] Mobile preview și plain text verificate?
- [ ] SPF/DKIM/DMARC configured?
- [ ] Analiză post-campanie planificată la 72h?
- [ ] ROI calculat?

**VK-uri:**
1. `✅ [PROC] AIM-107 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Email Marketing Personalization at Scale" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Segment profiling | Claude Sonnet | Analiză psihologică, nuanță |
| Subject line generation | ChatGPT GPT-4o | Creativitate, hooks, punch |
| Email body copy | ChatGPT GPT-4o | Ton conversațional, persuasiune |
| Cold email sequences | ChatGPT GPT-4o | Concizie, directness |
| QA și brand voice check | Claude Sonnet | Detectare AI-isms, consistency |
| Performance analysis | Claude Sonnet | Structured analysis, recomandări |
| Segmentare strategică | Claude Opus | Viziune, complex segment design |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ESP (Klaviyo $20+/mo / ActiveCampaign $29/mo / Mailchimp $13+/mo) | Trimitere, automation, analytics |
| ChatGPT GPT-4o / Claude Sonnet | Generare copy, profiling, analysis |
| CRM (HubSpot / Pipedrive / Salesforce) | Date segment și comportament |
| Instantly ($30/mo) | Cold email outbound cu warmup |
| Make.com ($9/mo) | Pipeline automation |
| Google Analytics 4 | Email attribution conversions |

---

## 6. Instrumente

- ChatGPT Plus ($20/mo) — copywriting email, subject lines, cold email
- Claude Pro ($20/mo) — segment profiling, QA, performance analysis
- Klaviyo (free-$100+/mo) — e-commerce email personalization leader
- ActiveCampaign ($29/mo) — B2B automation, CRM, lead scoring
- Instantly ($30/mo) — cold email outbound, warmup, deliverability
- Phrasee (enterprise) — AI subject line optimization
- Litmus ($99/mo) — email preview cross-client, QA
- Make.com ($9/mo) — pipeline automation
- Beehiiv (free-$49/mo) — newsletter platform cu monetization

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Open rate (broadcast) | ≥ 25% |
| Open rate (sequences) | ≥ 35% |
| CTR (broadcast) | ≥ 3% |
| CTR (sequences) | ≥ 5% |
| Unsubscribe rate | ≤ 0.3% |
| Bounce rate | ≤ 2% |
| Segmente active per campanie | ≥ 3 |
| A/B tests per campanie | ≥ 1 |
| Revenue per email sent | Track & improve MoM |
| Cold email reply rate | ≥ 5% |
| Sequence completion rate | ≥ 60% |
