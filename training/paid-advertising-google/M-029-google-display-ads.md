---
type: procedure
created: 2026-03-22
status: active
slug: m-029-google-display-ads
tags: [procedure, nexus]
---

# Google Display Ads Campaign — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea și optimizarea campaniilor Google Display Ads pentru brand awareness și remarketing pe rețeaua de display Google (GDN).

---

## 1. Problema

Campaniile Display Ads sunt frecvent irosite pe plasamente irelevante, audiențe prea largi și bannere de calitate scăzută, generând un CTR sub 0.1% și conversii minime la costuri ridicate. Fără excluderi de plasament și targeting precis, Google Display Network (GDN) cheltuiește bugetul pe aplicații mobile ieftine și site-uri de conținut de slabă calitate. Această procedură garantează că fiecare campanie Display are targetare precisă și assets de calitate care generează rezultate măsurabile.

Situații acoperite:
- Prima campanie Display pentru brand awareness sau prospecting
- Campanie Display de remarketing (complementară M-024)
- Optimizarea unei campanii Display existente cu CTR și conversii sub benchmark

---

## 2. Procedura

### Pas 1: Definește obiectivul campaniei și tipul de targeting
Alege obiectivul principal: Brand Awareness (optimizat pentru reach/impressions) sau Conversions (optimizat pentru clicks/conversii). Obiectivul determină bid strategy și cum măsori succesul. Alege tipul de targeting: Audience Targeting (In-market, Affinity, Custom Intent, Remarketing) sau Contextual Targeting (Keywords, Topics, Placements). Evită "Optimized Targeting" la start — îți reduce controlul.

### Pas 2: Configurează audiențele cu precizie (nu folosi "All audiences")
Pentru prospecting: folosește In-Market Audiences (Google clasifică utilizatorii după intenția de cumpărare recentă) SAU Custom Intent Audiences (targetezi persoane care caută anumite keywords pe Google). Pentru remarketing: importă audiențele din M-024. Combină maximum 2 tipuri de targeting în același Ad Group — mai multe reduc reach-ul excesiv sau diluează relevanța.

### Pas 3: Excluderi de audiențe și plasamente obligatorii
Adaugă imediat excluderi: (1) Exclude "All Converters" din campaniile de prospecting, (2) Exclude aplicații mobile în bloc dacă nu ai campanie dedicată mobile (Placements → Exclude → "Mobile app categories"), (3) Exclude topicuri irelevante pentru brand (ex: conținut pentru copii dacă targetezi adulți). Aceste excluderi pot reduce costul cu 20-40% fără a afecta reach-ul relevant.

### Pas 4: Creează Responsive Display Ads cu assets de calitate maximă
Creează minimum 1 Responsive Display Ad per Ad Group cu: 5 headlines (30 char), 1 long headline (90 char), 5 descriptions (90 char), minimum 5 imagini landscape (1200x628) și 2 imagini square (1200x1200), logo. Toate imaginile trebuie să fie imagini reale (nu stock photos generice) — Google evaluează calitatea imaginilor și preferă cele autentice. Ad Strength trebuie să fie "Good" sau "Excellent".

### Pas 5: Setează bid strategy și frequency cap
Bid strategy pentru awareness: Target CPM (cost per 1000 impresii). Bid strategy pentru conversii: Target CPA sau Maximize Conversions. Frequency cap: OBLIGATORIU pentru Display — setează maximum 3-5 impresii/utilizator/zi și maximum 15-20 impresii/utilizator/săptămână. Fără frequency cap, aceleași persoane văd anunțul de zeci de ori, dăunând brandului și crescând costul fără beneficiu.

### Pas 6: Excluderi de plasamente după primele 72h (Placement Exclusions)
După 72h de rulare, descarcă Placement Report. Identifică și excludele: site-uri cu CTR foarte mare (potențial click fraud), aplicații mobile cu zeci de mii de impresii și 0 conversii, site-uri de conținut complet irelevant. Creează o Placement Exclusion List la nivel de cont și aplica-o la toate campaniile Display. Revizuiește plasamentele lunar.

### Pas 7: Monitorizează View-Through Conversions cu atenție
Display Ads generează adesea View-Through Conversions (utilizatorul a văzut anunțul dar a convertit prin alt canal). Acestea pot fi înșelătoare — nu le considera egale cu click conversions. Setează View-Through Conversion window la maximum 1 zi (nu 30 de zile default) pentru o atribuire mai realistă. Documentează în Cortex: audiențe targetate, CTR, VTC vs. Click conversions, placementuri excluse.

---

## 3. Cortex Logging

```json
{
  "text": "M-029 executat: Campanie Google Display Ads configurată. Obiectiv: [awareness/conversii]. Tip targeting: [audiențe/contextual]. Audiențe: [tipuri]. Frequency cap: [N/zi]. Placementuri excluse după 72h: [N]. CTR: [%]. View-Through Conversion window: [N zile].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-029",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "display-ads", "gdn", "responsive-display", "placement-exclusions", "frequency-cap", "in-market-audiences"],
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
- Frequency cap nesetat pentru campaniile Display → violation
- Campanie Display lansată fără excludere aplicații mobile → violation
- View-Through Conversion window lăsat la 30 de zile default fără justificare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Placement Report revizuit și excluseri aplicate după 72h?
- [ ] Frequency cap setat la maximum 5 impresii/utilizator/zi?

**VK-uri obligatorii:**
1. `✅ [PROC] M-029 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Display Ads Campaign" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Display Network (GDN) | Rețeaua de plasamente |
| Remarketing Audiences (M-024) | Audiențe pentru remarketing Display |
| Google Analytics 4 | Analiza comportamentului post-click de pe Display |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CTR minim campanie Display | Minim 0.35% (benchmark GDN = 0.1%, target 3x) |
| Placement Exclusions aplicate | Minimum 20 plasamente excluse la 30 zile |
| Frequency cap respectat | Maximum 5 impresii/utilizator/zi |
