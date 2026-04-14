---
type: procedure
created: 2026-03-22
status: active
slug: m-068-google-search-console-setup
tags: [procedure, nexus]
---

# Google Search Console Setup and Monitoring — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurare și monitorizare Google Search Console — de la verificare proprietate până la monitorizarea Core Web Vitals și alertarea automată pentru erori de indexare.

---

## 1. Problema

Fără Google Search Console configurat, nu există vizibilitate asupra performanței organice (clicks, impressions, CTR, poziții), erorilor de indexare sau problemelor Core Web Vitals. Site-urile cu GSC neconfirmat nu pot detecta penalizările Google sau paginile dezindexate.

Situații acoperite:
- Site nou care necesită monitorizare SEO
- Audit SEO care necesită acces la date de performance organică
- Detectarea și rezolvarea problemelor de indexare

---

## 2. Procedura

### Pas 1: Adaugă proprietatea în Search Console
Accesează search.google.com/search-console → Add Property:

**Domain property (recomandat)** — acoperă toate subdomain-urile și protocoalele:
- Introdu domeniu fără www sau https (ex: `domeniu.ro`)
- Verifică prin DNS TXT record

**URL-prefix property** — pentru verificare rapidă:
- Introdu URL complet (`https://www.domeniu.ro/`)
- Verifică prin HTML tag, fișier HTML sau GA

### Pas 2: Verifică proprietatea (ownership)
Alege metoda de verificare:
- **HTML tag** — adaugă `<meta name="google-site-verification">` în `<head>`
- **DNS TXT record** — adaugă record în panoul DNS (verificare domain property)
- **Google Analytics** — automat dacă GA este instalat cu același cont Google
- **GTM** — automat dacă GTM este publicat cu snippet pe site

Confirmă verificarea — GSC va afișa "Ownership verified".

### Pas 3: Trimite sitemap.xml
Mergi la GSC → Sitemaps → Add a new sitemap:
- Introdu URL-ul sitemap-ului (ex: `sitemap.xml` sau `sitemap_index.xml`)
- Apasă Submit
- Verifică că status este "Success" și numărul de URL-uri descoperite este corect

Generează sitemap cu Yoast SEO (WordPress) sau plugin dedicat dacă nu există.

### Pas 4: Verifică Coverage report pentru erori de indexare
Mergi la Index → Coverage:
- **Error** (roșu) — pagini cu probleme care blochează indexarea → rezolvă urgent
- **Valid with warnings** (portocaliu) — pagini indexate cu probleme minore → investighează
- **Valid** (verde) — pagini indexate corect
- **Excluded** (gri) — pagini excluse intenționat (noindex, canonical) → verifică că excluderile sunt corecte

Documentează erorile găsite și planifică rezolvarea.

### Pas 5: Analizează Performance report
Mergi la Performance → Search results. Setează date range la "Last 3 months":
- **Clicks** — trafic organic total
- **Impressions** — de câte ori apare site-ul în rezultate
- **CTR** — procentul care dă click (benchmark: 2-5% pentru poziții 5-10)
- **Average position** — poziția medie pentru toate keyword-urile

Identifică: keyword-uri cu impressions mari dar CTR mic (oportunitate de optimizare titlu/meta).

### Pas 6: Configurează alerte email pentru erori de acoperire
GSC → Settings → Email preferences:
- Activează notificările pentru: Coverage errors, Manual actions, Security issues
- Frecvență: imediat sau săptămânal

Adaugă adresa de email a echipei SEO pentru notificări automate.

### Pas 7: Monitorizează Core Web Vitals
Mergi la Experience → Core Web Vitals:
- **LCP** (Largest Contentful Paint) — target: sub 2.5s
- **FID/INP** (Interaction to Next Paint) — target: sub 200ms
- **CLS** (Cumulative Layout Shift) — target: sub 0.1

Paginile cu status "Poor" afectează ranking. Prioritizează fix-urile pentru paginile cu cel mai mult trafic organic.

---

## 3. Cortex Logging

```json
{
  "text": "Google Search Console configurat: proprietate adăugată și verificată, sitemap trimis, Coverage report verificat, Performance report analizat, alerte email activate, Core Web Vitals monitorizate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-068",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["search-console", "gsc", "seo", "indexare", "core-web-vitals", "sitemap"],
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
1. `✅ [PROC] M-068 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Search Console Setup and Monitoring" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Search Console | Platformă monitorizare organică |
| Yoast SEO / plugin sitemap | Generare sitemap.xml |
| DNS provider | Verificare prin TXT record |
| Google Analytics 4 | Verificare alternativă + linking |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Proprietate verificată | Da |
| Sitemap trimis cu success | Da |
| Alerte email activate | Da |
| Core Web Vitals monitorizate | Da |
