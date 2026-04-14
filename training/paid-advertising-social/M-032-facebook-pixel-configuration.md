---
type: procedure
created: 2026-03-22
status: active
slug: m-032-facebook-pixel-configuration
tags: [procedure, nexus]
---

# Facebook Pixel Configuration and Custom Events — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Instalarea, configurarea și verificarea Facebook Pixel cu evenimente standard și custom pentru tracking complet al conversiilor.

---

## 1. Problema

Un pixel instalat incorect sau neverificat face imposibilă optimizarea campaniilor pentru conversii și construirea audiențelor de retargeting. Fără evenimente custom corecte, algoritmul Facebook nu are semnalele necesare pentru a identifica și a scala audiențele profitabile.

Situații acoperite:
- Instalare pixel pe site nou fără tracking activ
- Reconfigurare pixel cu evenimente custom pentru funnel specific
- Verificare și debug pixel existent cu date incorecte sau lipsă

---

## 2. Procedura

### Pas 1: Creare Pixel în Events Manager
Accesează Events Manager în Business Manager și creează un Pixel nou cu un nume descriptiv (ex: "ClientNume - Main Pixel"). Notează Pixel ID-ul. Dacă există deja un pixel, verifică că este asociat Ad Account-ului corect și că nu există pixeli duplicați activi pe același site.

### Pas 2: Instalare cod pixel pe site
Alege metoda de instalare potrivită: manual (adaugă codul în `<head>` pe toate paginile), prin partener (Shopify, WordPress/PixelYourSite, WooCommerce), sau prin Google Tag Manager (recomandată pentru flexibilitate). Instalează codul de bază pe toate paginile, nu doar pe pagina de confirmare. Verifică că pixel-ul nu este blocat de Consent Mode sau cookie banner fără fallback.

### Pas 3: Configurare Conversions API (CAPI)
Implementează Conversions API server-side pentru a complementa pixel-ul browser-side și a recupera semnalele pierdute din cauza ad blockerelor sau iOS 14+. Conectează CAPI prin partenerul de platformă (Shopify, WooCommerce) sau prin integrator direct. Setează `event_id` identic între pixel și CAPI pentru deduplicare automată.

### Pas 4: Instalare evenimente standard
Implementează evenimentele standard relevante pentru business: `PageView` (toate paginile), `ViewContent` (pagini produs/serviciu), `AddToCart` (coș), `InitiateCheckout` (start checkout), `Purchase` (confirmare comandă cu `value` și `currency`). Pentru lead generation: `Lead` și `CompleteRegistration`. Fiecare eveniment trebuie să trimită parametrii corecți (content_ids, content_type, value).

### Pas 5: Creare Custom Events și Custom Conversions
Definește Custom Events pentru acțiuni specifice businessului care nu sunt acoperite de evenimentele standard (ex: `VideoPlayed30s`, `QuizCompleted`, `PhoneNumberClicked`). Creează Custom Conversions în Events Manager pentru a filtra evenimentele standard pe baza URL-ului (ex: Purchase pe pagina de confirmare cu valoare minimă). Setează fereastra de atribuire la 7-day click, 1-day view.

### Pas 6: Verificare cu Pixel Helper și Events Manager
Instalează extensia Meta Pixel Helper în Chrome și vizitează fiecare pagină cheie a site-ului. Verifică că fiecare eveniment se declanșează corect, fără duplicate și cu parametrii corecți. În Events Manager > Test Events, trimite trafic test și confirmă că toate evenimentele apar în timp real. Verifică că Purchase trimite `value` și `currency` corecte.

### Pas 7: Configurare agregare evenimente și prioritizare (iOS 14+)
În Events Manager > Aggregated Event Measurement, verifică domeniul și configurează maximum 8 evenimente prioritizate în ordinea importanței (Purchase = 1, Lead = 2, etc.). Această configurare este obligatorie pentru livrarea reclamelor pe dispozitive iOS 14+. Documentează configurația finală și testează o campanie test cu obiectivul Conversions pentru a confirma că pixelul primește date.

---

## 3. Cortex Logging

```json
{
  "text": "Facebook Pixel configurat complet — pixel instalat, CAPI activ, evenimente standard și custom verificate în Events Manager, Aggregated Event Measurement configurat pentru iOS 14+.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-032",
    "domain": "paid-advertising-social",
    "rule_id": "META-H-002",
    "tags": ["facebook", "pixel", "capi", "tracking", "conversions", "events"],
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
- Pixel instalat fără verificare în Events Manager → violation
- CAPI absent fără justificare documentată → violation
- Aggregated Event Measurement neconfigurat pentru conturi cu campanii iOS → violation
- Purchase event fără parametrii `value` și `currency` → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-social

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Pixel Helper confirmă toate evenimentele fără erori?
- [ ] Aggregated Event Measurement configurat cu minim 4 evenimente prioritizate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-032 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook Pixel Configuration and Custom Events" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Events Manager | Dashboard central pentru pixel și CAPI |
| Meta Pixel Helper (Chrome Extension) | Verificare și debug pixel în browser |
| Conversions API (CAPI) | Tracking server-side pentru semnale iOS-proof |
| Google Tag Manager | Deployment flexibil al codului pixel |
| Aggregated Event Measurement | Compatibilitate iOS 14+ |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Evenimente active fără erori | 100% |
| Rata de deduplicare CAPI+Pixel | < 10% duplicate |
| Event Match Quality Score | Minim 6.0/10 |
| Acoperire PageView | 100% pagini site |
