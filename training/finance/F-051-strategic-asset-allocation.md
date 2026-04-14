---
type: procedure
created: 2026-03-22
status: active
slug: f-051-strategic-asset-allocation
tags: [procedure, nexus]
---

# Strategic Asset Allocation (SAA) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Determinarea alocării strategice optime pe clase de active pentru un portofoliu pe termen lung, folosind optimizare mean-variance și Efficient Frontier.

---

## 1. Problema

Alocarea strategică a activelor explică 90%+ din variabilitatea randamentelor pe termen lung (Brinson, Hood, Beebower, 1986). Alegerea ad-hoc a alocării, fără o bază cantitativă, duce la portofolii suboptimale. Această procedură acoperă situațiile în care un CIO sau portfolio manager trebuie să determine mix-ul optim pe clase de active care satisface obiectivele din IPS.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire obiective pe termen lung
Preia din IPS (F-050): return target real anualizat, volatilitate maximă acceptabilă, maximum tolerable drawdown, horizon temporal. Traduce obiectivele în cerințe pentru modelul de optimizare: return minim necesar, volatilitate maximă ca constraint. Documentează: un investitor cu return target 7% real și volatilitate max 12% va necesita un portofoliu mai agresiv decât unul cu target 4% și volatilitate max 8%.

### Pas 2: Identificare clase de active pentru portofoliu
Selectează clase de active eligibile conform IPS: Equities (US large cap, US small cap, International Developed, Emerging Markets), Fixed Income (US Treasuries, Investment Grade Corporate, High Yield, EM Bonds, TIPS/inflation-linked), Real Assets (REITs, commodities, infrastructure), Cash (T-bills, money market). Elimină clasele excluse prin constrângerile din IPS (ex: dacă exclude EM → scoate din set).

### Pas 3: Colectare date return/risc/corelație
Adună date istorice (minimum 20 ani recomandat) sau forward-looking estimates (Capital Market Assumptions — CMA): Expected Return anual per clasă de active (atenție: randamentele trecute nu sunt neapărat predictive), Volatilitate (deviație standard anualizată), Matricea de corelație între toate perechile de clase. Surse CMA: BlackRock, JP Morgan, Vanguard publică anual Capital Market Assumptions. Decide: folosești date istorice sau CMA? CMA sunt mai potrivite pentru SAA pe 10+ ani.

### Pas 4: Rulare mean-variance optimization (Efficient Frontier)
Construiește modelul de optimizare Markowitz: `Maximizează Return - λ × Variance` subiect la constraints (suma ponderilor = 100%, fiecare pondere ≥ 0%, constrângeri maxime per clasă din IPS). Rulează optimizarea pentru un set de valori λ (risk aversion parameter) pentru a genera portofolii de pe Efficient Frontier. Vizualizează Frontier: axa X = volatilitate, axa Y = expected return. Fiecare punct pe frontier este optim (nu mai mult return la același risc).

### Pas 5: Selectare alocație target pe Efficient Frontier
Găsește portofoliul optim pentru profilul clientului: portofoliul cu cel mai mare Sharpe Ratio (tangency portfolio), sau portofoliul cu volatilitatea maximă specificată în IPS, sau portofoliul cu return target minim specificat în IPS. Aplică constrângerile din IPS (ex: max 60% equity, min 20% FI). Verifică că portofoliul selectat nu are concentrări extreme (un singur asset cu >40%).

### Pas 6: Aplicare constrângeri investitor
Ajustează portofoliul matematic pentru constrângerile practice: reballancing costs (evita alocări mici <3% care generează costuri ridicate relativ), Tax efficiency (prefer asset classes cu eficiență fiscală bună în conturi taxabile), Implementability (sunt instrumentele disponibile cu costuri rezonabile în jurisdicția investitorului?). Calculează portofoliul final ajustat și compară cu portofoliul optim teoretic — documentează trade-offs.

### Pas 7: Documentare SAA policy cu triggere de rebalancing
Scrie documentul de SAA policy: alocarea target per clasă de active (%), band de rebalancing (±%), benchmark per clasă și compozit, frecvența de revizie (anual sau când condițiile de piață se schimbă semnificativ). Setează triggere explicite pentru revizia SAA: schimbări majore de obiective, schimbări de regim macroeconomic pe termen lung, schimbări în caracteristicile claselor de active (ex: dobânzi la zero = tratament diferit FI).

---

## 3. Cortex Logging

```json
{
  "text": "Strategic Asset Allocation (SAA) executat: clase de active selectate, date return/risc/corelație colectate, Efficient Frontier construită, alocație target selectată și ajustată pentru constrângeri, SAA policy documentată",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-051",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["strategic-asset-allocation", "Efficient-Frontier", "Markowitz", "portfolio-management", "intermediate"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- SAA stabilit fără Efficient Frontier (sau justificare alternativă) → violation
- Alocație neverificată față de constrângerile din IPS → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-050 → IPS (input pentru SAA)
- F-052 → TAA Overlay (implementare tactică pe baza SAA)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-051 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Strategic Asset Allocation (SAA)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| IPS (F-050) | Obiective și constrângeri de optimizare |
| Capital Market Assumptions (BlackRock/JP Morgan) | Expected returns și volatilitate forward-looking |
| Python (scipy.optimize) / Excel Solver | Optimizare mean-variance |
| Bloomberg / MSCI | Date istorice și benchmark-uri |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Clase de active evaluate | Minimum 6 categorii distincte |
| Sharpe Ratio portofoliu selectat | ≥ 0.5 (la CMA rezonabile) |
| Constrângeri IPS respectate | 100% |
