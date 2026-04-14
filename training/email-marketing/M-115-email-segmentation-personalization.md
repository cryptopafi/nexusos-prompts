---
type: procedure
created: 2026-03-22
status: active
slug: m-115-email-segmentation-personalization
tags: [procedure, nexus]
---

# Email Segmentation and Personalization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Segmentarea listei de email și personalizarea mesajelor pentru relevanță maximă și conversii superioare.

---

## 1. Problema

Trimiterea aceluiași email întregii liste generează rate de deschidere scăzute, unsubscribe ridicat și venituri sub potențial. Fără segmentare și personalizare, mesajele devin irelevante pentru segmente mari ale listei. Procedura definește un sistem repetabil de segmentare și personalizare care crește relevanța, engagement-ul și conversiile.

Situații acoperite:
- Configurarea inițială a segmentelor pentru o listă existentă
- Personalizarea unei campanii email pentru un segment specific
- Reactivarea unui segment inactiv prin mesaje personalizate

---

## 2. Procedura

### Pas 1: Auditul listei existente și colectarea datelor
Analizează datele disponibile în platforma de email: comportament de deschidere/click, date demografice, sursa de opt-in, istoricul achizițiilor, activitate pe site (dacă tracking este activ). Identifică câmpurile de date lipsă care ar îmbunătăți segmentarea și planifică colectarea lor progresivă.

### Pas 2: Definirea segmentelor primare
Creează minimum 4 segmente de bază: (1) Activi recenți — au deschis/clickat în ultimele 30 zile; (2) Activi în adormire — au interacționat în ultimele 31-90 zile; (3) Inactivi — fără interacțiune 91-180 zile; (4) Pierduti — fără interacțiune peste 180 zile. Adaugă segmente comportamentale bazate pe interese/achiziții dacă datele permit.

### Pas 3: Validarea și curățarea listei înainte de segmentare
Verifică lista pentru: adrese invalide sau bounced (elimină hard bounces imediat), adrese duplicat, subscriberi fără confirmare (dacă double opt-in activ). Nu porni campania pe o listă necurățată — validarea este obligatorie. Folosește un serviciu de validare dacă lista nu a fost curățată în ultimele 90 zile.

### Pas 4: Crearea personalizării pe segmente
Pentru fiecare segment, adaptează: subject line (tonul și urgența diferă), greeting (prenume + context relevant), corp email (mesaj aliniat cu stadiul din customer journey), CTA (oferta potrivită stadiului). Activii recenți primesc oferte directe; inactivii primesc re-engagement cu valoare înainte de ofertă.

### Pas 5: Configurarea câmpurilor dinamice și merge tags
Implementează personalizare dinamică: {prenume} în subject și salut, conținut condiționat bazat pe tag-uri/segmente (ex: bloc diferit pentru clienți vs. prospecți), recomandări bazate pe istoricul achizițiilor unde platforma permite. Testează că toate merge tags se randează corect înainte de trimitere.

### Pas 6: Testarea A/B pe segmentul cel mai mare
Pe segmentul activ (cel mai mare), rulează A/B test pe: subject line (variabilă principală), sau personalizare vs. fără personalizare în corp. Alocă 20% din segment pentru test, trimite câștigătorul la 80% rămase după 4 ore. Nu trimite fără minimum un A/B test pe subject line.

### Pas 7: Trimiterea, monitorizarea și iterarea segmentelor
Trimite campaniile segmentate cu interval de 30-60 minute între segmente (nu simultan). Monitorizează în primele 4 ore: rata de deschidere, CTR, unsubscribes, spam reports. Documentează performanța fiecărui segment. Actualizează segmentele după fiecare campanie cu noile date comportamentale.

---

## 3. Cortex Logging

```json
{
  "text": "Segmentare și personalizare email executată conform M-115: lista validată, segmente definite, personalizare aplicată, A/B test rulat, campanie trimisă și monitorizată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-115",
    "domain": "email-marketing",
    "rule_id": "META-H-002",
    "tags": ["segmentation", "personalization", "email-marketing", "list-management", "a-b-testing"],
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
- Email trimis fără test pe subject line (Pas 6 sărit) → violation
- Segmentare realizată fără validarea listei (Pas 3 sărit) → violation
- Campanie trimisă simultan tuturor segmentelor fără interval → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum email-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Lista validată și curățată înainte de trimitere?
- [ ] A/B test pe subject line rulat pe segmentul activ?

**VK-uri obligatorii:**
1. `✅ [PROC] M-115 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Email Segmentation and Personalization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Email platform (ActiveCampaign/Klaviyo/etc.) | Segmentare, automație, A/B testing |
| Serviciu validare email (NeverBounce/ZeroBounce) | Curățarea listei |
| CRM / eCommerce platform | Date comportamentale și achiziții |
| Google Analytics / pixel tracking | Comportament pe site pentru segmentare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rata de deschidere segment activ | ≥ 30% |
| CTR segment activ | ≥ 3% |
| Unsubscribe rate per campanie | ≤ 0.5% |
