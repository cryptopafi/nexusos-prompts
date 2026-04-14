---
id: AGO-003
title: Client Onboarding Process and Checklist
domain: agency-operations
source: Digital Client Manager - Client Management and Consulting (Udemy)
version: 2.0
forge_score: "estimated H/3.0"
business_mapping: ["ai-agency"]
---

# Client Onboarding Process and Checklist

## Obiectiv
Implementează un proces de onboarding standardizat care transformă un client nou semnat în client activ în maxim 5 zile lucrătoare, setând așteptările corecte, colectând toate informațiile necesare, configurând tooling-ul intern și livrând prima valoare vizibilă (quick win) în primele 14 zile — reducând churn-ul din primele 90 zile de la media de 20-25% la sub 10%.

## Pași

### Pas 1: Trimite Welcome Package automatizat în sub 4 ore de la semnare
- Configurează automation în GoHighLevel sau HubSpot: la status "Closed Won" → trigger Welcome Package automat
- **Welcome Email** conține:
  - Confirmare semnare + confirmare plată primită (sau link de plată dacă nu e procesată)
  - Next steps cu timeline exact: "Ziua 1: Primești acest email + questionnaire. Ziua 2-3: Kickoff Call. Ziua 5: Setup complet. Ziua 14: Prima livrare + quick win."
  - Datele de contact ale account managerului dedicat (nume, email, telefon, program de disponibilitate)
  - Link Calendly pentru Kickoff Call (programat în max 3 zile lucrătoare)
- **Welcome Package atașamente**:
  - Onboarding Questionnaire link (Typeform sau Google Form)
  - Client Portal access (ClickUp guest access sau Notion shared page)
  - Brand asset request checklist (logo vector, brand guidelines, font files, photo library)
  - Acces credentials form (Google Analytics, Search Console, Ad accounts, CMS login, social media — folosește LastPass/1Password shared vault, NU email cu parole)
- **Welcome video** (opțional dar puternic): 2-min Loom personalizat de la account manager — "Bun venit la [agenție], [Nume client]! Sunt [Nume] și voi fi partenerul tău principal. Iată ce urmează..." Conversion de la "nou client" la "partener" crește cu 30-40% cu video personalizat.
- Ton: entuziast, profesionist, concret. Zero vagueness. "Vom începe în curând" = GREȘIT. "Kickoff call este marți 14:00 CET" = CORECT.

### Pas 2: Colectează informațiile prin Onboarding Questionnaire structurat
- **Secțiunea Business Context** (obligatorie):
  - Ce face compania, USP (Unique Selling Proposition), target audience (demografie + psihografie)
  - Top 3 competitori (cu URL-uri — vei face competitive audit)
  - Revenue lunar actual și target la 6/12 luni
  - Ciclul de vânzare: cât durează de la lead la client plătitor?
- **Secțiunea Goals & History** (obligatorie):
  - Ce vrea să obțină în 30 / 90 / 180 zile — în termeni măsurabili (nu "mai mult trafic" → "200 leads/lună")
  - Ce a încercat înainte: in-house? altă agenție? DIY? Ce a funcționat și ce nu?
  - Cum definește succesul — ce cifră, dacă o atinge, înseamnă că investiția a meritat?
  - Deal-breakers: ce ar fi inacceptabil? (ex: "să nu postăm nimic politic", "să nu folosim AI-generated images")
- **Secțiunea Assets & Access** (obligatorie, cu deadline):
  - Google Analytics 4 (grant admin access la email agenție), Google Search Console, Google Ads account, Meta Business Suite, TikTok Ads, CMS access (WordPress/Shopify/Webflow login), email marketing platform (Klaviyo/Mailchimp)
  - Brand assets: logo vector (AI/SVG/EPS), brand guidelines PDF, font files, approved photo library (Dropbox/Drive link)
  - Existing content: blog posts, videos, lead magnets, previous campaign reports
- **Secțiunea Operațional** (obligatorie):
  - POC (persoana de contact principală) + backup POC
  - Cine aprobă materialele? (un singur decision maker — nu comitete)
  - Procesul intern de aprobare: câți pași? cât durează? (avertizare: dacă aprobarea durează >5 zile business, impactează timeline-ul — comunică explicit)
  - Zile/ore de contact preferate + canal preferat (email/Slack/WhatsApp)
  - Blackout periods: perioade în care nu se lansează campanii (holiday, audit intern, event)
- **Deadline hard pentru completare**: 48 ore de la primire. Email auto-reminder la 24h și 48h. Dacă nu completează în 48h → account manager sună direct. Mesaj: "Questionnaire-ul e necesar pentru a respecta timeline-ul. Fără el, Kickoff Call-ul se amână."

### Pas 3: Conduce Kickoff Call structurat (60-90 minute)
- **Pre-call prep** (account manager, 30 min):
  - Review questionnaire complet, notează 3-5 clarificări
  - Quick competitive audit: top 3 competitori (SimilarWeb, SEMrush quick view), observații principale
  - Verifică accesuri primite — ce funcționează, ce lipsește
  - Pregătește Kickoff Deck (Google Slides template standardizat, 10-12 slides)
- **Agenda fixă** (nu improviza — comunică agenda în avans):
  1. (5 min) Prezentări personale, ton, ground rules comunicare
  2. (15 min) Review questionnaire + clarificări. Întreabă: "Am înțeles corect că [X]? Ce am omis?"
  3. (20 min) Audit rapid al situației actuale — share screen, arată ce ai observat deja (site speed, ranking gaps, ad account waste, social presence gaps). Demonstrează competență din ziua 1.
  4. (15 min) Setarea așteptărilor explicite: ce livrezi, când, cum comunici, ce NU livrezi, ce ai nevoie de la ei. Citește secțiunea EXCLUS DIN SCOPE din SOW — confirmă că au înțeles.
  5. (10 min) Quick wins planificate: "În primele 14 zile, vom face [X] care va avea impact [Y]. Veți vedea prima valoare concretă până la [data]."
  6. (5 min) Next steps cu date exacte, acțiuni cu owner clar, deadline-uri
- Înregistrează call-ul (cere permisiune explicit, offer transcription) — trimite Loom/recording link cu summary
- **Post-call deliverable (24h)**: Kickoff Summary email cu:
  - Obiective agreate (30/90/180 zile)
  - KPIs contractate (din SOW, confirmate verbal)
  - Acțiuni agenție + deadline
  - Acțiuni client + deadline
  - Cadența de reporting agreată (săptămânal/lunar)
  - Canal de comunicare confirmat + SLA response time

### Pas 4: Configurează tooling intern și client workspace (Ziua 2-4)
- **ClickUp/Asana client space**:
  - Create project: "[Client Name] — [Service]"
  - Lists: Onboarding Tasks / Monthly Deliverables / Content Calendar / Approvals / Ad Hoc
  - Tasks pentru luna 1 cu deadline-uri, assignees, dependencies
  - Custom fields: Status, Priority, Approval Status, Due Date
  - Client access: guest viewer pe spațiul lor (vede progress, nu editează)
- **Google Drive folder structure**:
  ```
  /Clients/[Client Name]/
    /01-Onboarding/ (questionnaire, kickoff recording, SOW semnat)
    /02-Assets/ (logo, brand guide, photos, fonts)
    /03-Strategy/ (audit, competitive analysis, strategy doc)
    /04-Content/ (blog drafts, social posts, ad creatives)
    /05-Reports/ (monthly reports, QBR decks)
    /06-Approvals/ (drafts awaiting client approval)
    /07-Admin/ (invoices, COs, contracts)
  ```
- **GoHighLevel sub-account** (dacă oferă white-label SaaS):
  - Create sub-account cu branding client
  - Import snapshot per nișă (AGO-009)
  - Connect phone numbers, email, calendar
  - Activate automations: lead capture → CRM → nurture → appointment
- **Analytics & tracking setup**:
  - Verifică GA4 implementation (events, conversions goals, cross-domain tracking)
  - GSC verified + sitemap submitted
  - Ad accounts auditate: pixel tracking, conversion events, attribution model
  - Connect Looker Studio sau AgencyAnalytics dashboard — primele date live în 24h
  - Setează UTM standards pentru tot ce trimite agenția
- **Intern: capacity allocation**:
  - Asignează team members cu ore alocate per lună în Harvest/Toggl
  - Update resource planning spreadsheet
  - Setează alerte: notify account manager dacă client consumption atinge 80% din ore incluse

### Pas 5: Livrează Quick Win în primele 7-14 zile
- **Principiu**: prima valoare vizibilă trebuie livrată ÎNAINTE de primul raport lunar. Quick win-ul construiește încrederea timpurie și justifică decizia de cumpărare.
- **Quick wins per serviciu** (alege 1-2 din lista de mai jos):
  - **SEO**: audit tehnic cu top 5 issues fixate (broken links, missing meta, speed fix, schema markup) — vizibil în GSC în 1-2 săptămâni
  - **PPC**: audit campanie existentă + prima optimizare (negative keywords cleanup, bid adjustments, ad copy refresh) — vizibil în cost savings imediat
  - **Social Media**: primele 10 postări aprobate și programate + prima analiză competitor — vizibil în feed imediat
  - **Email Marketing**: email list cleanup (remove bounces/inactives) + prima secvență automată setup — vizibil în deliverability improvement
  - **Web**: speed optimization (image compression, caching, CDN) + 3 quick UX fixes — vizibil în PageSpeed score improvement
  - **GHL SaaS**: missed call text-back activat + appointment booking live — vizibil în primele leads captured
- **Comunicare proactivă** a quick win-ului: nu aștepta raportul lunar. Trimite email/Slack: "Update rapid: Am făcut [X], care a rezultat în [Y]. Detalii: [screenshot/link]."
- Include before/after screenshot sau metric — vizual bate textul de fiecare dată

### Pas 6: Setup monitoring și alerting continuu
- Configurează Google Alerts pentru brand client + competitors
- Setează uptime monitoring pentru site (UptimeRobot gratuit)
- Activează anomaly alerts în GA4 (traffic drops >20%, conversion rate changes)
- Configurează ad account alerts: budget pacing, CPA spikes, disapproved ads
- **Internal health monitoring**: weekly check pe fiecare client nou (primele 60 zile):
  - Suntem on track cu deliverables?
  - Clientul răspunde la timp?
  - Au apărut cereri out-of-scope?
  - Health Score inițial (AGO-014)

### Pas 7: Trimite Onboarding Completion Summary la ziua 14
- Document scurt (1 pagină PDF branded):
  - Ce s-a realizat în primele 14 zile (fiecare deliverable cu status ✅)
  - Quick win livrat cu metrici/screenshot
  - Baseline metrics stabilite (tabel: metric | value | source | date)
  - Toate accesurile confirmate funcționale (checklist)
  - Plan luna 1 complet (task-uri cu deadline-uri)
  - Cadență reporting confirmată + data primului raport lunar
- **Onboarding NPS survey** (3 întrebări, Google Form sau Typeform):
  1. "Pe o scară de 1-10, cât de clar a fost procesul de onboarding?" (target: 8+)
  2. "Ce aspect al onboarding-ului ai îmbunătăți?"
  3. "Te simți confident că suntem pe drumul cel bun?" (Da/Nu/Parțial)
- Dacă NPS onboarding < 7: account manager programează call de clarificare în 48h — nu lăsa un client nemulțumit la 14 zile
- Folosește feedback-ul agregat trimestrial pentru a îmbunătăți procesul de onboarding

### Pas 8: Tranziție de la onboarding la ongoing management (Ziua 15-30)
- Ziua 15-20: prima livrare majoră conform planului (raport complet, primele ads live, primul batch content publicat)
- Ziua 21: check-in call scurt (15-20 min): "Cum te simți? Ce întrebări ai? Suntem aliniați?"
- Ziua 25-30: primer report lunar complet (AGO-007) cu baseline comparison
- Ziua 30: account manager review intern: "Este clientul potrivit? Estimarea de ore e corectă? Trebuie ajustat ceva la pachet?"
- De la ziua 30: clientul intră în cadența normală de management (monthly report, weekly/bi-weekly check-ins dacă e contractat, QBR trimestrial)

## Verificare
- [ ] Welcome Package trimis automat în sub 4 ore de la semnare
- [ ] Onboarding Questionnaire completat de client (deadline 48h respectat)
- [ ] Kickoff Call ținut cu agenda structurată și recording
- [ ] Summary Kickoff Call trimis în 24 ore cu acțiuni și deadline-uri
- [ ] Toate accesurile primite, verificate și funcționale
- [ ] Client workspace creat în ClickUp/Asana + Google Drive
- [ ] GoHighLevel sub-account configurat (dacă se aplică)
- [ ] Analytics și tracking verified
- [ ] Quick win livrat și comunicat proactiv în primele 14 zile
- [ ] Onboarding Completion Summary trimis la ziua 14
- [ ] NPS survey colectat
- [ ] Tranziție la ongoing management completă la ziua 30

## Instrumente
- GoHighLevel / HubSpot — automation welcome sequence + CRM status trigger
- Typeform ($25/mo) / Google Forms (free) — Onboarding Questionnaire
- ClickUp ($7/user/mo) / Asana (free-$11/user/mo) — client workspace și project management
- Google Drive — asset management și document sharing
- Calendly — scheduling Kickoff Call
- Loom ($15/mo) — welcome video personalizat + Kickoff Call recording
- 1Password for Teams ($4/user/mo) / LastPass — secure credential sharing (NU email)
- Harvest ($12/user/mo) / Toggl Track (free-$9/user/mo) — time tracking per client
- Looker Studio (free) / AgencyAnalytics ($12/client/mo) — dashboard setup
- Screaming Frog (free to 500 URLs) — technical audit pentru quick win SEO

## Note
- **Primele 30 zile definesc relația pe termen lung** — investește timp disproporționat în onboarding. Agențiile cu onboarding structurat au 40% mai puțin churn în primele 90 zile (sursa: HubSpot Agency Benchmark 2024)
- Dacă clientul nu completează questionnaire-ul în 48 ore, sună direct — nu aștepta pasiv. Apatic la onboarding = semnale de churn timpuriu
- Quick win-ul nu trebuie să fie spectaculos, trebuie să fie vizibil și atribuit muncii tale
- Standardizează onboarding-ul dar personalizează comunicarea — nu trimite template-uri reci cu "[Client Name]" uitat neschimbat
- **Cost onboarding**: calculează cât costă onboarding-ul per client (ore × cost). Benchmark: onboarding sănătos = 5-10% din valoarea contractului pe primul an. Dacă e mai mult, simplifică procesul.
- Onboarding este prima oportunitate de a demonstra că ești diferit de "agenția anterioară" — livrează exemplar aici, chiar dacă sacrifici marjă pe luna 1
