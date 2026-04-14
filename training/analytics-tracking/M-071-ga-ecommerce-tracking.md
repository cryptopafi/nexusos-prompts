---
type: procedure
created: 2026-03-22
status: active
slug: m-071-ga-ecommerce-tracking
tags: [procedure, nexus]
---

# Google Analytics E-commerce Tracking — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementare completă e-commerce tracking în GA4 — de la activarea rapoartelor până la configurarea funnel-ului de checkout și monitorizarea performanței produselor.

---

## 1. Problema

Un magazin online fără e-commerce tracking în GA4 nu poate măsura venituri reale, rata de conversie a produselor sau drop-off-ul în funnel-ul de cumpărare. Deciziile de produs și marketing sunt luate fără date despre ce produse convertesc și unde pierd clienții.

Situații acoperite:
- Magazin WooCommerce sau Shopify care necesită tracking complet
- Audit e-commerce tracking pentru GA4
- Setup funnel de checkout pentru identificarea drop-off-ului

---

## 2. Procedura

### Pas 1: Activează e-commerce reporting în GA4
Mergi la GA4 → Admin → Reporting Identity → și verifică că Blended sau Observed este activ. Apoi:
- Admin → Data Display → E-commerce Setup → Enable e-commerce reporting (toggle On)

Pentru WooCommerce: instalează plugin **GTM4WP** sau **Pixel Caffeine** care trimite automat events GA4.
Pentru Shopify: adaugă GA4 din Sales Channels → Online Store → Preferences.

### Pas 2: Implementează evenimentul purchase cu parametri completi
Event-ul `purchase` este cel mai important. Trebuie să includă:
```javascript
gtag('event', 'purchase', {
  transaction_id: 'T12345',
  value: 149.99,
  currency: 'RON',
  items: [{
    item_id: 'SKU_001',
    item_name: 'Produs X',
    price: 149.99,
    quantity: 1
  }]
});
```
Verifică că `transaction_id` este unic per comandă pentru a evita duplicarea veniturilor.

### Pas 3: Adaugă events pentru funnel-ul de cumpărare
Implementează events în ordine pentru funnel complet:
- `view_item` — utilizatorul vede pagina produsului
- `add_to_cart` — adaugă în coș
- `begin_checkout` — inițiază checkout
- `add_payment_info` — completează datele de plată
- `purchase` — comandă finalizată

Fiecare event trebuie să includă parametrul `items` cu detaliile produsului.

### Pas 4: Configurează enhanced e-commerce items
Adaugă parametri suplimentari pentru analiză mai detaliată:
- `item_category` — categoria produsului
- `item_brand` — brandul
- `item_variant` — varianta (culoare/mărime)
- `item_list_name` — sursa (pagina categorie / search / recomandare)

Acești parametri permit rapoarte de performanță per categorie și brand.

### Pas 5: Configurează Funnel Exploration Report
În GA4 → Explore → Funnel Exploration:
- Creează funnel cu pașii: view_item → add_to_cart → begin_checkout → purchase
- Setează tipul: Open funnel (permite intrarea la orice pas)
- Activează "Show elapsed time" pentru a vedea durata între pași
- Salvează explorationul ca raport recurent

### Pas 6: Calculează și monitorizează ratele de conversie ale funnel-ului
Calculează drop-off rate pentru fiecare pas:
- `view_item → add_to_cart`: benchmark 5-15%
- `add_to_cart → begin_checkout`: benchmark 50-70%
- `begin_checkout → purchase`: benchmark 60-80%

Orice pas sub benchmark este prioritate pentru optimizare.

### Pas 7: Monitorizează performanța produselor
În GA4 → Reports → Monetization → E-commerce Purchases:
- Identifică produsele cu cel mai mare venit total
- Identifică produsele cu cel mai mare număr de vizualizări dar rate de conversie mică (oportunitate)
- Identifică produsele adăugate frecvent în coș dar rareori cumpărate (friction în checkout)

---

## 3. Cortex Logging

```json
{
  "text": "GA4 E-commerce Tracking configurat: reporting activat, purchase event cu parametri completi implementat, funnel events adăugate (view_item→purchase), enhanced items configurat, Funnel Exploration creat, performanță produse monitorizată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-071",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["ga4", "ecommerce", "tracking", "funnel", "purchase", "conversii"],
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
1. `✅ [PROC] M-071 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Analytics E-commerce Tracking" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics 4 | Platformă analytics e-commerce |
| Google Tag Manager | Implementare events tracking |
| WooCommerce / Shopify | Platformă e-commerce sursă |
| GTM4WP / Pixel Caffeine | Plugin WooCommerce → GA4 |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| purchase event activ și validat | Da |
| Funnel Exploration creat | Da |
| Drop-off rate monitorizat | Per pas funnel |
