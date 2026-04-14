---
id: CRO-105
title: Funnel Analysis and Drop-off Diagnosis
domain: conversion-optimization
source: Complete Conversion Optimization Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Funnel Analysis and Drop-off Diagnosis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Diagnosticarea sistematică a punctelor de abandon în funnel cu GA4 Funnel Exploration, segmentare multi-dimensională, revenue impact calculation, și action plan prioritizat (quick wins vs. major redesign). Include benchmarks de drop-off per industrie.

---

## 1. Problema

Fără analiză structurată a funnelului, echipele optimizează pașii cu cel mai mic drop-off, lăsând neadresate bottleneck-urile reale. O reducere de 20% a drop-off-ului la un pas critic poate valora mai mult decât optimizarea completă a tuturor celorlalte pagini. Funnel analysis identifică UNDE să investești efortul.

Situații acoperite:
- Funnel nou cu conversie sub benchmark industrie
- Scădere bruscă a conversiei (diagnosticare cauză)
- Prioritizare efort CRO în funnel complex (5+ pași)

---

## 2. Procedura

### Pas 1: Cartografierea completă a funnelului
Definește fiecare pas de la prima vizită până la conversie:

**Funnel types și pași tipici**:
| Tip | Pași |
|-----|------|
| E-commerce | Landing → Product Page → Add to Cart → Cart → Checkout → Payment → Confirmation |
| Lead Gen | Landing → Form Start → Form Submit → Thank You → Email Nurture → Meeting |
| SaaS | Homepage → Pricing → Trial Signup → Onboarding → Feature Activation → Upgrade |
| B2B Service | Landing → Contact Form → Discovery Call → Proposal → Closed Won |

Documentează fiecare URL/acțiune. Creează o diagramă vizuală simplă a funnelului (Miro, FigJam, sau Sheets).

### Pas 2: Configurarea funnel tracking în GA4
GA4 → Explore → Funnel Exploration:
1. Adaugă fiecare pas ca event sau page_view
2. Setează funnel ca **open** (orice pas de intrare) vs. **closed** (ordine strictă) — start cu closed, apoi compare cu open
3. Configureaza breakdown per segment: device (mobile/desktop), source/medium, new/returning, country
4. **Verificare tracking**: Fă o tranzacție test completă → verifică că fiecare event apare corect în Funnel Exploration
5. Alternativa GA4: Mixpanel ($25/mo) sau Amplitude (free tier) — funnel analysis mai puternic

### Pas 3: Identificarea bottleneck-ului (cel mai mare drop-off)
Exportă datele de funnel și calculează per pas:

| Pas | Users | Drop-off absolut | Drop-off relativ | Benchmark |
|-----|-------|------------------|-------------------|-----------|
| Landing | 10,000 | - | - | - |
| Product Page | 4,000 | 60% | 60% | 50-70% normal |
| Add to Cart | 1,200 | 70% | 30% | 25-35% normal |
| Checkout | 600 | 50% | 50% | **45-55% = high** |
| Payment | 420 | 30% | 30% | 20-35% normal |
| Confirmation | 350 | 17% | 17% | 10-20% normal |

**Drop-off benchmarks** (e-commerce):
- Landing → Product: 50-70% drop-off (normal — browsing)
- Product → Cart: 25-40% (interest but not ready)
- Cart → Checkout: 30-50% (comparison shopping, sticker shock)
- Checkout → Payment: 15-30% (friction, trust, payment issues)
- **Cart abandonment rate overall**: 70% (Baymard Institute, average across industries)

Identifică pasul cu drop-off relativ ANORMAL (semnificativ peste benchmark) → acesta e bottleneck-ul.

### Pas 4: Analiza segmentată a drop-off-ului
NU analiza global — segmentează pe dimensiuni cheie:

| Segment | Ce cauți | Implicație |
|---------|---------|------------|
| Mobile vs. Desktop | Gap CR >30%? | Problemă UX mobilă specifică |
| Paid vs. Organic | Paid are drop-off mai mare? | Message mismatch pe ads |
| New vs. Returning | New users drop mai mult? | Lipsă educație/trust |
| Country/Region | Drop-off diferit per geografie? | Payment methods, shipping, language |
| Browser | Drop-off pe Safari/Firefox specific? | Bug tehnic browser-specific |

Fiecare segment cu comportament semnificativ diferit = diagnostic și soluție DIFERITĂ.

### Pas 5: Diagnosticarea cauzei drop-off-ului (multi-tool)
Aplică diagnostic pe pasul cu cel mai mare drop-off (minim 3 surse de date):
1. **Heatmaps** pe pagina cu drop-off (CRO-106) — ce clickuiesc? cât scrollează?
2. **Session recordings** — 20+ sesiuni de abandonuri la acel pas. Pattern-uri de confuzie?
3. **Exit surveys** (Hotjar) — "De ce pleci acum?" / "Ce lipsește?"
4. **Form analysis** (dacă formular) — care câmp are cel mai mare abandon? (Hotjar Form Analysis)
5. **Technical check** — JavaScript errors? Loading slow pe acel pas? Broken elements?

Coroborează datele: dacă 3 surse indică aceeași problemă → confidence ÎNALTĂ → action.

### Pas 6: Calculul potențialului de revenue
ÎNAINTE de a prioritiza un fix, calculează impactul:

```
Revenue potențial = [Users zilnici la pas] × [% reducere drop-off estimată] × [Average Order Value]
```

**Exemplu**:
- 1,000 users/zi la checkout, 70% drop-off, AOV €50
- Dacă reduci drop-off cu 10% (de la 70% la 60%): 1,000 × 0.10 × €50 = **€5,000/zi** = **€150,000/lună**
- Acest calcul JUSTIFICĂ investiția de dev resources (chiar și un redesign complet)

### Pas 7: Action plan prioritizat per effort level

| Categorie | Timp | Exemple | Implementare |
|-----------|------|---------|-------------|
| **Quick Wins** | <1 zi, no dev | Copy changes pe button/form, adăugare trust signals, microcopy | IMEDIAT |
| **Medium Effort** | 2-5 zile dev | A/B test pe layout, reducere câmpuri form, adăugare progress bar | Roadmap CRO-103 |
| **Major Redesign** | >5 zile dev | Restructurare checkout, guest checkout, new payment flow | Necesită aprobare buget |

**Quick wins**: Implementează IMEDIAT (nu în sprint-ul următor). Sunt free money.
**Medium effort**: Adaugă în backlog CRO-103 cu PIE/ICE score ridicat.
**Major redesign**: Prezintă cu revenue calculation → aprobare management → sprint dedicat.

**Re-analizează funnelul la 30 zile post-implementare** pentru a verifica impactul.

---

## 3. Cortex Logging

```json
{
  "text": "Funnel analysis completată: fiecare pas cartografiat și tracked în GA4, drop-off per pas calculat vs. benchmarks, bottleneck identificat prin analiza segmentată pe 5+ dimensiuni, cauză diagnosticată din 3+ surse, revenue impact calculat, action plan quick-wins/medium/major creat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-105",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["funnel-analysis", "drop-off", "cro", "ga4", "bottleneck", "revenue-impact", "segmentation"],
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
- Analiza funnelului fără segmentare → violation
- Bottleneck identificat fără revenue impact calculation → violation
- Diagnostic bazat pe o singură sursă de date → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Bottleneck identificat prin segmentare (nu global)?
- [ ] Revenue impact calculat?
- [ ] Quick wins implementate imediat?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-105 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Funnel Analysis and Drop-off Diagnosis" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics 4 (Funnel Exploration) | Analiza cantitativă drop-off |
| Hotjar ($39/mo) / Clarity (free) | Heatmaps, recordings, exit surveys |
| Hotjar Form Analysis | Analiza abandon formulare |
| Google Sheets | Revenue impact calculator |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Pași funnel documentați cu date | 100% |
| Segmente analizate per bottleneck | ≥3 |
| Surse de date coroborate per diagnostic | ≥3 |
| Quick wins implementate post-audit | în 5 zile lucrătoare |
| Uplift conversie funnel post-intervenție | ≥10% |
| Revenue impact calculat | per bottleneck |
