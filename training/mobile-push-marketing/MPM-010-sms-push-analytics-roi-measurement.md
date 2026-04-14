---
id: MPM-010
title: SMS/Push Campaign Analytics and ROI Measurement
domain: mobile-push-marketing
source: SMS Marketing A-Z (Udemy) + Automate WhatsApp & SMS Marketing (SMSrush) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# SMS/Push Campaign Analytics and ROI Measurement

## Obiectiv
Configurează un sistem complet de tracking și analytics pentru campaniile SMS, push și WhatsApp, măsoară ROI-ul real per campanie, per GEO și per canal, și folosește datele pentru optimizare continuă — de la UTM setup și postback configuration la dashboard executiv și attribution modeling, cu benchmarks specifice betting/casino pe piețele SMSads (Bolivia, Peru, Kenya, Chile).

## Pași

### Pas 1: Infrastructura de tracking — setup complet
1. **UTM parameters** (obligatoriu pe FIECARE link din mesaje):
   ```
   https://smsads.com/bonus?utm_source=sms&utm_medium=bulk&utm_campaign=welcome-bonus-pe-mar26&utm_content=variant-a&utm_term=fresh-leads
   ```
   - `utm_source`: `sms` / `push` / `whatsapp` / `push_adnetwork`
   - `utm_medium`: `bulk` / `drip` / `transactional` / `cpc` / `cpm`
   - `utm_campaign`: `{obiectiv}-{geo}-{luna}` (ex: `welcome-bonus-pe-mar26`)
   - `utm_content`: `variant-a` / `variant-b` (A/B test identification)
   - `utm_term`: `{segment}` (ex: `fresh-leads`, `dormant-30d`, `vip`)
2. **URL shortener cu tracking** (nu folosi URL lung în SMS):
   - **Rebrandly** (rebrandly.com): branded URLs, tracking click-uri + device + GEO. $13/mo
   - **Bitly** (bitly.com): tracking basic, gratuit până la 1,000 links/mo
   - **ClickMagick** (clickmagick.com): tracking avansat, rotație, sub-id tracking, bot filtering. $69/mo. RECOMANDAT pentru affiliate
   - **Tracker propriu** (Voluum/BeMob/RedTrack): dacă rulezi push ads, tracker-ul are short link built-in
   - **Platform built-in**: SMSrush, TextMagic au link tracking nativ (basic)
3. **Promo code tracking** (cea mai precisă atribuire):
   - Generează cod UNIC per contact: `PERU-CAR-7842`
   - La conversie: user introduce codul → match 1:1 cu contactul → atribuire perfectă
   - Funcționează chiar dacă user copiază link-ul manual sau accesează de pe alt device
4. **Pixel de conversie pe site/landing page**:
   - **Google Analytics 4**: configurează conversion events (sign_up, deposit, purchase)
   - **Facebook Pixel**: dacă rulezi click-to-WhatsApp ads
   - **Postback URL** (server-to-server): cel mai precis pentru affiliate
     - Flow: user convertește → platforma de betting trimite postback → tracker-ul înregistrează
     - Format: `https://tracker.voluum.com/postback?clickid={clickid}&payout={payout}&txid={transaction_id}`
5. **Link unic per campanie**: NU reutiliza același short URL — creează link NOU per campanie

### Pas 2: Definirea KPI-urilor — metrici care contează per canal
1. **SMS Marketing KPIs** (betting/casino):
   | Metric | Formula | Benchmark bun | Red flag |
   |---|---|---|---|
   | Delivery Rate | Delivered / Sent × 100 | >95% | <90% |
   | CTR | Clicks / Delivered × 100 | 8-15% (betting) | <3% |
   | Registration Rate | Registrations / Clicks × 100 | 15-25% | <10% |
   | FTD Rate | Deposits / Registrations × 100 | 20-30% | <15% |
   | Opt-out Rate | STOP / Delivered × 100 | <1% | >2% |
   | Cost per Click | Total Cost / Clicks | $0.30-0.80 | >$1.50 |
   | Cost per Registration | Total Cost / Registrations | $1-3 | >$5 |
   | Cost per FTD | Total Cost / FTDs | $5-20 | >$30 |
   | ROI | (Revenue - Cost) / Cost × 100 | >200% | <50% |
   | Revenue per SMS | Revenue / Total Sent | >$0.10 | <$0.02 |

2. **Push Notification KPIs** (ad-network):
   | Metric | Ad-Network | Lista proprie |
   |---|---|---|
   | CTR | 0.1-0.5% (bun), >1% (excelent) | 5-12% |
   | Conversion Rate | 2-8% din clicks | 10-25% din clicks |
   | CPC efectiv | $0.003-0.05 | N/A (owned) |
   | CPA (per FTD) | $10-40 | $3-15 |
   | Subscriber churn | N/A | <2%/campanie |

3. **WhatsApp KPIs**:
   | Metric | Benchmark bun | Notes |
   |---|---|---|
   | Delivery Rate | >95% | 2 checks |
   | Read Rate | >70% | Blue checks |
   | Reply Rate | >5-10% | Engagement signal |
   | Link CTR | 15-40% | Cel mai mare CTR din orice canal |
   | Opt-out (STOP) Rate | <1% | Meta monitorizează |
   | Block Rate | <2% | >5% = quality score drop |
   | Cost per conversation | $0.03-0.08 | Meta pricing per GEO |

4. **Cross-channel comparison** (pentru budget allocation):
   | Metric | SMS | Push (ad-net) | Push (owned) | WhatsApp |
   |---|---|---|---|---|
   | Reach | 99% mobile | 60-70% Android | 100% subscribers | 67-94% per GEO |
   | Open/View Rate | ~98% | N/A | 60-80% | 80-95% |
   | CTR | 8-15% | 0.1-0.5% | 5-12% | 15-40% |
   | Cost per msg | $0.01-0.05 | $0.003-0.05 | $0 (owned) | $0.03-0.08 |
   | Cost per FTD | $5-20 | $10-40 | $3-15 | $5-20 |
   | Compliance risk | Medium | Low | Low | High (Meta ban) |

### Pas 3: Setup raportare per platformă
1. **SMS Platforms** (SMSrush / TextMagic / Twilio):
   - Post-campanie: Dashboard → Campaigns → click campanie → Export CSV
   - CSV conține: phone, delivery status, timestamp, click (dacă link tracking activ)
   - Twilio: Messaging Insights dashboard + API export (programatic)
2. **Push Ad-Networks** (PropellerAds / Evadav / RichAds):
   - Dashboard → Statistics → filtrează per campanie
   - Metrici: impressions, clicks, CTR, spend, conversions (dacă postback activ)
   - Export CSV per campaign + per zone (pentru whitelist/blacklist analysis)
3. **OneSignal** (push owned):
   - Dashboard → Delivery → selectează campanie
   - Metrici: Sent, Delivered, Clicked (CTR), Influenced Opens
   - Segment analytics: performanță per tag/segment
4. **WhatsApp** (360dialog / WATI):
   - Conversation analytics: sent, delivered, read, replied
   - Template performance: CTR per template
   - Quality Score trend (Green/Yellow/Red)
5. **Google Analytics 4**:
   - Reports → Acquisition → Traffic Acquisition → filtrează utm_source = sms/push/whatsapp
   - Metrici: sessions, conversions, revenue per campaign
   - Conversion paths: multi-touch attribution (vezi Pas 5)
6. **Tracker extern** (Voluum / BeMob / RedTrack):
   - Real-time dashboard: clicks, conversions, revenue, ROI per campaign/geo/creative
   - Drill-down: per zone (ad-network), per device, per OS, per hour
   - RECOMANDAT ca single source of truth pentru affiliate campaigns

### Pas 4: Calculul ROI complet — per campanie, per GEO, per canal
1. **Formula ROI standard**:
   ```
   ROI = (Revenue - Total Cost) / Total Cost × 100
   ```
2. **Calculul costului total** (nu doar costul SMS-ului):
   ```
   Total Cost = Direct Costs + Indirect Costs

   Direct Costs:
   - SMS cost: nr_sms × cost_per_sms (ex: 10,000 × $0.03 = $300)
   - WhatsApp cost: nr_conversations × cost_per_conv (ex: 5,000 × $0.05 = $250)
   - Push ad spend: total CPC/CPM spend (ex: $200)
   - Link shortener: monthly fee allocated (ex: $13/mo ÷ 4 campaigns = $3.25)

   Indirect Costs:
   - Labor: hours × rate (ex: 3h × $25/h = $75)
   - Platform fees: monthly subscription allocated (ex: $50/mo ÷ 4 = $12.50)
   - Creative design: if outsourced (ex: $50)
   - HLR lookups: nr_lookups × $0.005 (ex: 10,000 × $0.005 = $50)

   Example Total: $300 + $250 + $200 + $3.25 + $75 + $12.50 + $50 + $50 = $940.75
   ```
3. **Atribuirea veniturilor** (metode, de la simplu la complex):
   - **Last click (direct)**: revenue din sesiunile cu utm_source=sms/push/wa în aceeași zi. Simplu dar inaccurat
   - **Promo code tracking**: numeri exact câte coduri unice au fost folosite → PERFECT attribution
   - **Multi-touch attribution**: GA4 model-based → vede toate touchpoint-urile (SMS + push + email)
   - **Time-window attribution**: orice conversie în 7 zile de la click pe SMS = atribuită campaniei
4. **Exemplu ROI complet (campanie SMS betting Peru)**:
   ```
   Campanie: Welcome Bonus Peru Martie 2026
   Lista: 15,000 contacte Peru (segment: fresh leads)
   Cost SMS: 15,000 × $0.04 = $600
   HLR lookup: 15,000 × $0.005 = $75
   Labor: 3h × $25 = $75
   TOTAL COST: $750

   Delivery rate: 96% → 14,400 delivered
   CTR: 12% → 1,728 clicks
   Registration rate: 22% → 380 registrations
   FTD rate: 25% → 95 FTDs
   Average deposit: $30
   Operator revenue share: 35% of losses (CPA model: $25 per FTD)
   REVENUE: 95 × $25 = $2,375

   ROI: ($2,375 - $750) / $750 × 100 = 216.7%
   Revenue per SMS: $2,375 / 15,000 = $0.158/SMS
   Cost per FTD: $750 / 95 = $7.89
   ```
5. **ROI benchmarks per GEO (SMS betting/casino)**:
   | GEO | Avg Cost/SMS | Avg CTR | Avg Cost/FTD | Avg ROI |
   |---|---|---|---|---|
   | Bolivia | $0.02-0.04 | 8-12% | $5-12 | 150-400% |
   | Peru | $0.03-0.05 | 10-15% | $8-18 | 120-300% |
   | Kenya | $0.01-0.02 | 12-18% | $3-8 | 200-600% |
   | Chile | $0.03-0.05 | 8-12% | $10-22 | 100-250% |

### Pas 5: Attribution modeling — dincolo de last click
1. **Multi-channel attribution** (necesar când folosești SMS + push + WhatsApp simultan):
   - User vede push ad (PropellerAds) → click → nu converteste
   - Primește SMS 2 zile mai târziu → click → se înregistrează → nu depune
   - Primește WhatsApp drip la 24h → click → depune
   - **Last click**: atribuie 100% WhatsApp. **Realitate**: toate 3 canalele au contribuit
2. **Modele de atribuire**:
   - **Last click** (simplu, default): 100% credit ultimului touchpoint. Supraestimează closing channels (WhatsApp, email)
   - **First click**: 100% credit primului touchpoint. Supraestimează awareness channels (push ads, social)
   - **Linear**: credit egal pe toate touchpoint-urile (33% push + 33% SMS + 33% WhatsApp)
   - **Time decay**: mai mult credit touchpoint-urilor mai recente (10% push + 30% SMS + 60% WhatsApp)
   - **Data-driven** (GA4): ML model, cel mai precis dar necesită volum (min 300 conversii/30 zile)
3. **Setup GA4 attribution**:
   - Admin → Attribution Settings → selectează modelul dorit
   - Conversion paths: Reports → Advertising → Attribution → Conversion paths
   - Vei vedea exact cum contribuie fiecare canal la conversii
4. **Recomandare practică SMSads**:
   - La start (volum mic): Last click cu promo code tracking (simplu + precis)
   - La scale (>500 conversii/lună): GA4 data-driven attribution
   - MEREU: promo code tracking ca ground truth independent

### Pas 6: Dashboard master — Google Sheets / Looker Studio
1. **Master tracker campanii (Google Sheets)**:
   ```
   Sheet: "Campaign Tracker"
   Columns:
   A: Data | B: GEO | C: Canal (SMS/Push/WA) | D: Segment | E: Nr Contacte |
   F: Delivered | G: Delivery% | H: Clicks | I: CTR% | J: Registrations |
   K: Reg Rate% | L: FTDs | M: FTD Rate% | N: Cost Total | O: Revenue |
   P: ROI% | Q: Cost/Click | R: Cost/FTD | S: Rev/SMS | T: Opt-out% |
   U: Ora Trimitere | V: Creative Winner | W: Notes
   ```
2. **Formule automate**:
   ```
   Delivery% = F/E*100
   CTR% = H/F*100
   Reg Rate% = J/H*100
   FTD Rate% = L/J*100
   ROI% = (O-N)/N*100
   Cost/Click = N/H
   Cost/FTD = N/L
   Rev/SMS = O/E
   ```
3. **Grafice automate** (pe Sheet separate):
   - **Line chart**: ROI% per campanie (trend în timp) — ești pe trend ascendent?
   - **Bar chart**: Cost per FTD per GEO — care GEO e cel mai eficient?
   - **Stacked bar**: Revenue per canal (SMS vs Push vs WA) — unde investești?
   - **Heatmap**: CTR per zi × oră (din MPM-009) — când trimiți?
   - **Pie chart**: distribuția bugetului per GEO și per canal
4. **Google Looker Studio** (gratuit, vizualizare premium):
   - Connect Google Sheets ca data source
   - Connect GA4 ca data source
   - Dashboard interactive: filtrare per GEO, per canal, per perioadă
   - Auto-refresh: datele se actualizează automat din Sheets
5. **Dashboard update cadence**:
   - După fiecare campanie: adaugă rândul în tracker (5 min)
   - Săptămânal: review grafice, identifică trend-uri (15 min)
   - Lunar: raport complet cu insights (30 min)

### Pas 7: A/B testing analytics — framework riguros
1. **Setup A/B test SMS/Push**:
   - **Ipoteză** (scrie-o ÎNAINTE de test): "Mesajul cu urgență va genera CTR mai mare decât mesajul cu beneficiu"
   - **Split**: random 50/50 din aceeași listă (SAME segment, SAME ora, SAME GEO)
   - **Metrică primară**: CTR (mai ușor de măsurat cu volum mic)
   - **Metrică secundară**: conversion rate, opt-out rate (verificare că CTR mare nu vine cu opt-out mare)
   - **Control**: varianta curentă (champion). Challanger: varianta nouă
2. **Dimensiunea minimă a eșantionului**:
   - Pentru detectarea diferenței de 20% relativ (ex: 10% CTR vs 12% CTR):
     - Min 1,000 contacte per variantă (total 2,000)
   - Pentru detectarea diferenței de 10% relativ (ex: 10% vs 11%):
     - Min 4,000 contacte per variantă (total 8,000)
   - Tool: Evan Miller's sample size calculator (evanmiller.org/ab-testing/sample-size.html)
3. **Semnificativitate statistică**:
   - p-value < 0.05 = diferența e statistic semnificativă (95% confident)
   - Tool: VWO significance calculator, ABTestGuide calculator
   - NU declara câștigător dacă p-value > 0.05 — s-ar putea să fie noise
4. **A/B test log** (documentare obligatorie):
   | Test ID | Data | Element | Var A | Var B | CTR A | CTR B | p-value | Winner | Notes |
   |---|---|---|---|---|---|---|---|---|---|
   | T-001 | 2026-03-20 | Hook | Urgency | Benefit | 12.1% | 9.8% | 0.003 | A | Urgency +23% lift |
   | T-002 | 2026-03-22 | CTA | "Registrate" | "Juega ahora" | 11.5% | 13.2% | 0.018 | B | Action verb wins |
   | T-003 | 2026-03-25 | Emoji | Cu emoji | Fara emoji | 10.8% | 10.2% | 0.34 | - | No sig. difference |
5. **Compounding optimization**: câștigătorul Round 1 = baza pentru Round 2
   - Round 1: titlu urgency câștigă → Round 2: testează CTA pe mesajul cu urgency
   - După 5-10 runde: mesajul e optimizat cu 50-100% peste original

### Pas 8: Raportare lunară și trimestrială — format executiv
1. **Raport lunar** (pentru management / client):
   ```
   === SMS/Push/WA Marketing Report — Martie 2026 ===

   EXECUTIVE SUMMARY:
   - Total campanii: 12 (8 SMS, 3 Push, 1 WhatsApp)
   - Total contacte atinse: 85,000
   - Total revenue atribuit: $8,450
   - Total cost: $2,890
   - Overall ROI: 192%
   - Cost per FTD mediu: $12.40

   PER GEO:
   | GEO | Campanii | Contacte | FTDs | Cost | Revenue | ROI |
   |---|---|---|---|---|---|---|
   | Peru | 4 | 35,000 | 85 | $1,400 | $3,400 | 143% |
   | Kenya | 3 | 25,000 | 92 | $500 | $3,200 | 540% |
   | Bolivia | 3 | 15,000 | 38 | $600 | $1,250 | 108% |
   | Chile | 2 | 10,000 | 22 | $390 | $600 | 54% |

   PER CANAL:
   | Canal | Cost/FTD | CTR | ROI |
   |---|---|---|---|
   | SMS | $11.20 | 11.8% | 210% |
   | Push (ad-net) | $18.50 | 0.35% | 85% |
   | WhatsApp | $8.90 | 28% | 340% |

   TOP 3 CAMPANII (best ROI):
   1. Kenya Welcome SMS — ROI 620%
   2. Peru Sport Event WA — ROI 380%
   3. Bolivia Reactivare SMS — ROI 290%

   WORST 3 (lecții):
   1. Chile Casino Push — ROI 12% (creative fatigue, audience mismatch)
   2. Peru Broadcast SMS — ROI 45% (unsegmented, low CTR)
   3. Bolivia Cold list SMS — ROI -15% (dirty list, 18% delivery fail)

   KEY LEARNINGS:
   - Kenya = cel mai profitabil GEO (cost mic, CTR mare)
   - WhatsApp outperforms SMS pe ROI dar volume limitate (Meta tiers)
   - Segmented campaigns: 2.3x ROI vs unsegmented
   - Ora optimă confirmată: 10-12 și 18-20 local time

   PLAN LUNA VIITOARE:
   - Scale Kenya +50% volume
   - Test WhatsApp drip sequence Peru
   - A/B test: video WA vs image WA
   - HLR cleanup Bolivia list
   ```

2. **Raport trimestrial** (strategic):
   - Trend ROI pe 3 luni: îmbunătățim? stagnăm? declinăm?
   - Comparație cross-channel: SMS vs Push vs WA — unde investim mai mult?
   - Customer Lifetime Value per canal: care canal aduce cei mai valoroși useri?
   - Budget reallocation recommendations
   - Competitive landscape: ce fac competitorii? (insights din spy tools)
   - Regulatory updates per GEO (MINCETUR, BCLB changes)

3. **Format deliverabil**: HTML report (publish_html) sau Google Sheets cu grafice auto-update

### Pas 9: Advanced analytics — cohort analysis și LTV
1. **Cohort analysis** (cel mai valoros report pe termen lung):
   - Grupează useri per luna de achiziție (cohorta Martie 2026, cohorta Aprilie 2026...)
   - Trackează per cohortă: retenție, deposite cumulate, revenue cumulate pe 3/6/12 luni
   - Exemplu:
     ```
     Cohorta Mar 2026 (via SMS):
     Luna 1: 95 FTDs, $2,375 revenue
     Luna 2: 42 active (44% retenție), $1,050 revenue
     Luna 3: 28 active (29%), $700 revenue
     Total 3-month LTV: $43.42 per FTD ($4,125 / 95)
     ```
   - Compară LTV per canal: SMS users vs Push users vs WhatsApp users
   - Dacă WhatsApp users au LTV 2x mai mare → reallocate budget towards WhatsApp
2. **ROAS (Return on Ad Spend)** per canal:
   ```
   ROAS = Revenue / Ad Spend
   SMS ROAS: $8,450 / $2,890 = 2.92x (pentru fiecare $1 cheltuit, câștigăm $2.92)
   Target ROAS betting: >2x (break even ~1.0x)
   ```
3. **Predictive analytics** (avansat):
   - Pe baza datelor de 3+ luni: predict ROI pentru campanii viitoare
   - Linear regression simplu: ROI = f(GEO, segment, ora, canal, creative_type)
   - Tool: Google Sheets FORECAST function sau Python scikit-learn
4. **Fraud detection** (pentru push ad-networks):
   - Click fraud indicators: CTR anormal de mare (>5% pe ad-network), 0 conversii la 1000+ clicks
   - Bot traffic: clicks din aceleași zone repetitiv, time-on-site <2 secunde
   - Acțiune: blacklistează zonele suspecte, reportează la ad-network
   - PropellerAds/RichAds au anti-fraud built-in dar nu e perfect — verifică manual

### Pas 10: Optimization framework — de la date la acțiuni
1. **Weekly optimization cycle**:
   - **Luni**: review weekend campaigns performance
   - **Marți**: A/B test results analysis, document winners
   - **Miercuri**: creative refresh + new test setup
   - **Joi**: campaign launch cu optimizări aplicate
   - **Vineri**: weekly KPI review, prep weekend campaigns
2. **Monthly strategic review**:
   - Budget reallocation: move $ from low-ROI to high-ROI channels/GEOs
   - List hygiene: HLR cleanup, segment refresh
   - Competitor analysis: spy tools check (Anstrex for push)
   - New GEO evaluation: should we expand to new markets?
3. **Decision matrix** (ce faci cu datele):
   | Situation | Action |
   |---|---|
   | ROI >200% | Scale: crește buget +30-50%, expand GEO |
   | ROI 100-200% | Optimize: A/B test, segment better, clean list |
   | ROI 50-100% | Fix: schimbă oferta, creative refresh, try different GEO |
   | ROI <50% | Kill or Pivot: opreste, analizează root cause, pivot |
   | CTR >15% but low conversion | Landing page issue: testează alt LP |
   | High delivery but low CTR | Creative issue: refresh, test new angles |
   | Low delivery (<90%) | Infrastructure issue: HLR, route quality, sender ID |

## Verificare
- [ ] UTM parameters configurate pe TOATE link-urile (source, medium, campaign, content, term)
- [ ] URL shortener cu tracking configurat (Rebrandly / ClickMagick / platform built-in)
- [ ] Promo code unic per contact generat (perfect attribution)
- [ ] Pixel conversie / GA4 events configurate și verificate (test conversion PASS)
- [ ] Postback URL configurat dacă rulezi push ads (tracker ↔ ad-network)
- [ ] KPIs definite per canal cu benchmarks și red flags (tabel completat)
- [ ] Master tracker Google Sheets creat cu formule automate
- [ ] Grafice automate: ROI trend, cost/FTD per GEO, CTR per canal
- [ ] A/B test framework: ipoteză + eșantion minim + semnificativitate + log
- [ ] Raport lunar template creat (format executive summary)
- [ ] Cross-channel comparison: SMS vs Push vs WA
- [ ] Attribution model ales și configurat (last click + promo code la start)

## Instrumente
- **Google Analytics 4**: analytics.google.com — UTM tracking, attribution modeling, conversion paths
- **Rebrandly**: rebrandly.com — branded URL shortener cu tracking ($13/mo)
- **ClickMagick**: clickmagick.com — advanced tracking, rotație, bot filtering ($69/mo)
- **Voluum**: voluum.com — tracker #1 pentru affiliate ($199/mo)
- **BeMob**: bemob.com — tracker gratuit pentru start
- **RedTrack**: redtrack.io — tracker cu team features ($149/mo)
- **Bitly**: bitly.com — URL shortener basic (gratuit)
- **Google Sheets**: campaign tracker + formule + grafice
- **Google Looker Studio**: datastudio.google.com — dashboard vizual gratuit conectat la Sheets + GA4
- **VWO Significance Calculator**: vwo.com/tools/ab-split-test-significance-calculator
- **Evan Miller Sample Size Calculator**: evanmiller.org/ab-testing/sample-size.html
- **Anstrex**: anstrex.com — spy tool push ads ($89/mo)

## Note
- SMS = cel mai ușor canal de atribuit (promo code unic = 1:1 attribution)
- Push notification ROI pe ad-network: folosește postback + tracker. Fără tracker, ești orb
- WhatsApp read receipts (blue checks) = avantaj enorm vs SMS (unde open rate e estimat, nu măsurat)
- Kenya = cel mai eficient GEO din portofoliul SMSads (cost mic + CTR mare = ROI 200-600%)
- ROI de 200%+ pe SMS betting e NORMAL pentru liste de calitate — dacă ești sub 100% ceva e fundamental greșit
- Cross-channel insight tipic: WhatsApp aduce cei mai buni useri (LTV mai mare) dar SMS are reach-ul cel mai mare
- Documentarea FIECĂREI campanii = investiție. În 6 luni ai un playbook propriu care bate orice curs sau benchmark generic
- Greșeala #1: nu trackezi conversiile (arunci bani fără să știi ce funcționează)
- Greșeala #2: optimizezi pentru CTR în loc de ROI (CTR mare cu 0 conversii = bani pierduți)
- Greșeala #3: nu faci A/B testing ("merge bine, de ce să schimb?") — MEREU testezi, MEREU optimizezi
- Greșeala #4: ignori unit economics (cost/FTD > payout/FTD = pierzi bani la fiecare conversie)
