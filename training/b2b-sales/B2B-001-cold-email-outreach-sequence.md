---
id: B2B-001
title: Cold Email Outreach Sequence Design
domain: b2b-sales
source: AI Cold Emails Lead Generation Sales Masterclass 2024
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Cold Email Outreach Sequence Design — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Proiectarea unei secvențe de cold email B2B cu deliverability optimă, personalizare relevantă și calls-to-action care generează întâlniri/răspunsuri calificate. Incorporează SPIN selling principles în messaging și Challenger Sale reframes în follow-up-uri.

---

## 1. Problema

Secvențele de cold email fără structură generează rate de răspuns sub 1% și risc de spam. Fără o metodologie clară pentru subiect, body, CTA și follow-up, resursele de outreach sunt irosite și reputația domeniului de email este compromisă. Cold email-ul rămâne cel mai scalabil canal de outreach B2B cu cost per lead de $0.10-0.50 vs. $5-50 pentru ads, dar NUMAI dacă deliverability și personalizarea sunt impecabile.

Situații acoperite:
- Lansare campanie outreach pentru un segment de piață nou
- Scalarea unui proces manual de cold email într-un sistem semi-automatizat
- Optimizarea unei secvențe existente cu rate de răspuns scăzute
- Agency selling marketing/dev services to SMBs (deal size $2K-25K/mo)
- Agency selling to enterprise (deal size $25K+/mo, longer cycle)

---

## 2. Procedura

### Pas 1: Definirea ICP și segmentarea listei (Challenger Profile)
Definește Ideal Customer Profile (ICP) cu specificitate maximă: industrie verticală (nu "tech" — ci "B2B SaaS, Series A-C, 50-200 employees"), titlu decident exact (VP Marketing, Head of Growth, Founder/CEO pentru SMBs), trigger events (funding round în ultimele 90 zile, hiring pentru rol relevant, expansion to new market, bad reviews pe G2, competitor acquisition). Segmentează lista în cel puțin 3-5 segmente distincte cu messaging diferit per segment. **Agency-specific**: pentru vânzarea serviciilor de marketing către SMBs, segmentează pe: (A) businesses cu website dar fără SEO/ads active, (B) businesses care rulează ads dar cu landing pages slabe, (C) businesses în creștere care au postat joburi de marketing (nu au echipă internă). Fiecare segment primește un angle diferit bazat pe pain point-ul predominant. Folosește Apollo, Clay, sau LinkedIn Sales Navigator pentru building liste. Calitate > cantitate: 500 de prospects bine segmentate bat 5,000 generice.

### Pas 2: Infrastructure de deliverability — domeniu, DNS, warming
**Domeniu separat obligatoriu**: Nu trimite NICIODATĂ cold email de pe domeniul principal al business-ului (risc de blacklist pe domeniul principal). Cumpără un domeniu similar: dacă business-ul este `acme.io`, folosește `getacme.io`, `acme-growth.io` sau `tryacme.io`. Configurare DNS obligatorie pe domeniul de outreach:
- **SPF**: `v=spf1 include:_spf.google.com ~all` (sau provider-ul folosit) — publică în DNS TXT record
- **DKIM**: Generează cheia DKIM din provider-ul de email (Google Workspace, Zoho), adaugă record-ul CNAME/TXT în DNS
- **DMARC**: `v=DMARC1; p=none; rua=mailto:dmarc@domain.com` — pornește cu `p=none` pentru monitoring, treci la `p=quarantine` după 30 zile clean
- **Custom tracking domain**: Configurează un subdomain de tracking custom în tool-ul de sequencing (ex: `track.getacme.io`) — evită domeniile de tracking shared care sunt blacklisted
- Verificare: MXToolbox.com → introduci domeniul → verifică SPF, DKIM, DMARC toate verzi

**Warming protocol** (domeniu nou): Folosește Instantly Warmup, Warmbox.ai, sau Lemwarm. Programul de ramp: Săptămâna 1: 2-3 emailuri/zi (doar warming), Săptămâna 2: 5-8/zi, Săptămâna 3: 10-15/zi, Săptămâna 4: 20-30/zi. După 4 săptămâni: poți trimite max 30-50 emailuri reale/zi/domeniu. Pentru scalare peste 50/zi: adaugă domenii și mailbox-uri suplimentare (max 50 emailuri/mailbox/zi). Monitorizează Google Postmaster Tools zilnic în prima lună.

### Pas 3: Scrierea emailului de deschidere (Email 1) — Challenger Reframe
Structură obligatorie cu timpuri de scriere per secțiune:
- **Subject line** (max 6 cuvinte, lowercase, fără clickbait, fără RE: fake): Testează 3-5 subjecte per segment. Formule câștigătoare: "[prenume], question about [topic]", "[company] + [pain point]", "noticed [trigger event]". Response rate benchmarks: subject lines personalizate = 30-50% open rate vs. 15-25% generice
- **Opening line** (prima propoziție = personalizare specifică): NU "Am văzut că lucrezi la X" (generic). DA: "Am citit articolul tău despre [topic specific] — ideea despre [detaliu] m-a pus pe gânduri." sau "Am văzut că ați făcut hire pentru [rol] luna trecută — de obicei asta înseamnă că [pain point specific]."
- **Challenger reframe** (1-2 propoziții): Oferă un insight pe care prospectul nu îl are. "Majoritatea companiilor din [industrie] pierd [X]% din budget pe [activitate] fără să realize că [insight]. Am ajutat [client similar] să [rezultat] prin [abordare diferită]."
- **CTA** (o singură acțiune, da/nu): "Merită 15 minute să-ți arăt cum funcționează?" sau "Are sens să vorbim săptămâna viitoare?" — nu cere mai mult de o acțiune. Evită CTA cu friction mare ("Completează acest formular").
- **Lungime totală**: 50-100 cuvinte body (sub 125 absolut). Fără atașamente, fără imagini, fără HTML complex (toate reduc deliverability).

### Pas 4: Scrierea follow-up-urilor (Email 2-5) cu cadence precis
Fiecare follow-up TREBUIE să adauge valoare nouă — nu doar "Am vrut să verific dacă ai văzut emailul meu":
- **Email 2 (Ziua 3)**: Reply la Email 1 (thread, nu email nou). Adaugă un alt unghi sau un data point relevant. "Am uitat să menționez — am publicat recent un [case study/articol] despre cum [companie similară] a [rezultat]. [Link]"
- **Email 3 (Ziua 7)**: Valoare gratuită — un insight, un mini-audit gratuit, o observație despre industria lor. "Am făcut un quick audit al [site-ului/ads-urilor/presences lor] — am găsit 3 oportunități pe care le pierdeți. Merită 10 minute să le discut cu tine?"
- **Email 4 (Ziua 14)**: Challenger reframe #2 — prezintă costul inacțiunii (SPIN Implication). "Companiile din [industrie] care nu [acțiune] pierd în medie [X]/lună. Nu spun asta pentru a presa — vreau doar să fie pe radarul tău."
- **Email 5 (Ziua 21)**: Breakup email — ton de plecare, scurt, fără presiune. "Pare că momentul nu e potrivit — înțeleg complet. Dacă situația se schimbă, răspunde la acest email oricând. Succes cu [proiect/obiectiv menționat anterior]!"

### Pas 5: Personalizarea la scală cu AI (agency-specific)
Creează un template cu variabile de personalizare: `{{first_name}}`, `{{company}}`, `{{recent_trigger}}`, `{{pain_point_specific}}`, `{{competitor_mention}}`, `{{industry_insight}}`. Workflow de personalizare:
1. **Data collection** (Clay, Phantombuster, Apollo enrichment): colectează LinkedIn about, recent posts, company blog, G2 reviews, Crunchbase data, recent job postings per prospect
2. **AI prompt** pentru prima linie: "Based on this data about [prospect], write a 1-sentence specific observation that connects their recent activity to a pain point in [domain]. Do NOT use flattery. Be specific, not generic. Max 25 words." Model: Claude Haiku sau GPT-4o-mini ($0.01-0.03/prospect)
3. **QC**: Revizuiește 15-20% din outputs manual. Target: ≥85% outputs utilizabile fără rescris. La sub 80%: ajustează prompt-ul
4. **Agency-specific personalization angles**: pentru agentii care vând servicii de marketing — personalizează pe: website-ul lor (site speed, SEO gaps vizibile), social media activity, ads library (ce ads rulează pe Meta/Google), reviews pe Google/Trustpilot

### Pas 6: Setup tehnic și lansare
Configurează tool-ul de sequencing (Instantly recomandabil pentru cost/feature, Lemlist pentru personalizare avansată, Apollo pentru all-in-one, Smartlead pentru multi-mailbox). Setup obligatoriu:
- **Sending window**: 8:00-17:00 în timezone-ul prospectului (nu al tău)
- **Randomizare timing**: ±30-60 minute între emailuri
- **Daily limit**: Max 30 emailuri/mailbox primele 2 săptămâni post-warming, apoi max 50/mailbox/zi
- **Open/click tracking**: DEZACTIVEAZĂ click tracking (adaugă tracking pixel URLs care pot fi flagged spam). Open tracking: evaluează risc vs. beneficiu (unele filtere spam îl detectează)
- **Unsubscribe link**: Obligatoriu legal (CAN-SPAM, GDPR). Adaugă un text mic în footer: "Nu mai vrei emailuri de la mine? Răspunde cu 'stop'." (mai deliverable decât un link de unsubscribe shared)
- **Test pre-lansare**: Trimite 10 emailuri reale (la colegi, prieteni, alte adrese) și verifică că ajung în inbox (nu spam/promotions)

### Pas 7: Monitorizare zilnică și A/B testing continuu
Monitorizează ZILNIC în primele 2 săptămâni, apoi de 3x/săptămână:
- **Open rate**: Target ≥45% (cu subject line personalizate). Sub 30%: problemă de deliverability SAU subject line slab → testează alt subject
- **Reply rate total**: Target ≥5% (include negative replies). Benchmark real cold email B2B: 1-3% reply rate pe campanii noi. Peste 5% = performanță excelentă
- **Positive reply rate**: Target ≥2% (din total emails trimise). Acest % generează meetings
- **Bounce rate**: SUB 3% obligatoriu. Peste 5%: verifică lista cu ZeroBounce/NeverBounce și elimină emailuri invalide IMEDIAT (bounce rate ridicat distruge reputația domeniului)
- **Spam complaint rate**: SUB 0.1% (Google threshold = 0.3% → blacklist). Dacă depășește 0.1%: oprește campania, investighează
- **A/B testing**: Testează simultan 2 variante de subject line pe fiecare segment (split 50/50). După 100+ emailuri per variantă: păstrează câștigătorul. Apoi testează opening line, apoi CTA. Un singur element pe test

### Pas 8: Handoff la sales și documentarea learningurilor
La fiecare reply pozitiv: (1) Răspunde manual în max 2h (nu automat) cu ton conversational. (2) Propune meeting cu link Calendly sau 2-3 time slots concrete. (3) Mută în CRM cu tag "Cold Email - Interested" + notează segmentul, triggerul și mesajul care a generat reply-ul. La fiecare 500 emailuri trimise: documentează winning variants (subject lines, opening lines, CTAs cu cele mai bune reply rates) în biblioteca de templates pentru reutilizare.

---

## 3. Cortex Logging

```json
{
  "text": "Secvență cold email B2B designată: ICP segmentat (3-5 segmente), domeniu separat configurat (SPF/DKIM/DMARC), warming 4 săptămâni, 5-email sequence cu Challenger reframes, personalizare AI la scală, KPIs monitorizați zilnic.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-001",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["cold-email", "outreach", "b2b", "email-sequence", "deliverability", "spf", "dkim", "dmarc", "challenger-sale", "agency-sales"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Domeniu fără SPF/DKIM/DMARC configurat → violation
- Email lansat fără warming pe domeniu nou → violation
- CTA cu mai mult de o acțiune cerută → violation
- Cold email trimis de pe domeniul principal al business-ului → violation critică
- Mai mult de 50 emailuri/zi/mailbox → violation deliverability
- Bounce rate >5% fără acțiune imediată → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum b2b-sales

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Domeniu separat configurat cu SPF/DKIM/DMARC verificat pe MXToolbox?
- [ ] Warming minim 3-4 săptămâni înainte de outreach real?
- [ ] Fiecare email are max un singur CTA?
- [ ] Reply rate și bounce rate monitorizate zilnic?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-001 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Cold Email Outreach Sequence Design" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Instantly / Lemlist / Smartlead | Sequencing și deliverability management |
| Apollo / Hunter.io / Clay | Prospecting și enrichment date contact |
| Google Postmaster Tools / MXToolbox | Monitorizare reputație domeniu |
| ZeroBounce / NeverBounce | Verificare validitate emailuri pre-launch |
| Claude Haiku / GPT-4o-mini | Personalizare la scală prima linie |
| Warmbox.ai / Instantly Warmup / Lemwarm | Domain warming automat |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Open rate | ≥45% (cu personalizare) |
| Reply rate total | ≥5% (benchmark real: 1-3%) |
| Positive reply rate | ≥2% |
| Bounce rate | <3% (critic: <5%) |
| Spam complaint rate | <0.1% |
| Meetings booked per 1000 emails | ≥10 |
| Cost per meeting booked | <$50 |
| Time to first reply | <48h de la lansare |
