---
id: AGO-010
title: Service Productization Framework
domain: agency-operations
source: Productized Agency - Start a Productized Agency in 7 Days (Udemy)
version: 2.0
forge_score: "estimated H/3.0"
business_mapping: ["ai-agency"]
---

# Service Productization Framework

## Obiectiv
Transformă serviciile custom ale agenției în produse standardizate (productized services) cu scope fix, preț fix, SOP de livrare documentat și pagina de vânzare dedicată — reducând costul de vânzare cu 60-70%, accelerând onboarding-ul de la zile la ore, crescând marjele cu 15-25%, și eliminând dependency pe fondator pentru delivery.

## Pași

### Pas 1: Identifică serviciile candidat la productizare cu scoring obiectiv
- Listează toate serviciile livrate în ultimele 6-12 luni cu: nume serviciu, nr. de ori livrat, revenue total, ore medii, feedback client
- Per serviciu, evaluează pe 5 criterii (scor 1-5 fiecare):
  - **Repetiție** (5 = identic de la un client la altul, 1 = complet custom): cât de similar e procesul de livrare?
  - **Scope definibil** (5 = perfect delimitabil, 1 = ambiguu): poți defini clar ce e inclus și ce NU?
  - **Cerere** (5 = cerut frecvent, 1 = rar): câți clienți au cumpărat sau întrebat?
  - **Profitabilitate** (5 = margin >60%, 1 = margin <30%): cât câștigi real?
  - **Delegabilitate** (5 = oricine din echipă poate livra, 1 = doar fondatorul): se poate livra fără tine?
- Calculează scor total (max 25). **Servicii cu scor ≥18 = productizare imediată. 13-17 = candidat cu ajustări. <13 = rămâne custom.**
- **Top candidați pentru productizare** (universali):
  | Serviciu | Scor tipic | Preț tipic | Timp livrare |
  |---------|----------|-----------|-------------|
  | SEO Technical Audit | 22-25 | $750-$2,500 | 5-7 zile |
  | Website Speed Optimization | 20-24 | $500-$1,500 | 3-5 zile |
  | Google Ads Audit | 20-23 | $500-$1,500 | 3-5 zile |
  | Social Media Starter Pack | 19-23 | $750-$2,000 | 7-10 zile |
  | Email Marketing Setup | 18-22 | $1,000-$3,000 | 5-10 zile |
  | Landing Page Build | 19-23 | $1,500-$5,000 | 7-14 zile |
  | Brand Identity Mini | 17-21 | $2,000-$5,000 | 10-14 zile |
  | Conversion Rate Audit | 19-22 | $1,000-$3,000 | 5-7 zile |

### Pas 2: Definește scope-ul fix cu delimitare chirurgicală
- **Product Definition Document** (1 pagină per produs):
  ```
  PRODUCT: [Name]
  ONE-LINE: [What client gets in one sentence]

  DELIVERABLES (exact):
  • [Deliverable 1 — with format and quantity]
  • [Deliverable 2]
  • [Deliverable 3]

  INCLUDES:
  • [Specific inclusion 1]
  • [Specific inclusion 2]
  • [X rounds of revisions]
  • [Delivery call / walkthrough — X minutes]

  EXCLUDES:
  • [Specific exclusion 1]
  • [Specific exclusion 2]
  • [Specific exclusion 3]

  TIMELINE: [X] business days from receipt of all client materials
  CLIENT PROVIDES: [List of things client must send before work begins]
  PRICE: $[X] fixed (no hourly, no negotiation)
  ```
- **Exemplu complet — SEO Technical Audit**:
  ```
  PRODUCT: SEO Technical Audit
  ONE-LINE: Complete technical health check of your website with
  prioritized fix list and 45-minute strategy call.

  DELIVERABLES:
  • 20-30 page PDF report covering 50+ technical SEO factors
  • Prioritized action plan: top 15 issues ranked by impact + effort
  • Competitor gap analysis (3 competitors)
  • Core Web Vitals assessment with specific fix recommendations
  • 45-minute video call to walk through findings and answer questions
  • Recording of walkthrough call for your team

  INCLUDES:
  • Crawl of up to 500 pages (Screaming Frog + Sitebulb)
  • Backlink profile analysis (Ahrefs)
  • Keyword gap analysis vs 3 competitors (SEMrush)
  • 1 round of follow-up questions via email (within 7 days of delivery)

  EXCLUDES:
  • Implementation of fixes (available as separate service or retainer)
  • Content audit or content creation
  • Link building
  • Ongoing monitoring
  • Sites with 5,000+ pages (custom quote required)

  TIMELINE: 5 business days from receipt of access credentials
  CLIENT PROVIDES: GA4 admin access, GSC access, CMS login (read-only),
  3 competitor URLs
  PRICE: $1,250 (sites up to 500 pages) | $1,950 (500-2,000 pages)
  ```
- **Revision policy**: X revizii incluse (tipic 2 runde). Revizii adiționale = $150-$350 per rundă. Definiție clară "revision" vs "new request" (revision = change to existing deliverable, new request = additional work not in original scope).

### Pas 3: Stabilește prețul fix cu margin protection
- **Cost calculation** (worst-case, not average):
  - Estimează ore pentru cel mai complex scenariu probabil (nu cel mediu — productized = fixed price, nu ai buffer per client)
  - Include: execution hours + PM overhead (15%) + QA time (10%) + delivery call
  - Exemplu: 8h execution × $55/hr + 1.5h PM + 1h QA + 1h call = 11.5h × $55 = $632 COGS
  - Preț la 60% target margin: $632 / (1 - 0.60) = $1,580 → rotunjit $1,500
- **Pricing rules**:
  - Un singur preț (sau max 2 tiere: Standard + Premium). Nu negociezi.
  - Nu oferi discount (excepție: bundle 3+ produse = 10-15% off)
  - Dacă clientul cere personalizare beyond scope fix = upgrade la custom engagement cu alt pricing model
  - **Minimum product price**: $500. Sub acest prag, costul de vânzare mănâncă marja.
  - **Maximum product price**: $10,000-$15,000. Peste acest prag, clientul vrea custom experience — trebuie proposal + discovery call.
- **Payment terms**: 100% upfront ÎNAINTE de a începe lucrul. Productized = zero risk for agency (scope fix, preț fix, prepaid).
  - Alternativă pentru produse >$3,000: 50% upfront / 50% at delivery
  - Offering: Stripe Checkout, PayPal Business, or bank transfer
- **Price testing**: lansează la prețul calculat. Dacă close rate >60% → crește cu 15-20%. Dacă close rate <30% → fie reduce preț, fie îmbunătățește value proposition. Sweet spot: 40-50% close rate.

### Pas 4: Documentează procesul intern de livrare (SOP complet)
- **SOP structure** per produs (Notion sau ClickUp doc):
  ```
  SOP: [Product Name] — v[Version] — Last Updated: [Date]
  Owner: [Team member responsible for this SOP]

  SECTION 1 — PRE-WORK CHECKLIST
  □ Payment confirmed
  □ Client materials received: [checklist per product]
  □ Access credentials verified (login test)
  □ ClickUp/Asana task created with deadline
  □ Team member assigned

  SECTION 2 — EXECUTION STEPS (numbered, detailed)
  Step 1: [Exact action] — Tool: [tool name] — Time est: [X min]
  Step 2: [Exact action] — Tool: [tool name] — Time est: [X min]
  ...
  Step N: [Compile deliverable into template]

  SECTION 3 — TEMPLATES & ASSETS
  • Report template: [Google Drive link]
  • Email templates: intake email, delivery email, follow-up email
  • Slide deck template (if applicable): [link]

  SECTION 4 — QA CHECKLIST (before delivery)
  □ All sections of report/deliverable completed
  □ Data accuracy verified (spot-check 3 data points)
  □ Client name and branding correct throughout
  □ Links and screenshots current and working
  □ Grammar and formatting check
  □ Second reviewer approved

  SECTION 5 — DELIVERY PROCESS
  □ Send delivery email with [template]
  □ Schedule delivery call (if included) via Calendly
  □ During call: walk through deliverable, answer questions, record
  □ Post-call: send recording link + follow-up email

  SECTION 6 — POST-DELIVERY
  □ Client feedback requested (3-question form)
  □ Time logged in Harvest/Toggl
  □ If upsell opportunity identified: flag to account manager
  □ Update product delivery tracker
  ```
- **Fiecare pas din SOP trebuie să fie executabil de oricine din echipă** — dacă doar fondatorul poate face Step 5, nu e productizat, e consultanță déguisée.
- **Video SOP**: pe lângă text, record un Loom walkthrough (15-20 min) al întregului proces. New team members onboard 3x mai rapid cu video.
- **SOP testing**: primele 3 livrări cu SOP nou → team member execută cu fondatorul în umbră. Notează blocajele. Fix SOP after each. La livrarea #4: team member execută autonom, fondator doar QA.

### Pas 5: Construiește pagina de vânzare și intake flow automatizat
- **Landing page dedicat per produs** (nu ascuns într-un meniu cu 20 servicii):
  - **Headline**: beneficiul principal, nu feature-ul. "Find Out Exactly What's Holding Your Website Back" (not "SEO Technical Audit")
  - **Sub-headline**: mechanism + credibility. "50-point audit by certified SEO specialists. Delivered in 5 business days."
  - **What you get**: bullet list cu deliverables exacte (din Product Definition)
  - **What you DON'T get**: 2-3 excluderi cheie (previne cereri nepotrivite)
  - **Timeline**: "Results in [X] business days"
  - **Price**: afișat clar (nu "request a quote" — productized = transparent pricing)
  - **Social proof**: 2-3 testimoniale specifice ACESTUI produs (nu generic agency testimonials). Cu cifre: "The audit revealed 23 critical issues we didn't know about. Within 60 days of fixing them, organic traffic increased 34%."
  - **Before/After**: screenshot sau metric showing typical improvement
  - **Process**: 3-4 steps vizuale: "1. Purchase → 2. Share Access → 3. We Audit → 4. You Get Results"
  - **FAQ**: 5-8 frequent questions (obiecții comune convertite în răspunsuri):
    - "What if I'm not satisfied?" → "We offer [money-back guarantee / revision round]"
    - "What happens after the audit?" → "We provide a prioritized action plan. If you want us to implement the fixes, we offer retainer packages starting at $X/mo."
    - "How is this different from free audits?" → "Free tools check 5-10 factors. We check 50+ with human expert analysis, competitor benchmarking, and a personalized strategy call."
  - **CTA**: button "Buy Now" or "Get Your Audit" → Stripe Checkout or Calendly + payment
- **Intake form** (auto-sent after purchase):
  - Standardized per product (Typeform or Google Form)
  - Collect: business URL, access credentials (secure method), 3 competitors, primary goals, contact person
  - Deadline: "Please complete within 48 hours. Your [X]-day delivery timeline starts when we receive your completed form."

### Pas 6: Construiește referral și upsell engine din produse
- **Product → Retainer upsell pathway**:
  - SEO Audit → SEO Retainer ($2,500-$8,000/mo): "We found 23 issues. Want us to fix them all and keep your SEO growing? Here's our retainer package."
  - Google Ads Audit → PPC Management ($2,000-$5,000/mo): "We identified $3,200/mo in wasted ad spend. Let us manage it and save you money while increasing conversions."
  - Landing Page → Full Web Design + CRO ($5,000-$25,000 project): "Your landing page is converting at 4.2%. Imagine that across your entire site."
  - **Conversion rate benchmark**: 20-30% of productized service buyers convert to retainer within 60 days
- **Upsell timing**: mention retainer possibility at delivery call, but don't hard pitch. "This is what we'd do next if you wanted ongoing support. No pressure — the audit stands on its own." Follow up in 14 days: "Have you had a chance to implement any of the recommendations? If you'd like help, here's what our ongoing service looks like."
- **Bundle offers**: "Get SEO Audit + Google Ads Audit + Speed Optimization for $[bundle price] (save 15%)." Bundles increase average order value by 30-40%.
- **Referral program for productized**: "Refer a colleague → they get 10% off, you get $100 credit toward your next service." Simple, trackable, effective.

### Pas 7: Lansează, colectează date, și iterează sistematic
- **Soft launch**: primele 5 vânzări = validation phase. Track obsessively:
  - Actual time vs estimated time per delivery
  - Client questions during and after delivery (reveals scope gaps)
  - Feedback scores (1-10 satisfaction)
  - Upsell conversion rate
  - Refund requests (target: <5%)
- **At 5 deliveries — First iteration**:
  - Adjust time estimates based on actuals (usually increase by 10-20%)
  - Update SOP with fixes for recurring issues
  - Update FAQ with questions from first 5 clients
  - Recalculate pricing if margin is off target
- **At 15 deliveries — Major iteration**:
  - Review all feedback: identify patterns
  - Consider: should you add a Premium tier with additional deliverables?
  - Create case study from best result
  - Build automated follow-up sequence for upsell
- **At 30+ deliveries — Scale**:
  - Hire/train dedicated team member for this product
  - Founder fully removed from delivery
  - A/B test landing page (headline, price, guarantee)
  - Expand to adjacent niches (same product, different vertical)

### Pas 8: Scale cu multiple produse și product catalog
- **Product catalog strategy**:
  - Start: 1 product, perfect it (3-6 months)
  - Expand: add 2nd product (complementary — cross-sell natural). Ex: if first product is SEO Audit, second is Speed Optimization or Content Strategy
  - Scale: 3-5 products covering different stages of client journey (audit → setup → ongoing → optimization)
  - Max: 5-7 productized services. Beyond that, you're back to complexity.
- **Product ladder** (ascending price = ascending commitment):
  1. Lead magnet ($0): free checklist or mini-audit → captures email
  2. Entry product ($250-$750): quick win, low risk → demonstrates competence
  3. Core product ($1,000-$3,000): main value delivery → builds trust
  4. Premium product ($3,000-$10,000): comprehensive → positions as expert
  5. Retainer ($2,000-$15,000/mo): ongoing relationship → maximum LTV
- Each step sells the next. This is how you build a scalable revenue engine without custom proposals for every deal.

## Verificare
- [ ] Servicii candidate evaluate cu scoring pe 5 criterii (min 1 cu scor ≥18)
- [ ] Product Definition Document complet pentru primul produs
- [ ] Deliverables, inclusions, exclusions, și revision policy documentate explicit
- [ ] Preț fix calculat pe baza worst-case COGS + 55-65% margin
- [ ] SOP intern complet (pre-work, execution, QA, delivery, post-delivery) cu video walkthrough
- [ ] Pagina de vânzare publicată cu headline, deliverables, price, FAQ, CTA
- [ ] Intake form automatizat (triggered la purchase)
- [ ] Payment flow configurat (Stripe Checkout sau equivalent)
- [ ] Upsell pathway documented (product → retainer conversion plan)
- [ ] First 5 deliveries tracked cu time, feedback, și margin actuals
- [ ] SOP iterated based on first 5 deliveries

## Instrumente
- ClickUp ($7/user/mo) / Notion (free-$10/mo) — SOP-uri, project management per livrare, product tracker
- Webflow ($16/mo) / WordPress — landing page per produs (SEO-optimized)
- Stripe Checkout ($0 + 2.9% per transaction) — payment instant pentru fix-price products
- Typeform ($25/mo) / Google Forms (free) — intake form standardizat
- Loom ($15/mo) — video walkthrough al deliverable-ului la livrare + SOP training
- Screaming Frog (free-$259/yr) / Ahrefs ($99/mo) / SEMrush ($130/mo) — tools per SEO audit product
- Google Slides / Canva — report templates
- Harvest ($12/user/mo) — time tracking per product delivery (validate estimates)

## Note
- **Productizarea nu înseamnă mai puțină calitate** — înseamnă calitate CONSISTENTĂ și eficientă. Clientul 50 primește aceeași calitate ca clientul 1.
- Primul produs va dura 3-5x mai mult să îl construiești decât crezi. Al doilea durează 2x. Al treilea durează 1x. Investiția inițială e mare, dar compounding-ul e masiv.
- **"Nișa" produsului contează enorm**: "SEO Audit for Dental Practices" se vinde mai bine decât "SEO Audit" generic. Specificity builds trust și justifică preț premium (+20-30%).
- Productized services sunt excelente ca **entry-point** (foot in the door) pentru clienți noi → upsell natural spre retainer. 20-30% conversion rate from product to retainer.
- **Nu productiza un serviciu pe care nu l-ai livrat custom de minimum 5 ori** — altfel optimizezi un proces pe care nu-l înțelegi complet.
- **Death trap — Customization requests**: "Can you add just this one thing?" Răspuns: "The productized package includes X. For custom requirements, we offer bespoke engagements starting at $[higher price]." Niciodată nu customizezi productized la preț productized.
- **Benchmark**: agenții cu 30%+ revenue din productized services au EBITDA margin cu 8-12 puncte procentuale mai mare decât cele full-custom. Productizarea = profitabilitate.
