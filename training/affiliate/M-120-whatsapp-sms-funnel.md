---
type: procedure
created: 2026-03-17
status: active
slug: m-120-whatsapp-sms-funnel
tags: [procedure, nexus]
---

# WhatsApp/SMS Funnel Automation for Affiliate Marketing — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Construirea si automatizarea funnelurilor de conversie prin WhatsApp Business API si SMS pentru piete cu penetrare email scazuta (LATAM, Africa), cu opt-in compliance per GEO si tracking granular per mesaj.

---

## 1. Problema

M-005 acopera email funnel — dar in Peru/Chile open rate email e 22% vs WhatsApp 85-90%. Kenya: email penetration sub 30%, SMS/WhatsApp e canalul principal. Fara procedura WhatsApp/SMS: se foloseste email ca unic canal, se pierd 60-70% din lead-urile care nu deschid emailul, zero automatizare pe canalul cu cel mai mare engagement.

**Situatii acoperite:**
- Setup WhatsApp Business API cu provider (Twilio/Wati/360dialog)
- Construire secventa automatizata 5-7 mesaje cu A/B testing
- SMS fallback pentru piete cu penetrare WhatsApp mai mica
- Lead capture + opt-in compliant per GEO (Peru Ley 29733, Kenya Data Protection Act 2019, Chile Ley 19.628)
- Tracking granular per mesaj cu Voluum subIDs
- Scaling multi-GEO cu replicare funnel localizat

**Ce se intampla fara procedura:**
- Email-only funnel pe piete cu WhatsApp >80% penetrare → 60-70% audienta pierduta
- Mesaje trimise fara opt-in explicit → sanctiuni legale, ban WhatsApp Business
- Funnel fara tracking per mesaj → imposibil de optimizat, nu stii unde e drop-off-ul
- Copy generica in engleza pe piete non-anglofone → engagement sub 10%
- Lipsa unsubscribe mechanism → spam complaints, delivery rate sub 50%

## 2. Procedura

### Pas 1: CHANNEL SELECTION PER GEO

Per GEO: determina canal primar pe baza datelor de penetrare si comportament utilizator.

| GEO | WhatsApp Penetrare | SMS Penetrare | Canal Primar | Canal Secundar |
|-----|-------------------|---------------|--------------|----------------|
| Peru | 95% | 70% | WhatsApp | SMS fallback |
| Chile | 90% | 65% | WhatsApp | SMS fallback |
| Kenya | 72% | 92% | SMS | WhatsApp complementar |
| Colombia | 88% | 68% | WhatsApp | SMS fallback |
| Nigeria | 65% | 88% | SMS | WhatsApp complementar |

Regula: WhatsApp primar daca penetrare >80%, SMS primar daca WhatsApp <80%. Dual-channel (WhatsApp + SMS) recomandat pentru audienta maxima pe orice GEO.

**Tool**: Claude Opus — analiza GEO data din M-114 scoring + M-115 offer discovery pentru selectie canal.
**Output**: Document `channel-selection-{GEO}.md` cu canal primar, secundar, rationale si date suport.
**Checkpoint**: Canal selectat corespunde cu datele de penetrare din tabel. Daca GEO nu e in tabel, research obligatoriu inainte de decizie.

### Pas 2: PROVIDER SETUP

**(a) WhatsApp Business API:**

| Provider | Cost/mesaj | Plan minim | Best for |
|----------|-----------|------------|----------|
| Twilio | $0.005-0.05/msg per GEO | Pay-as-you-go | Flexibilitate maxima, API robust |
| Wati | $0.03-0.06/msg | $49/luna starter | UI simplu, non-dev teams |
| 360dialog | $0.003-0.04/msg | Pay-as-you-go | Volume mari, cost minim |

Setup: creare cont business, verificare Meta Business Manager, template messages approval (WhatsApp necesita pre-approved templates pentru first-contact — approval dureaza 24-48h).

**(b) SMS:**
- **Twilio SMS** — global, $0.01-0.10/msg per GEO
- **Africa's Talking** — Kenya/Nigeria, $0.005-0.02/msg, superior pe piata africana
- Setup: number provisioning per GEO, sender ID registration (obligatoriu Kenya/Nigeria), shortcode application daca volume >10k/luna

**Tool**: Twilio/Wati/360dialog consoles, Africa's Talking dashboard.
**Output**: Document `provider-setup-{GEO}.md` cu provider selectat, cont creat, verification status, template approval status.
**Checkpoint**: Business verification APPROVED. Cel putin 3 template messages APPROVED de WhatsApp. Sender ID registered pentru SMS. Test message trimis si primit cu succes.

### Pas 3: OPT-IN MECHANISM COMPLIANT PER GEO

Regula FUNDAMENTALA: ZERO mesaje fara opt-in explicit. Violation = ban WhatsApp Business + sanctiuni legale.

**Double opt-in flow:**
1. LP cu checkbox opt-in clar ("Primesc mesaje WhatsApp cu oferte") — checkbox NU pre-checked
2. Primul mesaj = confirmare ("Raspunde DA pentru a continua")
3. Doar dupa confirmare = activare secventa automatizata

**Compliance per GEO:**

| GEO | Lege | Cerinte specifice |
|-----|------|-------------------|
| Peru | Ley 29733 (Proteccion de Datos Personales) | Consimtamant explicit scris, drept la eliminare date in 10 zile, APDP notification obligatorie pentru baze >1000 |
| Kenya | Data Protection Act 2019 | Consimtamant explicit, Data Protection Officer obligatoriu, cross-border transfer restrictions, ODPC registration |
| Chile | Ley 19.628 (Proteccion de la Vida Privada) | Consimtamant explicit, drept la eliminare, stocare date doar pe teritoriu national sau cu consimtamant transfer |

**Unsubscribe mechanism obligatoriu:**
- Fiecare mesaj contine optiune de dezabonare ("Scrie STOP" / "Responde PARAR" / "Reply STOP")
- Procesare automata instant — utilizatorul NU mai primeste mesaje dupa STOP
- Log unsubscribe pentru audit trail

**Tool**: Claude Opus — generare texte opt-in per GEO, compliance review contra legislatie locala; M-118 compliance framework.
**Output**: Document `opt-in-mechanism-{GEO}.md` cu texte LP, texte confirmare, unsubscribe mechanism, compliance checklist.
**Checkpoint**: Opt-in flow testat E2E cu 5 test users. Unsubscribe functional (trimite STOP, confirma ca nu mai primeste mesaje). Texte compliance validate contra legislatie specifica GEO.

### Pas 4: SEQUENCE CONSTRUCTION (5-7 MESAJE)

Secventa localizata per GEO (limba + ton cultural din M-116 Pas 1). Fiecare mesaj are tracking subID unic pentru atribuire per-mesaj in Voluum.

**Message Sequence Template:**

| Mesaj # | Timing | Channel | Content Template | Char Limit | CTA | Tracking subID |
|---------|--------|---------|-----------------|------------|-----|---------------|
| M1 | Imediat post-opt-in | WhatsApp (primar) | Welcome + bonus offer in `currency_format` + salut conform `greeting_style` | 500 (WA) / 160 (SMS) | "Obtine bonusul tau acum" + link | `msg1-{GEO}-{campaign}` |
| M2 | Day 1 | WhatsApp/SMS | Tutorial depozit cu `payment_methods_local` specific — pas cu pas cu screenshots mentionate | 500 / 160 | "Depoziteaza acum" + link tutorial | `msg2-{GEO}-{campaign}` |
| M3 | Day 2 | WhatsApp/SMS | Social proof local (testimonial cu nume locale: "Juan din Lima a castigat 500 PEN") + reminder bonus | 500 / 160 | "Vezi ce au castigat altii" + link | `msg3-{GEO}-{campaign}` |
| M4 | Day 4 | WhatsApp/SMS | Urgency scarcity ("Bonusul tau expira in 48h") + countdown | 500 / 160 | "Activeaza bonusul ACUM" + link | `msg4-{GEO}-{campaign}` |
| M5 | Day 7 | WhatsApp/SMS | Re-engagement cu oferta secundara sau upgrade; disclaimer obligatoriu | 500 / 160 | "Oferta exclusiva pentru tine" + link | `msg5-{GEO}-{campaign}` |
| M6 | Day 14 (optional) | WhatsApp | Win-back cu oferta exclusiva; ton empatic, nu agresiv | 500 | "Ne-a fost dor" + link oferta | `msg6-{GEO}-{campaign}` |
| M7 | Day 30 (optional) | WhatsApp | Survey/feedback + reactivare cu incentive mic | 500 | "Spune-ne parerea" + link survey | `msg7-{GEO}-{campaign}` |

**Reguli per mesaj:**
- SMS: max 160 caractere STRICT (split message = cost dublu + UX prost)
- WhatsApp: max 500 caractere (optimal 200-300 pentru readability)
- Link tracking scurt (custom domain, nu bit.ly — bit.ly blocked pe unele retele)
- Emoji moderat (max 2-3 per mesaj)
- Disclaimer in M1 si M5 conform regulator local (din M-116 cultural brief `disclaimer_text`)
- Toate mesajele contin optiunea unsubscribe

**Tool**: Claude Opus — copy generation per GEO cu ton cultural din M-116; M-116 cultural brief ca input obligatoriu.
**Output**: Document `funnel-sequence-{GEO}-{canal}.md` cu toate mesajele, timing, copy complet in limba locala, subIDs.
**Checkpoint**: Fiecare mesaj sub limita de caractere (count verificat). Disclaimer prezent in M1 si M5. Payment methods locale mentionate in M2. SubIDs unice per mesaj. Copy validata contra M-116 cultural brief (zero expresii engleza, ton nativ).

### Pas 5: TRACKING INTEGRATION PER MESAJ

Configureaza tracking granular per mesaj pentru atribuire conversii la nivel de mesaj individual:

1. Link-uri tracking unice per mesaj cu Voluum subID (din tabelul Pas 4)
2. Postback conversie: operator → Voluum → log per mesaj cu subID ca identificator
3. Configurare S2S postback conform M-121 pentru fiecare link din funnel
4. UTM parameters per mesaj: `utm_source=whatsapp|sms`, `utm_medium=funnel`, `utm_campaign={GEO}`, `utm_content=msg{N}`

**Metrici per mesaj:**

| Metrica | Sursa | Calcul |
|---------|-------|--------|
| Delivery rate | Provider (Twilio/Wati) | Delivered / Sent |
| Open rate | WhatsApp read receipts (WA only) | Read / Delivered |
| Click rate | Voluum click tracking | Clicks / Delivered |
| Conversion rate | Voluum postback S2S | Conversions / Clicks |
| Revenue per mesaj | Voluum revenue tracking | Total revenue / Messages sent |

Dashboard: funnel visualization per GEO cu drop-off per mesaj — identifica exact unde se pierd utilizatorii.

**Tool**: Voluum (tracking setup, dashboards), M-121 (S2S postback framework), M-008 (A/B testing framework).
**Output**: Export config `tracking-funnel-{GEO}.md` cu toate subIDs, postback URLs, UTM schema, dashboard setup.
**Checkpoint**: Test click E2E per fiecare mesaj (click → LP → operator → postback → Voluum) confirmat. Dashboard functional cu date reale din test. SubIDs apar corect in Voluum reports.

### Pas 6: A/B TEST FRAMEWORK

Per mesaj: testeaza 2 variante (A: copy original, B: varianta alternativa). Testeaza secvential: M1 prima, apoi M2, etc.

| Test | Variante | KPI Primar | Split | Sample Size Min |
|------|----------|------------|-------|-----------------|
| M1 copy | Welcome A vs Welcome B | Click rate | 50/50 | 200 delivered/varianta |
| M2 tutorial | Pas cu pas vs Video link | Click rate | 50/50 | 200 delivered/varianta |
| M3 social proof | Testimonial local vs Statistici agregate | Click rate | 50/50 | 200 delivered/varianta |
| M4 urgency | Countdown vs Direct scarcity | Conversion rate | 50/50 | 200 delivered/varianta |
| Channel test | WhatsApp vs SMS (pe GEO dual) | End-to-end CR | 50/50 | 500 delivered/varianta |

**Decision rules:**
- Sub 200 mesaje livrate/varianta = NU decide, continua test
- Peste 500 mesaje + delta >15% = kill loser, scala winner
- Delta sub 5% la 1000 mesaje = no significant difference, pastreaza incumbent

**Tool**: Voluum (split testing), provider API (A/B delivery), M-008 (A/B testing framework).
**Output**: Document `ab-tests-funnel-{GEO}.md` cu testele definite, KPI, stop rules, timeline si rezultate.
**Checkpoint**: Cel putin 3 teste active simultan. Tracking functional pe ambele variante (verificat cu test delivery). Sample size atins inainte de orice decizie.

### Pas 7: AUTOMATION FLOW + SCALING

Configureaza si scala funnel-ul automatizat:

1. **Automation flow**: setup in provider (Twilio Studio / Wati Flows) — trigger = opt-in event, delay-uri programate per mesaj din tabelul Pas 4, branching pe raspuns (DA/NU/STOP)
2. **Conditional logic**: daca user click M1 si converteste → skip M2-M5 (nu mai trimite promotional). Daca user raspunde STOP → instant removal + log
3. **Scaling triggers**: daca funnel profitabil (cost/conversie < CPA target din M-115), creste volume sursa de leads (mai mult trafic pe LP)
4. **Multi-GEO replication**: replicare funnel cu localizare completa (M-116) pe GEO-uri noi — NU copy-paste, ci re-localizare culturala
5. **Monitoring alerts**:
   - Delivery rate <90% → investigare (posibil block WhatsApp sau sender ID issue)
   - Unsubscribe rate >10% → reduce frecventa sau revizuieste copy (spam risk)
   - Click rate M1 <10% → revizuieste welcome message urgent

**Tool**: Twilio Studio / Wati Flows (automation), Claude Opus (optimizare copy pe baza datelor), M-116 (localizare GEO nou).
**Output**: Document `automation-flow-{GEO}.md` cu flow diagram text, triggers, branching rules, scaling criteria, alert thresholds.
**Checkpoint**: Automation flow testat E2E cu 10 test users (parcurgere completa M1-M5). Branching functional (test STOP, test conversion early exit). Alerts configurate si testate (trigger manual alert threshold).

## 3. Cortex Logging

```json
{
  "text": "M-120 WhatsApp/SMS Funnel Automation — Construire si automatizare funneluri conversie prin WhatsApp Business API si SMS pentru piete LATAM/Africa cu opt-in compliance per GEO si tracking granular per mesaj",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-120",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "messaging-automation",
    "pipeline": "A",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "whatsapp", "sms", "funnel", "automation", "LATAM", "Africa", "training", "opt-in", "compliance"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (lansare funnel pe GEO nou)
- La fiecare funnel nou pe piata non-engleza
- La adaugare canal nou (WhatsApp sau SMS) pe GEO existent

### WHEN
- La fiecare campanie noua pe piata cu WhatsApp/SMS penetrare >50%
- La schimbare provider (Twilio → Wati, etc.)
- La schimbare legislatie locala (Peru Ley 29733, Kenya DPA 2019, Chile Ley 19.628)
- Lunar: re-validare delivery rates si compliance mechanism

### HOW (violation detection)
- Email-only funnel pe piata cu WhatsApp >80% penetrare → **VIOLATION**
- Mesaje trimise fara opt-in explicit → **VIOLATION CRITICA** (legal risk)
- Funnel fara tracking per mesaj (subIDs lipsa) → **VIOLATION**
- Copy in engleza pe piata non-anglofona fara localizare → **VIOLATION**
- Lipsa unsubscribe mechanism in orice mesaj → **VIOLATION CRITICA**
- Compliance checklist incomplet si funnel live → **VIOLATION CRITICA**
- Mesaje peste limita caractere (SMS >160) → **WARNING**

### CONNECT
- **M-004** — Landing page cu opt-in mechanism (Pas 3)
- **M-005** — Email funnel (WhatsApp il complementeaza/inlocuieste)
- **M-008** — Voluum tracking setup + A/B testing framework (Pas 5, Pas 6)
- **M-116** — Localizare continut per GEO: limba, ton, cultural brief (Pas 4)
- **M-118** — Gambling compliance framework (Pas 3 compliance)
- **M-121** — Server-side tracking S2S postbacks (Pas 5)

### VERIFY
1. Opt-in flow E2E functional cu double opt-in — testat pe 5 test users per GEO
2. Unsubscribe mechanism functional (STOP → no more messages) — testat pe 3 test users
3. Delivery rate >90% pe primele 100 mesaje livrate — verificat in provider dashboard
4. Tracking subIDs apar corect in Voluum per mesaj — verificat cu test click E2E per mesaj
5. Compliance texte (disclaimer, 18+, unsubscribe) prezente in M1 si M5 — manual review
6. Copy localizata validata contra M-116 cultural brief — zero expresii engleza ramase
7. Automation branching functional (STOP exit, conversion early exit) — testat cu 3 scenarii

### MODEL ROUTING
- **Claude Opus**: Copy generation per GEO, A/B variant creation, compliance review, cultural adaptation
- **Claude Sonnet**: Character count validation, format checks, quick translations (cost optimization)

## 5. Dependente

- **Tools**: Twilio/Wati/360dialog (WhatsApp API), Twilio SMS, Africa's Talking (Kenya/Nigeria SMS), Voluum (tracking), Claude Opus (copy generation)
- **Proceduri input**: M-114 (GEO scoring — date penetrare), M-115 (offer discovery — CPA targets), M-116 (cultural brief — localizare), M-118 (compliance framework)
- **Proceduri framework**: M-004 (LP cu opt-in), M-005 (email funnel — complementar), M-008 (tracking + A/B), M-121 (S2S tracking)
- **Proceduri adjacent**: M-117 (competitive intelligence), M-119 (TikTok/native ads — cross-channel)
- **Data sources**: Provider documentation (Twilio, Wati, Africa's Talking), legislatie locala (Peru Ley 29733, Kenya DPA 2019, Chile Ley 19.628)
- **Output consumed by**: M-116 (funnel localizat la Pas 4), M-121 (tracking per mesaj)

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| WhatsApp open rate LATAM | >70% | Per campanie | Twilio/Wati read receipts |
| WhatsApp open rate Africa | >55% | Per campanie | Twilio/Wati read receipts |
| Click rate M1 (welcome) | >15% | Per campanie | Voluum subID msg1 |
| Conversion rate funnel E2E | >3% | Saptamanal | Voluum postback S2S |
| Unsubscribe rate | <10% | Saptamanal | Provider dashboard |
| Delivery rate | >90% | Per mesaj batch | Provider dashboard |
| SMS delivery rate Kenya | >85% | Per batch | Africa's Talking |
| Cost per conversion via funnel | <CPA target M-115 | Saptamanal | Voluum cost tracking |
| A/B test velocity | >3 teste/luna | Monthly | Test log |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6 + Checklist)
- [x] Pasi numerotati secvential (1-7) cu Tool, Output si Checkpoint per pas
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] VERIFY cu 7 verificari concrete cu metoda specifica
- [x] KPIs masurabile cu target numeric, frecventa si sursa
- [x] Dependente listate (tools + proceduri input/framework/adjacent + data sources + output consumed by)
- [x] Metrics cu target, frecventa si sursa masurare (9 metrici)
- [x] Message sequence template table cu 7 mesaje (Mesaj #, Timing, Channel, Content, Char Limit, CTA, Tracking subID)
- [x] Compliance per GEO explicit (Peru Ley 29733, Kenya DPA 2019, Chile Ley 19.628)
- [x] Conexiuni cu proceduri existente validate (M-004, M-005, M-008, M-116, M-118, M-121)
- [x] A/B test framework cu decision rules si sample size minim
