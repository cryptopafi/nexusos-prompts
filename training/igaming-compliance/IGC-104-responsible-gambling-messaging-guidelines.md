---
id: IGC-104
title: Responsible Gambling Messaging Guidelines
domain: igaming-compliance
source: UKGC Social Responsibility Code / MGA RG Guidelines / GGL Spielerschutz / ONJN OUG 77/2009
version: 1.0
forge_score: —
business_mapping: ["smsads", "gambling-seo", "affiliate-ops"]
status: ACTIVE
created: 2026-03-20
rule_id: META-H-002
---

# Responsible Gambling Messaging Guidelines — Procedura Standard de Operare

## Obiectiv

Implementarea mesajelor de responsible gambling (RG) conforme per jurisdicție, pe toate canalele de comunicare (site, email, SMS, social media, ads). Aplicabil operatorilor și afiliaților iGaming.

## Prerequisites

- Channel inventory (site, email, SMS, social, ads, push)
- RG messaging library per jurisdiction (UKGC/MGA/GGL/ONJN/BCLB/Curaçao)
- Geo-detection IP service for serving jurisdiction-specific messages
- SMS template manager with character counting
- Content management system with geo-targeting capability

## 1. Problema

Fiecare jurisdicție iGaming impune cerințe specifice de responsible gambling messaging. UKGC prin Social Responsibility Code impune customer interaction și messaging pe toate touchpoints. GGL impune cel mai strict regim (panic button, €1,000 deposit cap, OASIS self-exclusion). MGA require linkuri către resurse RG. Nerespectarea duce la: sancțiuni (UKGC a aplicat amenzi > £10M pentru RG failures), pierderea licenței, daune reputaționale severe. Afiliații sunt indirect responsabili — operatorii UKGC/MGA trebuie să auditeze afiliații pentru RG compliance. SMS campaigns (SMSads) au cerințe suplimentare de RG messaging.

**Business Application**: SMSads (RG messaging obligatoriu în fiecare SMS gambling, format specific per jurisdicție), Gambling SEO (RG content pe site-uri affiliate — criterii de ranking SEO și compliance), Affiliate operations (RG messaging pe landing pages, reviews, comparisons — auditat de operatorii parteneri).

---

## 2. Procedura

### Pas 1: INVENTAR CANALE ȘI TOUCHPOINTS
Listează toate canalele de comunicare cu playeri/potențiali playeri:
- **Website**: homepage, landing pages, review pages, comparison tables, bonus pages, blog posts
- **Email**: welcome emails, promotional emails, re-engagement emails, newsletters
- **SMS** (SMSads): promotional SMS, transactional SMS, re-engagement SMS
- **Social media**: posts organice, ads plătite (Facebook, Instagram, Twitter/X, TikTok)
- **Paid ads**: Google Ads, display ads, native ads, programmatic
- **Push notifications**: mobile și web push
- **In-app/In-site**: banners, pop-ups, interstitials
Per fiecare canal, notează: frecvența, audiența (jurisdicție), tipul de mesaj (promotional/informational).

### Pas 2: DEFINIRE MESAJE RG PER JURISDICȚIE
Configurează messaging library cu variante per jurisdicție:

**UKGC (UK) — Cel mai detaliat:**
- Badge/text obligatoriu: "18+" vizibil pe orice material promotional
- Link organizație: "BeGambleAware.org" sau "www.begambleaware.org" (mandatory)
- Alternative: GamCare (gamcare.org.uk), National Gambling Helpline (0808 8020 133)
- Text RG recomandat: "Please gamble responsibly" sau "When the fun stops, stop"
- Self-exclusion: menționare GAMSTOP (gamstop.co.uk) — network-wide self-exclusion
- Customer interaction: operatorii (și indirect afiliații) trebuie să intervină la indicators of harm
- T&C bonusuri: complet vizibile, nu hidden behind click

**MGA (Malta):**
- Age restriction: "18+" sau "Players must be 18+"
- Link RG: responsiblegambling.org sau echivalent MGA-approved
- Self-exclusion info: menționare mecanismul de self-exclusion al operatorului
- Text: "Gambling can be addictive. Play responsibly."
- Cooling-off period info: pe pagini de depozit
- MGA licence number: vizibil în footer

**GGL (Germania) — Cel mai strict:**
- Panic button: funcționalitate obligatorie pe site (instant self-exclusion minim 24h)
- OASIS referință: menționare sistemul national de self-exclusion
- Deposit limit: menționare €1,000/lună limit cross-operator
- Text: "Spielen Sie verantwortungsvoll" + "Spielen kann süchtig machen"
- BZgA helpline: 0800 137 27 00 (gratuit)
- Auto-play: menționare interdicție (slots) unde relevant
- Restricții orar advertising: TV/radio doar 21:00-06:00
- GGL licence number: vizibil

**Curaçao:**
- Minimum: "18+" badge, "Gamble responsibly" text
- Self-exclusion: menționare opțiune la request
- Link RG: la discreția operatorului (cerință minimă)
- Licence seal: vizibil (master licensee logo)

**ONJN (România):**
- Text obligatoriu: "Jocurile de noroc implică riscuri financiare și pot crea dependență"
- "18+" vizibil
- Link: jocresponsabil.ro
- Self-exclusion: informare despre registrul național de auto-excludere
- Număr telefon asistență: disponibil pe site
- Licență ONJN: număr vizibil

**BCLB (Kenya):**
- "18+" badge obligatoriu
- "Gambling is addictive" warning
- Helpline number: afișat vizibil
- Self-exclusion: informare despre opțiune
- BCLB licence number: vizibil

### Pas 3: IMPLEMENTARE PE WEBSITE/LANDING PAGES
Pentru fiecare pagină:
- **Header/Navigation**: 18+ badge vizibil (minim 12px, contrast suficient)
- **Footer**: RG text complet per jurisdicție, linkuri organizații, licence numbers, helpline numbers
- **Bonus pages**: T&C complete vizibile (nu expandable/hidden), wagering requirements explicit
- **Deposit pages**: RG reminder, link self-exclusion, deposit limit setter
- **Geo-targeting**: detectează jurisdicția utilizatorului și afișează messaging-ul corect (UKGC pentru UK, GGL pentru DE, etc.)
- **Dedicated RG page**: pagină separată "Responsible Gambling" cu toate resursele, auto-assessment tools links, self-exclusion instructions per operator promovat
- **Technical**: RG elements în DOM nu în iframe/lazy-load (trebuie crawlable pentru audit)

### Pas 4: IMPLEMENTARE PE EMAIL ȘI SMS
**Email:**
- Footer obligatoriu cu: 18+ badge, RG text, unsubscribe link, organizație RG per jurisdicție (segment email lists per country)
- Pre-header sau first visible line: include RG mention pe emails promotional
- Frequency cap: nu depăși 3 emails promotional/săptămână (best practice, UKGC guidance)
- Re-engagement emails: include RG message prominently (player inactiv poate fi în recovery)

**SMS (SMSads — critical):**
- Mesaj RG în FIECARE SMS promotional: "18+. Gamble responsibly. BeGambleAware.org" (UK) sau echivalent per jurisdicție
- Character count: aloca minim 40-50 caractere pentru RG messaging din cele 160
- Opt-out: "Reply STOP to unsubscribe" obligatoriu (PECR/GDPR)
- Frequency: max 2 SMS/săptămână per recipient (industry best practice)
- Time restrictions: nu trimite 22:00-08:00 (best practice, unele jurisdicții impun legal)
- Template examples:
  - UK: "[Operator] Bonus: £20 free bet! T&Cs apply. 18+. BeGambleAware.org. Reply STOP to opt out"
  - RO: "[Operator] Bonus: 100 RON! T&C pe site. 18+. jocresponsabil.ro. Răspunde STOP pt dezabonare"
  - DE: NU trimite SMS promotional gambling (restricții severe GGL)
  - KE: "[Operator] Bonus: KES 500! T&Cs apply. 18+. Gamble responsibly. Reply STOP to opt out"

### Pas 5: IMPLEMENTARE PE SOCIAL MEDIA ȘI ADS
**Social media organic:**
- Bio/About: include RG statement și 18+
- Posts promotional: 18+ în text sau imagine, RG hashtag (#GambleResponsibly, #18plus)
- Comments/replies: nu engage cu minori aparenți, nu împinge gambling la complaints about losses
- Platform policies: verifică Meta/Google/TikTok gambling advertising policies (suplimentare față de jurisdicție)

**Paid ads:**
- Google Ads: certificare Google Gambling Ads obligatorie, landing page compliant
- Meta Ads: pre-approval pentru gambling ads, targeting 18+ only, RG messaging
- Display/Programmatic: 18+ badge în creative, RG text, click-to-T&C
- Geo-fencing: exclude jurisdicții unde nu ai compliance (ex: nu afișa ads GGL-targeted fără conformitate completă)

### Pas 6: CONFIGURARE GEO-TARGETING ȘI A/B TESTING
- **Geo-detection**: implementează IP-based geo + browser language detection pentru servirea mesajelor RG corecte
- **Fallback**: dacă geo-detection fails, afișează cel mai strict set de mesaje (UKGC + GGL combined)
- **A/B testing RG**: testează placement (header vs footer vs inline), wording, vizibilitate — optimizează pentru compliance + user experience
- **Audit trail**: loguiește ce messaging a fost servit, cui, când (necesar pentru demonstrarea compliance la audit)
- **Multi-language**: mesaje RG în limba locală (EN/DE/RO/SW pentru Kenya)

### Pas 7: MONITORIZARE ȘI AUDIT CONTINUU
Setează procese de monitorizare ongoing:
- **Monthly self-audit**: verifică random 10% din paginile active → RG messaging present și corect?
- **Quarterly full audit**: verifică toate canalele, toate jurisdicțiile (execută IGC-102 pentru secțiunea RG)
- **Regulatory update monitoring**: abonare la newsletter-ele autorităților (MGA, UKGC, GGL) pentru schimbări de cerințe
- **Competitor benchmarking**: verifică cum implementează top 5 competitori RG messaging (best practices)
- **Incident response**: dacă un regulator contactează despre RG non-compliance → response < 24h, remediere imediată, documentare completă
- **Training**: staff-ul care creează conținut trebuie instruit anual pe RG requirements per jurisdicție
- **KPIs**: RG page visits, self-exclusion referrals, complaint rate, regulatory notices received

---

## 3. Cortex Logging

```json
{
  "text": "RG messaging implementation completed: [N] channels configured across [N] jurisdictions. Geo-targeting: [ACTIVE/PARTIAL]. SMS templates: [N] approved. Audit result: [PASS/CONDITIONAL/FAIL].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "IGC-104",
    "domain": "igaming_compliance",
    "rule_id": "META-H-002",
    "tags": ["igaming", "compliance", "responsible_gambling", "messaging", "RG", "self_exclusion", "advertising"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție

### WHEN
La fiecare execuție a procedurii în context real sau training. Self-audit lunar, full audit trimestrial.

### HOW (violation detection)
- Canal de comunicare fără RG messaging --> violation (critical)
- SMS promotional fără RG text și opt-out --> violation (critical, risc PECR/GDPR fine)
- Geo-targeting inactiv (aceeași mesaje RG pentru toate jurisdicțiile) --> violation
- Pagină de bonus fără T&C vizibile --> violation
- RG page lipsă pe site --> violation
- Mesaje RG într-o limbă diferită de limba jurisdicției --> violation
- SMS trimis în DE cu conținut promotional gambling --> violation (restricții GGL severe)
- Self-audit lunar omis --> violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Eliminarea sau ascunderea mesajelor RG pentru a crește CTR, trimiterea SMS gambling fără RG footer, targeting minori (sub 18) pe orice canal, promovarea gambling ca sursă de venit, folosirea urgency tactics pe bonusuri fără T&C vizibile.

### CONNECT
- META-H-002 --> enforcement FORGE standard
- `procedure-health.json` --> adaugă entry
- Legătură cu IGC-101 (license comparison — cerințe RG per jurisdicție), IGC-102 (affiliate audit — verificarea RG compliance), IGC-103 (transaction monitoring — RG behavioral indicators overlap)

---

## 5. Validation Checklist

- [ ] Channel inventory complete with jurisdiction mapping per channel
- [ ] RG messaging library built with jurisdiction-specific variants (UKGC, MGA, GGL, Curaçao, ONJN, BCLB)
- [ ] Website implementation verified: 18+ badge, RG footer, dedicated RG page, T&C visibility
- [ ] Email templates include RG footer with jurisdiction-segmented content
- [ ] SMS templates include RG text + opt-out within 160 char budget (40-50 chars allocated for RG)
- [ ] Social media bio/about includes RG statement; promotional posts include 18+ and RG hashtag
- [ ] Geo-targeting active with fallback to strictest messaging set (UKGC+GGL combined)
- [ ] Audit trail logs which RG messaging was served, to whom, when
- [ ] Monthly self-audit executed (10% random page sample)
- [ ] Multi-language RG content verified (EN/DE/RO/SW)

## Anti-Patterns

- Removing or hiding RG messaging to increase CTR
- Sending SMS gambling promotions without RG footer and opt-out
- Targeting minors (under 18) on any channel
- Promoting gambling as a source of income
- Using urgency tactics on bonus offers without visible T&C
- Serving identical RG messaging regardless of jurisdiction (geo-targeting absent)
- Sending promotional gambling SMS to German numbers (GGL restrictions)
- Omitting the dedicated RG page from the website

## References

- UKGC Social Responsibility Code of Practice
- MGA Responsible Gambling Guidelines
- GGL Spielerschutz (Player Protection) Requirements
- ONJN OUG 77/2009 — Responsible Gambling Provisions
- BCLB Betting Control and Licensing Act — Advertising Restrictions
- BeGambleAware.org — UK RG Resources
- BZgA Helpline — German RG Resources
- jocresponsabil.ro — Romanian RG Resources
