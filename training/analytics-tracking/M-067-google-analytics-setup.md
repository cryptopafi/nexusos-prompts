---
type: procedure
created: 2026-03-22
status: active
slug: m-067-google-analytics-setup
tags: [procedure, nexus]
---

# Google Analytics Setup and Configuration — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurare completă proprietate GA4 — de la creare până la linking cu Google Ads și Search Console, cu events și conversii active.

---

## 1. Problema

Un site fără Google Analytics configurat corect nu poate măsura traficul, comportamentul utilizatorilor sau conversiile. Instalările incomplete (fără events, fără conversii, fără linking) oferă date inutilizabile pentru decizii de marketing.

Situații acoperite:
- Site nou care necesită analytics de la zero
- Migrare de la Universal Analytics la GA4
- Audit și reconfigurare GA4 instalat incorect

---

## 2. Procedura

### Pas 1: Creează proprietate GA4
Accesează analytics.google.com → Admin → Create Property:
- Completează: nume proprietate, fus orar, monedă
- Selectează tipul de business și obiectivele
- **Nu** activa Universal Analytics (deprecated)

Notează Measurement ID (format: `G-XXXXXXXXXX`).

### Pas 2: Instalează codul de tracking
Alege metoda de instalare:

**Opțiunea A — Google Tag Manager (recomandat)**:
- Creează container GTM dacă nu există
- Adaugă tag GA4 Configuration cu Measurement ID
- Publică containerul GTM

**Opțiunea B — Instalare directă**:
- Copiază Global Site Tag din GA4 → Data Streams → Web
- Lipește în `<head>` al fiecărei pagini

### Pas 3: Configurează Data Streams
În GA4 → Admin → Data Streams → Web:
- Verifică că stream-ul este activ (indicator verde)
- Activează Enhanced Measurement (recomandată): scroll, outbound clicks, site search, video, file downloads
- Verifică că datele vin în Real-Time report după instalare

### Pas 4: Configurează events de bază
GA4 colectează automat cu Enhanced Measurement:
- `page_view` — vizualizare pagină
- `scroll` — scroll 90%
- `click` — click pe outbound links
- `form_submit` — submit formular (dacă detectat automat)

Pentru events custom, configurează în GTM sau prin `gtag('event', ...)` în cod.

### Pas 5: Creează conversion events
Marchează events cheie ca conversii în GA4 → Configure → Events:
- Toggle "Mark as conversion" pentru: `purchase`, `generate_lead`, `sign_up`
- Sau creează conversion event nou din event existent

Verifică în Conversions report că apar date după 24h.

### Pas 6: Linkuiește cu Google Ads
GA4 → Admin → Product Links → Google Ads Linking:
- Conectează contul Google Ads din același Google Account
- Activează Auto-tagging în Google Ads (Settings → Account Settings)
- Importă conversiile GA4 în Google Ads pentru bidding

### Pas 7: Linkuiește cu Search Console
GA4 → Admin → Product Links → Search Console Linking:
- Selectează proprietatea Search Console verificată
- Activează linking

Date din Search Console vor apărea în GA4 → Acquisition → Search Console.

### Pas 8: Configurează rapoarte de bază
Personalizează Reports snapshot:
- Adaugă card Users by channel (Acquisition)
- Adaugă card Top pages (Engagement)
- Adaugă card Conversions by source
- Setează date range default la "Last 28 days"

---

## 3. Cortex Logging

```json
{
  "text": "Google Analytics 4 configurat: proprietate creată, tracking instalat via GTM, Enhanced Measurement activ, events de bază + conversii configurate, linking Google Ads + Search Console realizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-067",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["google-analytics", "ga4", "tracking", "gtm", "conversii", "setup"],
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

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum analytics-tracking
- `procedure-health.json` → adaugă entry la fiecare execuție

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-067 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Analytics Setup and Configuration" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics 4 | Platformă de analytics |
| Google Tag Manager | Management instalare tracking |
| Google Ads | Linking pentru import conversii |
| Google Search Console | Linking pentru date organice |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Tracking activ în Real-Time | Da |
| Conversii configurate | Minimum 1 |
| Linking GA → Google Ads | Activ |
