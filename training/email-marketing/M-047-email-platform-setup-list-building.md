---
type: procedure
created: 2026-03-22
status: active
slug: m-047-email-platform-setup-list-building
tags: [procedure, nexus]
---

# Email Marketing Platform Setup and List Building — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește procesul de configurare tehnică a platformei de email marketing și construire a listei de abonați calificați cu deliverability optimă.

---

## 1. Problema

O platformă de email marketing configurată greșit tehnic (DNS, SPF, DKIM, DMARC) duce la rate de deliverability scăzute și emailuri în spam, anulând orice efort de conținut. Construirea listei fără o strategie de lead magnet și segmentare prealabilă produce o bază de contacte neangajate cu open rate sub 15% și risc crescut de penalizări de la ESP-uri.

Situații acoperite:
- Business care lansează email marketing pentru prima dată sau migrează ESP-ul
- Listă de email construită fără segmentare sau double opt-in, cu metrici slabe
- Probleme de deliverability: emailuri în spam, sender reputation scăzut

---

## 2. Procedura

### Pas 1: Selecție platformă ESP și plan adecvat
Evaluează ESP-ul (Email Service Provider) în funcție de: dimensiunea listei actuale și proiectată 12 luni, funcții necesare (automatizări, segmentare, A/B testing), deliverability reputation ESP-ului (verifică rapoarte ReturnPath/Validity). Opțiuni principale: Mailchimp (beginner, sub 500 contacte gratuit), ActiveCampaign (automatizări avansate), Klaviyo (e-commerce), Brevo/Sendinblue (cost-eficient EU), ConvertKit (creatoare de conținut). Instalează planul care include automatizări de la bun început.

### Pas 2: Configurare tehnică DNS pentru deliverability maximă
Configurează în DNS-ul domeniului tău: SPF record (autorizează ESP-ul să trimită în numele domeniului), DKIM (semnătură criptografică per email), DMARC record (politică pentru emailuri neautorizate: policy=quarantine sau reject). Verifică configurarea cu tools: MXToolbox, Mail-Tester.com (scor minim 9/10), Google Postmaster Tools (reputație domeniu). Fără aceste setări, chiar și emailuri legitime ajung în spam.

### Pas 3: Creare lead magnet de valoare înaltă pentru list building
Designează un lead magnet specific și imediat acționabil: checklist (1 pagină PDF), template (Notion, Google Sheets, Canva), mini-curs (3-5 emailuri), ghid avansat (5-15 pagini), acces la tool/calculator, webinar recording. Lead magnetul trebuie să rezolve o problemă specifică a ICP-ului în 5-10 minute. Testează 2-3 lead magneți simultan în primele 60 zile și rămâi cu cel care convertește cel mai bine (target CR > 30% pe landing page).

### Pas 4: Setup landing page și formular de optin
Creează o landing page dedicată lead magnetului cu: headline clar bazat pe beneficiu (nu feature), bullet points cu ce primesc (3-5 beneficii), formularul de capturare (minim câmpuri: email + prenume), social proof (testimoniale sau număr abonați dacă > 100). Activează double opt-in pentru o listă mai sănătoasă și GDPR compliance. Plasează formularele de sign-up în: footer site, popup exit-intent, blog posts relevante, bio social media.

### Pas 5: Segmentare inițială și tagging la înrolare
Configurează tag-uri și segmente chiar de la început în ESP: sursă de înrolare (lead magnet specific, pagina de vânzări, newsletter, eveniment), interese declarate (dacă formularul include o întrebare de calificare), comportament imediat (a deschis emailul de welcome? a descărcat lead magnetul?). Segmentarea retroactivă pe o listă mare este extrem de dificilă — construiește structura de segmente în ziua zero.

### Pas 6: Email de welcome și secvență de onboarding (primele 7 zile)
Trimite imediat după confirmare: emailul de welcome cu livrarea lead magnetului + setarea așteptărilor (ce vor primi, cât de des, cum se dezabonează). În primele 7 zile, trimite 3-4 emailuri de onboarding care: prezintă brandul și povestea (email 2), livrează valoare suplimentară (email 3), identifică problema principală a abonatului (email 4 cu un survey simplu 1-2 întrebări sau CTA spre articol de diagnostic). Emailurile din primele 7 zile au cele mai mari open rate-uri — folosește-le strategic.

### Pas 7: Monitoring sănătate listă și deliverability ongoing
Monitorizează lunar: open rate (benchmark industrie 20-30%), click-through rate (2-5%), bounce rate (< 2%, > 2% → oprești trimiterea și curăți), unsubscribe rate (< 0.5% per campanie), spam complaints rate (< 0.1% — Google și Yahoo penalizează peste 0.3%). La fiecare 90 zile: campanie de re-engagement pentru abonații inactivi (+90 zile fără deschidere) și eliminarea celor care nu răspund. O listă mică de calitate performează mai bine decât una mare neangajată.

---

## 3. Cortex Logging

```json
{
  "text": "Email Platform Setup & List Building executat: ESP configurat tehnic, lead magnet creat, segmentare inițială activă.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-047",
    "domain": "email-marketing",
    "rule_id": "META-H-002",
    "tags": ["email-marketing", "list-building", "deliverability", "esp-setup", "spf-dkim-dmarc", "lead-magnet"],
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
- Email marketing lansat fără SPF/DKIM/DMARC configurate → violation
- Listă construită fără double opt-in (risc GDPR + deliverability) → violation
- Segmentare lipsă la înrolare (toți abonații într-o singură listă nesegmentată) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum email-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] SPF, DKIM și DMARC verificate cu Mail-Tester.com (scor > 9/10)?
- [ ] Double opt-in activat și secvență de welcome configurată?

**VK-uri obligatorii:**
1. `✅ [PROC] M-047 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Email Marketing Platform Setup and List Building" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ESP (Mailchimp / ActiveCampaign / Klaviyo) | Platformă principală trimitere email |
| DNS Manager (Cloudflare, GoDaddy etc.) | Configurare SPF / DKIM / DMARC |
| MXToolbox / Mail-Tester.com | Verificare deliverability tehnică |
| Google Postmaster Tools | Monitorizare reputație domeniu la Gmail |
| Canva / Notion | Creare lead magnet (PDF, template) |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Mail-Tester.com scor | > 9/10 |
| Open rate newsletter | > 25% |
| Bounce rate | < 2% |
| Spam complaints rate | < 0.1% |
