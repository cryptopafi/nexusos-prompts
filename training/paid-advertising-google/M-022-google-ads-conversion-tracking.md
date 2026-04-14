---
type: procedure
created: 2026-03-22
status: active
slug: m-022-google-ads-conversion-tracking
tags: [procedure, nexus]
---

# Google Ads Conversion Tracking Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea și validarea completă a conversion tracking în Google Ads pentru a măsura corect acțiunile valoroase ale utilizatorilor.

---

## 1. Problema

Fără conversion tracking configurat corect, Google Ads nu știe ce funcționează și cheltuiești buget pe clicks fără conversii. Smart Bidding devine inutilizabil, rapoartele sunt înșelătoare, iar optimizarea este imposibilă. Această procedură asigură că fiecare conversie valoroasă este măsurată precis înainte de lansarea oricărei campanii.

Situații acoperite:
- Setup conversion tracking pentru cont nou fără nicio măsurare activă
- Adăugarea unui nou tip de conversie (ex: formular + telefon + chat)
- Diagnosticarea și repararea unui conversion tracking defect sau care înregistrează dublu

---

## 2. Procedura

### Pas 1: Identifică toate acțiunile de conversie relevante
Listează toate acțiunile pe care utilizatorul le poate face pe site și care au valoare pentru business: completare formular, apel telefonic, cumpărătură, adăugare în coș, vizitare pagină de mulțumire, chat inițiat. Prioritizează-le ca Primary (contorizate în Smart Bidding) vs. Secondary (doar raportare). Nu configura mai mult de 3 conversii Primary simultan.

### Pas 2: Configurează Google Tag (global site tag) pe site
Instalează Google Tag (gtag.js) pe toate paginile site-ului, de preferință prin Google Tag Manager. Verifică că tag-ul se încarcă corect folosind Google Tag Assistant (extensia Chrome). Dacă folosești GTM, creează un container dedicat și publică tag-ul înainte de a continua.

### Pas 3: Creează conversion actions în Google Ads
În Google Ads → Tools → Conversions → New Conversion Action. Pentru fiecare acțiune identificată la Pas 1: alege categoria corectă (Purchase, Lead, Page View etc.), setează valoarea (fixă sau dinamică), setează Count la "One" pentru leads și "Every" pentru purchases, configurează Attribution Model (recomandat: Data-driven sau Linear).

### Pas 4: Implementează conversion tags prin GTM
În Google Tag Manager, creează câte un tag de tip "Google Ads Conversion Tracking" pentru fiecare conversie. Configurează trigger-ul corespunzător (ex: Page View pe /thank-you, Click pe buton formular, Timer pentru call duration). Testează în GTM Preview Mode că tag-ul se declanșează în condițiile corecte.

### Pas 5: Validează cu Google Tag Assistant și Test Conversion Tool
Folosește extensia Google Tag Assistant pentru a confirma că tag-ul se declanșează pe paginile corecte. Folosește instrumentul "Test conversion" din Google Ads (Tools → Conversions → tag-ul respectiv → Diagnose) pentru a verifica că Google Ads primește datele. Statusul trebuie să fie "Receiving data" în maximum 24h.

### Pas 6: Configurează Enhanced Conversions (dacă aplicabil)
Dacă site-ul colectează email/telefon/adresă la conversie, activează Enhanced Conversions pentru o acuratețe mai mare a atribuirii. În Google Ads → Conversions → Enhanced conversions → Setup cu GTM sau cod direct. Validează că hash-ul datelor se trimite corect prin Tag Assistant.

### Pas 7: Testează end-to-end și documentează
Efectuează o conversie reală de test (completează formularul, cumpără un produs test). Verifică în Google Ads că conversia apare în raportul "All conversions" în maximum 3h. Verifică că nu există dubluri de conversie (același eveniment înregistrat de 2 ori). Documentează în Cortex: tipuri de conversii configurate, metode de implementare, statusul validării.

---

## 3. Cortex Logging

```json
{
  "text": "M-022 executat: Conversion tracking configurat pentru [domeniu]. Conversii Primary: [N] - [tipuri]. Conversii Secondary: [N]. Implementare: GTM/cod direct. Enhanced Conversions: DA/NU. Status validare: PASS/FAIL. Test conversion: confirmat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-022",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "conversion-tracking", "gtm", "google-tag", "enhanced-conversions", "attribution"],
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
- Conversion tracking nevalidat prin Test Conversion Tool → violation
- Mai mult de 3 conversii Primary configurate simultan → violation
- Conversion tag implementat fără testare în GTM Preview Mode → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Status "Receiving data" confirmat în Google Ads?
- [ ] Test conversie reală efectuat și verificat în rapoarte?

**VK-uri obligatorii:**
1. `✅ [PROC] M-022 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Ads Conversion Tracking Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Tag Manager | Implementare și management tags |
| Google Tag Assistant | Validare implementare tag |
| Site/CMS | Instalare tag global și pagini de conversie |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Conversion tracking accuracy | 0% discrepanță față de CRM la 7 zile |
| Status validare | "Receiving data" în 24h de la implementare |
| Conversii Primary | Maximum 3 simultane per cont |
