---
id: AGO-007
title: Client Reporting Cadence and KPI Dashboard
domain: agency-operations
source: Digital Client Manager - Client Management and Consulting (Udemy)
version: 2.0
forge_score: "estimated H/3.0"
business_mapping: ["ai-agency"]
---

# Client Reporting Cadence and KPI Dashboard

## Obiectiv
Stabilește o cadență de reporting semi-automatizată cu KPI dashboard per client, rapoarte narative cu insights acționabile (nu doar numere), și un proces de livrare care durează maximum 45 minute per client per lună — crescând percepția de valoare, reducând churn-ul cu 20-30%, și creând oportunități organice de upsell prin data-driven recommendations.

## Pași

### Pas 1: Definește KPI-urile contractate per serviciu la onboarding
- Nu toți clienții urmăresc aceleași metrici — stabilește KPI-urile în Kickoff Call, documentează-le în SOW, și buildează dashboard-ul pe baza lor
- **KPI Framework per serviciu** (primary = contractual, secondary = context):
  - **SEO Primary**: organic sessions (MoM + vs baseline), keyword rankings (top 3 / top 10 / top 20 / new), organic conversions (leads/sales), backlinks acquired (quantity + quality DA). **Secondary**: CTR în Search Console, pages indexed, Core Web Vitals, bounce rate organic.
  - **PPC Primary**: conversions, cost per conversion (CPA), ROAS, click-through rate (CTR), cost per click (CPC). **Secondary**: impression share, quality score mediu, ad position, search impression share lost to budget.
  - **Social Media Primary**: engagement rate per post, reach (organic + paid), link clicks, follower growth rate. **Secondary**: best performing content type, optimal posting time, sentiment analysis.
  - **Email Marketing Primary**: open rate, CTR, conversion rate, revenue per email (e-comm), list growth rate. **Secondary**: unsubscribe rate, bounce rate, deliverability score, segment performance.
  - **Web Design/Dev Primary**: page speed scores (mobile + desktop), Core Web Vitals (LCP, FID, CLS), conversion rate post-launch, bounce rate. **Secondary**: session duration, pages per session, 404 errors.
- **Setează baseline** în prima lună de lucru (sau din datele istorice dacă clientul le are). Fără baseline, nu poți demonstra progres. Document baseline în "Client Baseline Report" — o singură pagină cu toate metricile la start + data.
- **Stabilește targets** pe 90 zile (ambitious but realistic) și pe 12 luni (aspirational). Targets sunt agreate cu clientul, nu impuse — clientul care agreează targetul e mai tolerant cu variația lunară.

### Pas 2: Stabilește cadența de reporting pe 3 niveluri
- **Weekly Pulse (opțional — pentru clienți cu ad spend >$10K/mo sau în perioadă de testare)**:
  - Format: email scurt (5-7 rânduri) sau Slack message
  - Conținut: top 3 metrici cu trend vs săptămâna anterioară + 1 action item
  - Time: 10-15 min per client cu template automatizat
  - Exemplu: "This week: 342 clicks (+12% WoW), CPA $23.50 (-8%), 14 conversions. We're testing new ad copy focused on urgency — early results positive. No action needed from you."
- **Monthly Report (standard pentru 100% din clienți)**:
  - Format: PDF branded (5-7 pagini) SAU live dashboard cu email summary
  - Conținut: full KPI review, MoM + vs baseline comparison, activities completed, insights, recommendations, plan next month
  - Trimis: primele 3 zile lucrătoare ale lunii (deadline hard — dacă nu trimite la Day 3, account manager escalade)
  - Time target: 30-45 min per client cu dashboard automatizat + 15 min narativ
- **Quarterly Business Review (QBR — obligatoriu pentru toți clienții pe retainer)**:
  - Format: call 45-60 min cu deck (Google Slides, 15-20 slides)
  - Conținut: 3-month retrospective, ROI estimation, strategy adjustment, upsell recommendations (2-3 max, data-backed)
  - Trimestrial: luna 3, 6, 9, 12 din relație
  - Booking: account manager programează cu 3 săptămâni înainte (nu last minute)
- Documentează cadența agreată în SOW/retainer agreement — devine obligație contractuală

### Pas 3: Configurează dashboard automatizat cu tooling-ul potrivit
- **Opțiunea A — Looker Studio (free, Google ecosystem)**:
  - Conectori nativi: GA4, GSC, Google Ads, BigQuery, Google Sheets
  - Conectori terți (via Supermetrics $99/mo sau Porter Metrics $15/mo): Meta Ads, TikTok Ads, LinkedIn Ads, Shopify, HubSpot, Mailchimp
  - White-label: limited (logo watermark removal nu e nativ — CSS override sau embed)
  - Best for: agenții buget-conscious, Google-centric clients
  - Setup time: 2-4 ore per client (prima dată), 30 min cu template (după)
- **Opțiunea B — AgencyAnalytics ($79/mo agency plan, ~$12/client/mo)**:
  - 80+ conectori nativi (inclusiv Meta, TikTok, LinkedIn, Yelp, Call Rail, SEMrush)
  - White-label complet (custom domain, logo, colors — clientul nu știe că e AgencyAnalytics)
  - Auto-generated monthly reports cu scheduling (trimis automat pe email la data setată)
  - Client login cu dashboard live (reduce "how's my campaign doing?" emails cu 60%)
  - Best for: agenții cu 10+ clienți, multi-platform reporting
  - Setup time: 1-2 ore per client cu template
- **Opțiunea C — DashThis ($33/dashboard/mo) sau Databox (free-$72/mo)**:
  - Alternative mid-market cu focus pe visual simplicity
  - DashThis: strong template library, good for PPC/social
  - Databox: real-time KPI tracking, mobile app, anomaly alerts
- **Template strategy**: creează 1 master dashboard template per serviciu (SEO template, PPC template, Social template). La fiecare client nou: clone template → connect data sources → customize KPIs → share. Reduce setup de la ore la minute.
- **Client access**: ÎNTOTDEAUNA oferă client read-only access la dashboard. Clienții care văd datele live pun mai puține întrebări urgente și au mai multă încredere.

### Pas 4: Construiește structura raportului lunar cu narativ obligatoriu
Format standardizat (5-7 pagini PDF sau dashboard live + email summary):

**Page 1 — Executive Summary (pentru decision makers care citesc doar asta)**
- 3-5 KPI principale cu trend vizual (green arrow up / red arrow down) + valoare actuală vs target
- 1 WIN: cel mai bun rezultat al lunii cu context ("Organic traffic up 23% MoM — driven by 3 blog posts ranking on page 1 for [keywords]")
- 1 CHALLENGE: ce nu a mers și de ce (transparent, nu defensiv) + ce faci în continuare
- 1 OPPORTUNITY: insight sau recomandare data-driven (seeds upsell natural)

**Page 2-3 — Performance Deep Dive**
- Toate metricile contractate cu comparison: actual vs target | actual vs previous month | actual vs baseline
- Grafice de trend (line charts pentru metrici de volum, bar charts pentru comparison) — vizual bate tabelul de numere
- Segmentare relevantă: by channel, by campaign, by content type, by device, by geography
- Attribution: dacă e posibil, conectează activitatea agenției cu resultatul ("Article X published on [date] generated 850 organic visits and 12 leads this month")

**Page 3-4 — Activities Completed**
- Ce s-a lucrat în luna respectivă (per serviciu), cu status per deliverable:
  | Deliverable | Status | Notes |
  |------------|--------|-------|
  | 8 blog articles | ✅ Delivered | 2 ranking page 1 already |
  | 6 backlinks | ✅ Delivered | Avg DA 35 |
  | Technical audit fixes | ✅ 12/15 fixed | 3 require client dev team |
  | Monthly strategy call | ✅ Completed | Recording: [link] |
- Ce era planificat vs ce s-a livrat efectiv — transparență totală

**Page 5 — Insights & Recommendations**
- 2-3 observații cheie din date (interpretat, nu evident):
  - "Mobile traffic crescut 40% dar mobile conversion rate e 0.3% vs 1.8% desktop → clear CRO opportunity on mobile"
  - "Top 3 performing blog posts all target comparison keywords → shifting content strategy to more comparison/review content"
- 2-3 recomandări concrete cu impact estimat și cost (dacă e outside scope):
  - "Recommend: Mobile CRO audit ($1,500 one-time) — estimated impact: 2-3x mobile conversion rate"
  - Nu pitch agresiv — data-driven suggestion. Clientul decide.

**Page 6 — Next Month Plan**
- Task-uri planificate cu deadlines și responsabili
- Obiective KPI estimate pentru luna următoare (ambitious but realistic)
- Anything needed from client (approvals, access, info) cu deadline

### Pas 5: Implementează procesul de livrare eficient
- **Calendar blocked**: primele 3 zile lucrătoare ale lunii = "Report Week". Account managers au calendar blocked pentru report creation.
- **Workflow per client**:
  1. Day 1-2: Pull data din dashboard (automatizat), review metrici, write narrative (30-45 min per client)
  2. Day 2-3: Internal QA — second reviewer checks numbers și narrative (15 min per report)
  3. Day 3: Send report via email cu contextualized summary
- **Email delivery template**:
  ```
  Subject: [Client Name] — [Month] Performance Report + Key Insights

  Hi [Name],

  Attached is your [Month] performance report. Here are the highlights:

  📈 WIN: [1 sentence — best result with number]
  🔍 INSIGHT: [1 sentence — most interesting finding]
  ➡️ NEXT: [1 sentence — what we're doing next month]

  Full report: [Dashboard link] | PDF attached

  Want to discuss anything? [Calendar link for 15-min slot]

  [Signature]
  ```
- **Track email opens**: Proposify/PandaDoc for PDF, or Mailtrack for email. Dacă report nu e deschis în 5 zile → follow-up call: "Wanted to make sure you saw the report. Any questions?"
- **Time benchmark**: raport complet = max 45 min muncă umană per client (cu dashboard automatizat). Dacă durează >1.5 ore, automatizează mai mult sau simplifică template-ul.

### Pas 6: Construiește QBR (Quarterly Business Review) ca instrument strategic
- **Pre-QBR prep** (1-2 ore per client):
  - Compile 3-month data summary cu trends
  - Calculate ROI estimation: "Your investment of $[X] over 3 months generated approximately $[Y] in [leads/revenue/traffic value]"
  - Identify 2-3 upsell opportunities backed by data
  - Prepare "next quarter strategy" recommendations
- **QBR deck structure** (Google Slides, 15-20 slides, branded):
  1. Title + agenda (1 slide)
  2. Quarter summary — 3-5 key metrics with trend (1-2 slides)
  3. Goal review — what we promised vs what we delivered (1 slide)
  4. Monthly breakdowns (3 slides — 1 per month highlight)
  5. Wins gallery — screenshots, rankings, testimonials from their customers (1-2 slides)
  6. Challenges + lessons learned (1 slide — transparency builds trust)
  7. Competitive landscape — what competitors did, how we're positioned (1 slide)
  8. Opportunities identified — data-backed recommendations (2-3 slides)
  9. Next quarter strategy + goals (1-2 slides)
  10. Q&A + feedback (1 slide)
- **QBR is the #1 retention and upsell tool**: clientul vede valoarea livrată in bulk, e mulțumit, și ascultă recomandări. Conversion rate pe upsell propus în QBR: 30-50%.
- **QBR NPS**: la finalul fiecărui QBR, trimite 1-question NPS survey: "On a scale of 1-10, how likely are you to recommend us?" Score <7 = immediate retention intervention (AGO-014).

### Pas 7: Automatizează cât mai mult — target: 80% automatizare
- **Data collection**: 100% automatizat via dashboard connectors (Looker Studio / AgencyAnalytics / Supermetrics)
- **Report generation**: 70-80% automatizat cu scheduled reports (AgencyAnalytics) sau Looker Studio + Google Slides API
- **Narativ / insights**: 20-30% — asta rămâne uman (AI poate ajuta cu draft, dar insight-ul strategic e valoarea ta)
- **Distribution**: 90% automatizat — scheduled emails, client portal access, auto-reminders
- **Time savings benchmark**: de la 2-3 ore/raport (manual) la 30-45 min/raport (semi-automat) la 15-20 min/raport (fully templated + auto)
- La 20 clienți × 45 min/raport = 15 ore/lună pe reporting. La 20 clienți × 2 ore/raport = 40 ore/lună. Automatizarea salvează 25 ore/lună = 1 angajat part-time.

### Pas 8: Transformă reporting-ul în instrument de retenție și upsell
- **Data-driven upsell seeding**: la finalul fiecărui raport, include 1 "Opportunity Identified" secțiune:
  - "GSC shows 40% of your traffic is mobile but mobile conversion rate is 0.3% vs 1.8% desktop. We recommend a Mobile CRO Audit ($1,500 one-time) with estimated 2-3x improvement."
  - "Your Google Ads search campaigns are generating leads at $32 CPA. We estimate Meta Ads retargeting could capture an additional 20-30 leads/month at $18-$22 CPA. Would you like a proposal?"
- Această abordare face upsell-ul organic, consultativ și data-driven — nu pitch agresiv
- **Track upsell conversion from reports**: câte oportunități identificate în rapoarte au convertit? Target: 10-15% conversion pe oportunități menționate.
- **Client satisfaction correlation**: clienții care primesc rapoarte consistent au NPS cu 15-20 puncte mai mare decât cei care nu primesc. Reporting = retention.

## Verificare
- [ ] KPI-uri per serviciu definite la onboarding și documentate în SOW
- [ ] Baseline stabilit și documentat în prima lună
- [ ] Cadența de reporting agreată contractual (weekly/monthly/quarterly)
- [ ] Dashboard Looker Studio sau AgencyAnalytics configurat per client cu data live
- [ ] Template raport lunar cu toate secțiunile creat și QA'd
- [ ] QBR deck template creat cu toate 10 secțiunile
- [ ] Report delivery process documentat cu calendar blocked pentru Report Week
- [ ] Livrarea raportului durează max 45 min per client (track time)
- [ ] Email follow-up dacă raportul nu e deschis în 5 zile
- [ ] Upsell opportunities tracked din rapoarte cu conversion rate

## Instrumente
- Looker Studio (free) — dashboard automatizat, Google ecosystem
- AgencyAnalytics ($79/mo + $12/client/mo) — white-label dashboard + auto-reports
- Supermetrics ($99/mo) / Porter Metrics ($15/mo) — conectori pentru Looker Studio
- GA4 / Google Search Console / Google Ads — surse date SEO/PPC
- Meta Business Suite / TikTok Ads Manager — surse date social
- Google Slides — QBR decks cu branded templates
- Mailtrack (free) / HubSpot — tracking deschidere email raport
- Harvest / Toggl Track — tracking timp petrecut pe reporting per client

## Note
- **Rapoartele care arată doar numere fără interpretare nu construiesc valoare** — clientul plătește pentru insight, nu pentru un printscreen din GA4. Dacă raportul tău poate fi generat de un script fără input uman, nu justifici fee-ul.
- Un dashboard live reduce anxietatea clientului și numărul de "how's my campaign doing?" emails cu 50-60%
- Dacă o lună a fost slabă: nu ascunde datele, nu minimiza. Explică cauzele, prezintă planul de remediere, și arată ce ai învățat. Transparența construiește încredere mai mult decât rezultatele bune.
- **Death trap — Over-reporting**: dacă petreci >1.5 ore per raport per client, pierzi bani. Automatizează, templateizează, și focus pe narativ nu pe data pulling.
- QBR-ul este investiția ta cea mai profitabilă în retenție și upsell. Un QBR de 60 minute bine pregătit poate genera $2,000-$5,000/mo upsell cu 30-50% conversion rate.
- **Benchmark**: agenții top au <3% churn pe clienții care primesc QBR vs 8-12% pe cei care nu primesc. QBR = churn insurance.
