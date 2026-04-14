---
id: SEO-109
title: Client SEO Reporting Template
domain: seo
source: Advanced SEO 2025 — Client Communication
version: 2.0
forge_score: "estimated M/3.5"
---

# Client SEO Reporting Template

## Obiectiv
Template standardizat pentru rapoarte SEO lunare cu executive dashboard, keyword rankings, link building progress, Ahrefs/Semrush data extraction, Looker Studio integration, comunicarea findings negative și meeting agenda structurat.

## Pași

### 1. Structura raport lunar (6 pagini)
**P1 — Executive Dashboard**: KPIs vs luna anterioară (trafic organic, conversii, revenue), trend 6 luni grafic, 3 bullet points: ce am făcut / ce s-a întâmplat / ce urmează
**P2 — SEO Performance**: trafic sessions + users MoM + YoY, organic conversions + CVR, top 10 pagini (trend ↑/↓/→), visibility score
**P3 — Keywords**: mișcări semnificative (>5 poziții), noi keywords top 10, keywords pierdute + acțiune
**P4 — Link Building**: linkuri noi (domeniu, DR, anchor), DR/TF trend, linkuri pierdute
**P5 — Activitate**: lista lucrărilor completate, on-page optimizations, issues tehnice rezolvate
**P6 — Plan luna viitoare**: activități planificate cu obiective, issues known, recomandări

### 2. Data extraction Ahrefs/Semrush
**Trafic Organic (GA4)**: Reports → Acquisition → Traffic Acquisition → organic/google → compare MoM + YoY
**Rankings (Semrush)**: Position Tracking → export CSV → filter per interval poziții
**Backlinks noi (Ahrefs)**: Backlinks → filter "New" (30 days) → export DR, Anchor, Target URL
**Conversii (GA4)**: Explorations → Funnel → filter Organic source

### 3. Vizualizare și design raport
- Culori consistente: verde = creșteri, roșu = scăderi, albastru = neutru
- Grafice simple: line chart trafic, bar chart keywords distribution
- Evită tabele lungi fără vizualizare — clientul nu citește 200 rânduri
- **Highlight box**: cele mai importante 3 cifre mari, vizibile, bold
- Tools: Looker Studio (live + export PDF), Canva, Google Slides

### 4. Looker Studio dashboard (recomandat)
- Creare o dată → actualizare automată lunar din GA4 + GSC
- Trimite link live read-only clientului SAU export PDF
- Adaugă logo client + branded colors
- Avantaj: reduce întrebări ad-hoc (clientul poate verifica oricând)

### 5. Comunicarea findings negative (framework)
Nu ascunde date negative — pierderea se vede oricum:
1. **Recunoaște** situația clar
2. **Explică** cauza (Google Update? Competitori noi? Problemă tehnică?)
3. **Propune** acțiuni luate/planificate
4. **Previzionează** timeline recuperare realistă

Exemplu: "Drop 15% trafic organic, corelat cu Core Update 14 martie. Am identificat 20 pagini afectate, am început optimizarea — estimăm recuperare parțială în 60-90 zile."

### 6. Meeting lunar — agenda 30 min
- 5 min: recap rapid raport (trimis 48h înainte)
- 10 min: Q&A pe date
- 10 min: plan luna viitoare + priorități
- 5 min: AOB (oportunități noi, feedback client)

### 7. KPIs per nișă specifice
**E-commerce**: trafic organic, organic revenue, organic CVR, avg position product pages
**Gambling**: keyword rankings casino/pariuri keywords, organic traffic per landing page, competitor position tracking
**Local business**: GMB impressions, local pack rankings, organic clicks, calls/directions from organic
**SaaS**: organic signups, trial conversions, blog traffic → signup funnel

### 8. YoY vs MoM — când folosești fiecare
- **YoY (Year over Year)**: mai relevant pentru SEO — elimină sezonalitate
- **MoM (Month over Month)**: util pentru tracking activities impact
- Regula: MEREU include YoY ca metric principal, MoM ca secondary
- Q4 (Nov-Dec) trafic e distorsionat sezonier → YoY esențial

## Verificare
- [ ] Template raport creat (Looker Studio sau Slides, branded per client)
- [ ] Surse date conectate (GA4 + GSC + Semrush/Ahrefs)
- [ ] Proces extragere documentat (max 2h per raport monthly)
- [ ] Calendar livrare stabilit (ex: ziua 5 a fiecărei luni)
- [ ] Meeting lunar calendarizat (link Meet + agenda template)
- [ ] Comunicare negative findings framework pregătit
- [ ] Looker Studio dashboard live configurat (opțional dar recomandat)
- [ ] YoY comparison inclus ca metric principal

## Instrumente
- Google Looker Studio — dashboard live + export PDF
- GSC — impressions, clicks, average position
- Semrush Position Tracking — keyword rankings monthly
- GA4 — trafic și conversii organice
- Canva / Google Slides — prezentare formatted

## Note
- Motivul #1 churn client SEO: "nu înțeleg ce plătesc" → rapoarte clare rezolvă
- YoY > MoM pentru SEO (sezonalitatea distorsionează MoM)
- Looker Studio live reduce dramatic întrebări ad-hoc
- Raportul trimis 48h ÎNAINTE de meeting (nu în ziua meetingului)
