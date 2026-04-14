---
type: procedure
created: 2026-03-22
status: active
slug: m-028-google-ads-extensions
tags: [procedure, nexus]
---

# Google Ads Extension (Assets) Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea, optimizarea și monitorizarea completă a tuturor tipurilor de Assets (Extensions) Google Ads pentru maximizarea CTR și calității anunțului.

---

## 1. Problema

Assets (fostele Extensions) sunt ignorate de majoritatea advertiserilor, deși Google le acordă o pondere semnificativă în Ad Rank și pot crește CTR-ul cu 10-30% fără cost suplimentar. Un cont Google Ads fără assets complete pierde constant spațiu în SERP față de competitori. Această procedură asigură că fiecare tip relevant de asset este configurat corect și optimizat continuu.

Situații acoperite:
- Setup complet assets pentru o campanie sau cont nou fără extensii configurate
- Audit și completare assets pentru un cont existent cu assets parțiale
- Optimizarea assets pe baza performance data (assets cu CTR scăzut sau neafișate)

---

## 2. Procedura

### Pas 1: Auditează assets existente și identifică ce lipsește
În Google Ads, navighează la Ads & Assets → Assets. Verifică ce tipuri de assets sunt active și care lipsesc. Prioritatea de setup: Sitelinks → Callouts → Structured Snippets → Call → Lead Form → Promotion → Price → Image. Notează pentru fiecare asset existent: status (Eligible/Limited/Disapproved), performanța (CTR per asset).

### Pas 2: Configurează Sitelinks (minimum 4, recomand 8+)
Sitelinks sunt cel mai impactant tip de asset — ocupă cel mai mult spațiu vizual în SERP. Adaugă minimum 4 sitelinks cu: titlu descriptiv (25 char), 2 rânduri de descriere (35 char fiecare). Exemple: "Servicii Complete", "Prețuri & Pachete", "Portofoliu Clienți", "Contact Rapid". Evită sitelinks generice ("Acasă", "Despre noi"). Fiecare sitelink trebuie să ducă la o pagină distinctă cu conținut relevant.

### Pas 3: Adaugă Callouts (minimum 4, ideal 8-10)
Callouts sunt texte scurte (25 char) care evidențiază USP-uri. Exemple: "Livrare în 24h", "Fără costuri ascunse", "Garanție 2 ani", "Suport 24/7", "Certificat ISO". Scrie callouts la nivel de cont pentru mesaje universale și la nivel de campanie/ad group pentru mesaje specifice. Nu repeta același mesaj din headline — callouts trebuie să adauge informație nouă.

### Pas 4: Configurează Structured Snippets și Call Assets
Structured Snippets: alege header relevant (Servicii, Produse, Mărci, Tipuri, Stiluri) și adaugă minimum 4 valori. Exemplu: Header "Servicii" → "Audit SEO", "Campanii Google Ads", "Facebook Ads", "Optimizare Rate Conversie". Call Asset: adaugă numărul de telefon cu scheduling setat DOAR în orele de program (nu afișa numărul când nu ești disponibil). Activează Call Reporting pentru a măsura apelurile.

### Pas 5: Adaugă Image Assets și Price Assets (dacă aplicabil)
Image Assets: adaugă minimum 3 imagini de calitate (landscape 1.91:1 și square 1:1). Imaginile trebuie să fie relevante pentru produs/serviciu — nu imagini stock generice. Price Assets: pentru produse sau servicii cu prețuri clare, adaugă Price Assets (minimum 3 prețuri cu link direct la pagina produsului). Price Assets cresc intenția de cumpărare filtrând click-urile necalificate.

### Pas 6: Configurează Lead Form Assets (dacă obiectivul este lead gen)
Lead Form Assets permit colectarea de lead-uri direct din anunț fără a vizita site-ul. Configurează: formular scurt (maximum 4 câmpuri), mesaj de confirmare clar, webhook sau integrare CRM pentru trimiterea automată a lead-urilor. Testează că lead-urile ajung în CRM în mai puțin de 5 minute de la completare. Lead Form Assets funcționează deosebit de bine pe mobile.

### Pas 7: Monitorizează performance per asset și înlocuiește assets slabe
Lunar, verifică "Asset details" pentru a vedea performanța fiecărui asset individual (afișări, clicks, CTR). Assets cu rating "Low" (afișate rar de Google pentru că nu performează) trebuie înlocuite cu variante noi. Testează minimum 2 variante pentru fiecare tip de asset. Documentează în Cortex: tipuri de assets active, CTR per tip, assets înlocuite și rezultatele.

---

## 3. Cortex Logging

```json
{
  "text": "M-028 executat: Assets Google Ads configurate/optimizate. Cont/Campanie: [Nume]. Tipuri assets active: [listă]. Sitelinks: [N]. Callouts: [N]. Assets cu rating Low înlocuite: [N]. CTR mediu cu assets vs. fără: [%]. Lead Form assets: DA/NU.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-028",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "assets", "extensions", "sitelinks", "callouts", "lead-form", "ctr-optimization", "ad-rank"],
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
- Cont activ cu mai puțin de 4 tipuri de assets configurate → violation
- Call Asset activ fără scheduling setat (afișat 24/7 fără disponibilitate reală) → violation
- Assets cu rating "Low" lăsate active fără înlocuire după 30 de zile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 4 tipuri de assets active per campanie?
- [ ] Call Reporting activat pentru Call Assets?

**VK-uri obligatorii:**
1. `✅ [PROC] M-028 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Ads Extension (Assets) Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Ads Account | Platformă principală |
| CRM / CMS | Destinații pentru sitelinks și Price assets |
| Call Tracking System | Măsurarea apelurilor din Call Assets |
| Lead Webhook / Zapier | Integrare Lead Form Assets cu CRM |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CTR improvement față de baseline (fără assets) | Minim +15% |
| Tipuri assets active per campanie | Minimum 6 tipuri |
| Assets cu rating "Low" | 0 lăsate active peste 30 zile |
