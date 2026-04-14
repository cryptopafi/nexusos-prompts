---
type: procedure
created: 2026-03-22
status: active
slug: f-067-tesla-style-financial-model
tags: [procedure, nexus]
---

# Tesla-Style Full Financial Model Build — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui model financiar complet pentru o companie cu multiple segmente de venituri și CapEx intensiv, folosind Tesla ca studiu de caz paradigmatic.

---

## 1. Problema

Companiile high-growth cu multiple linii de business (vehicule/energie/servicii) sau cu unit economics complexe necesită modele financiare mai sofisticate decât un simplu 3-Statement standard. Modelarea bottom-up (unități × preț mediu) este mai accurată decât extrapolarea top-line. Această procedură acoperă: cercetarea companiei, modelarea pe segmente, 3-Statement complet, și valorizare DCF cu scenarii de creștere.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Cercetarea companiei — Business model și unit economics
Cercetează în profunzime: raportul anual (10-K Tesla sau echivalent), earnings calls (transcrieri pe Seeking Alpha sau SEC), prezentările pentru investitori. Documentează: segmentele de business (Automotive: Model 3/Y/S/X/Cybertruck, Energy: Powerwall/Megapack/Solar, Services: FSD/Supercharging/Insurance), prețul mediu de vânzare (ASP) per produs, costul de producție per unitate (COGS per vehicle), și marjele gross per segment.

### Pas 2: Construirea modelului de livrări de vehicule
Modelul de vehicule: `Revenue Automotive = Deliveries × ASP`. Proiectează livrările pe baza: capacitatea fabricilor existente (Fremont/Giga Berlin/Shanghai/Texas), timeline pentru noi fabrici și creșteri de capacitate, cota de piață în EV global și penetrarea EV vs total auto, prețuri și presiune competitivă (cum evoluează ASP sub presiunea concurenței). Construiește tabelul: an × model × livrări × ASP × revenue.

### Pas 3: Construirea modelului de venituri din Energy și Services
**Energy**: `Revenue Energy = MWh deployed × preț per MWh` (Megapack pentru utilități, Powerwall pentru residențial). Proiectează creșterea MWh pe baza backlog-ului raportat și capacității de producție Lathrop. **Services**: `Revenue Services = FSD attachments × preț FSD + Supercharging miles × tarif + Insurance premiums`. Notă: Services este segmentul cu creștere potențial exponențială dacă FSD se scalează.

### Pas 4: Modelarea marjelor gross per segment
Aplică marje diferite per segment: Automotive: model tradeable margin evolution (Tesla a oscilat 14-28%); Energy: margins improving cu scale (de la <5% la 20%+); Services: gross margin > 50% (software/charging). Calculează: `Gross Profit = Σ(Revenue segment × Gross Margin segment)`. Modelează cum marjele evoluează în timp pe baza economies of scale, mix shift, și presiune competitivă.

### Pas 5: Construirea modelului de cheltuieli operaționale
OpEx detaliat: **R&D** — investiții masive în FSD AI, Dojo supercomputer, noi vehicule (proiectează ca % din venituri, scăzând pe termen lung prin scale); **SG&A** — vânzări directe (fără dealeri = avantaj structural); identifică cheltuielile "one-time" vs recurente. **CapEx** — plan de expansiune fabrici (miliarde per Gigafactory), maturizarea instalațiilor existente, CapEx ca % venituri în scădere pe termen lung.

### Pas 6: Construirea modelului CapEx (factories și charging)
CapEx este critical pentru Tesla. Construiește: lista investițiilor planificate (Gigafactories noi, Supercharger network expansion, Dojo), timeline și costuri estimate, capitalizare vs expensing, impactul pe D&A (depreciation schedule per asset), FCF = EBITDA - CapEx (Tesla a generat FCF pozitiv consistent din 2020). Modelează CapEx intensity ratio pe termen lung (capex/revenue în scădere de la >10% la <5%).

### Pas 7: Build 3-Statement Model complet
Aplică procedura F-065 pentru linkarea completă: Income Statement cu toate segmentele → Balance Sheet → Cash Flow. Asigură-te că: livrările vehicule se leagă la revenue, CapEx se leagă la Fixed Assets și CFI, profitul net curge în RE și CFO. Adaugă Working Capital pentru Tesla: inventarul de vehicule în tranzit, prepayments de la clienți, payables către furnizori (Panasonic etc).

### Pas 8: Valorizare DCF cu scenarii de creștere
Construiește valorizarea: **DCF**: proiectare FCF 10 ani + terminal value (Gordon Growth Model sau EV/EBITDA exit). **WACC**: cost of equity (CAPM cu beta Tesla ~2.0) + after-tax cost of debt (low, Tesla are cash net pozitiv). **Scenarii**: Bull (autonomy/robotaxi materializes: +50% to intrinsic), Base (execution continues, EV dominance), Bear (competition intensifies, margins compress). Compară valoarea intrinsecă cu prețul de piață și determină marja de siguranță.

---

## 3. Cortex Logging

```json
{
  "text": "Tesla-Style Financial Model F-067 construit: cercetare business, model livrari vehicule, segmente Energy/Services, marje per segment, CapEx fabrici, 3-Statement integrat, DCF cu scenarii.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-067",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["financial-modeling", "Tesla", "multi-segment-model", "DCF", "unit-economics", "EV", "bottom-up"],
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
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-067 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Tesla-Style Full Financial Model Build" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Tesla 10-K / Earnings Releases | Date oficiale pentru calibrarea modelului |
| Excel / Google Sheets | Construcția modelului multi-segment |
| SEC EDGAR | Acces documente oficiale Tesla |
| Seeking Alpha / Earnings call transcripts | Context calitativ management |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Balance Sheet check | = 0 |
| Număr scenarii DCF | Minimum 3 (bull/base/bear) |
