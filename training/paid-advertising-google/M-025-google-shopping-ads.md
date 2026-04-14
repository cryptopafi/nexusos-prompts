---
type: procedure
created: 2026-03-22
status: active
slug: m-025-google-shopping-ads
tags: [procedure, nexus]
---

# Google Shopping Ads Campaign — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea campaniei Google Shopping Ads de la Google Merchant Center până la prima campanie activă cu feed optimizat.

---

## 1. Problema

Google Shopping Ads este cel mai performant canal pentru e-commerce, dar o campanie prost configurată — cu feed de produse incomplet, titluri nerelevante sau fără segmentare — consumă buget masiv cu ROAS scăzut. Spre deosebire de Search, în Shopping nu controlezi direct keywords ci optimizezi feed-ul și structura campaniei. Această procedură acoperă întreg procesul de la setup Merchant Center la campanie optimizată.

Situații acoperite:
- Setup complet Google Merchant Center și prima campanie Shopping pentru un magazin online nou
- Migrarea unui feed de produse existent cu performanță slabă
- Segmentarea campaniei Shopping pe categorii de produse sau margini de profit

---

## 2. Procedura

### Pas 1: Configurează și verifică Google Merchant Center
Creează sau verifică contul Google Merchant Center (merchant.google.com). Verifică domeniu-ul site-ului (HTML tag, Google Analytics sau DNS). Conectează contul Merchant Center cu contul Google Ads prin linking. Dacă magazinul este pe Shopify sau WooCommerce, instalează plugin-ul oficial Google Channel pentru sincronizare automată.

### Pas 2: Creează și optimizează feed-ul de produse
Feed-ul este cel mai important element în Shopping. Câmpuri obligatorii: id, title, description, link, image_link, price, availability, condition, brand, gtin (sau mpn). Titlurile produselor trebuie să conțină: Brand + Tip produs + Atribute cheie (culoare, dimensiune, model) — maxim 150 caractere. Evită titluri generice ("Pantofi sport roșii" → "Adidas Ultraboost 22 Bărbați Roșu Mărimea 43").

### Pas 3: Setează reguli de feed și corectează erorile Merchant Center
În Merchant Center, verifică Diagnostics → Feed errors. Rezolvă toate erorile critice (produse respinse) înainte de lansarea campaniei. Setează Feed Rules pentru a normaliza automat câmpurile: mapare categorii Google Product Taxonomy, adăugare automată brand dacă lipsește, formatare prețuri. Target: 0 produse respinse, minimum 90% produse aprobate.

### Pas 4: Structurează campania Shopping cu segmentare pe categorii
Creează campania de tip Shopping (Standard sau PMax). Dacă folosești Standard Shopping, segmentează produsele în Ad Groups separate pe categorii principale (nu lăsa "All products" fără subdiviziune). Subdivide după: Product Category → Brand → Product ID pentru produsele cu volum mare. Setează bids mai mari pentru categorii cu marjă de profit ridicată.

### Pas 5: Configurează negative keywords pentru Shopping
Shopping Ads nu folosesc keyword targeting direct, dar negative keywords sunt esențiale. Adaugă negative keywords pentru: termeni de brand competitor (dacă nu vrei să apari pentru ei), termeni generic fără intenție de cumpărare ("gratuit", "second hand", "review"), termeni irelevanți din search terms report (după primele 7 zile). Verifică Search Terms Report zilnic în primele 2 săptămâni.

### Pas 6: Configurează Custom Labels pentru segmentare avansată
Adaugă Custom Labels (0-4) în feed pentru a segmenta după criterii business: bestsellers, produse cu marjă mare, produse sezoniere, clearance. Acest lucru permite setarea de bids diferite pentru grupuri strategice fără a crea campanii separate. Exemplu: Custom Label 0 = "high-margin", Custom Label 1 = "bestseller".

### Pas 7: Activează Merchant Promotions și Shopping Ratings (opțional dar recomandat)
Adaugă Merchant Promotions pentru a afișa ofertele speciale direct în Shopping ads ("20% reducere", "Transport gratuit"). Activează Product Ratings dacă ai reviews — cresc CTR-ul cu 15-20%. Monitorizează ROAS la 7, 14 și 30 de zile. Target: ROAS minim 300% în primele 30 de zile. Documentează în Cortex: nr produse active, ROAS initial, categorii configurate.

---

## 3. Cortex Logging

```json
{
  "text": "M-025 executat: Campanie Google Shopping configurată. Merchant Center: verificat. Produse active în feed: [N]. Produse respinse: [N]. Segmentare campanie: [categorii]. Custom Labels: [N configurate]. ROAS target: [%]. Merchant Promotions: DA/NU.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-025",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "shopping-ads", "merchant-center", "product-feed", "roas", "custom-labels", "ecommerce"],
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
- Campanie lansată cu feed-ul conținând erori critice (produse respinse peste 10%) → violation
- Campanie Shopping fără nicio subdivizare a produselor (rulează pe "All products" fără segmentare) → violation
- Titluri de produse în feed fără Brand și atribute cheie → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 90% produse aprobate în Merchant Center?
- [ ] Search Terms Report setat pentru review zilnic în primele 14 zile?

**VK-uri obligatorii:**
1. `✅ [PROC] M-025 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Shopping Ads Campaign" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Merchant Center | Feed produse și eligibilitate Shopping |
| CMS / Shopify / WooCommerce | Sursă date feed produse |
| Google Product Taxonomy | Categorisire corectă produse |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| ROAS minim la 30 zile | Minim 300% |
| Rata aprobare produse Merchant Center | Minimum 90% |
| Acoperire feed (produse active / total catalog) | Minimum 95% |
