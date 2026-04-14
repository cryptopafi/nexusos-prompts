---
type: procedure
created: 2026-03-22
status: active
slug: f-050-investment-policy-statement
tags: [procedure, nexus]
---

# Investment Policy Statement (IPS) Creation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea unui Investment Policy Statement complet și operaționalizabil care ghidează toate deciziile de alocare a capitalului pentru un investitor instituțional sau individual sofisticat.

---

## 1. Problema

Fără un IPS documentat, deciziile de investiție sunt reactive (luate pe baza emoțiilor sau volatilității de piață) și inconsistente cu obiectivele pe termen lung. Această procedură acoperă situațiile în care un wealth manager, CIO, sau consultant de investiții trebuie să creeze sau să revizuiască un IPS pentru un client (family office, fundație, fond de pensii, sau individ cu avere semnificativă).


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire obiective de investiție
Clarifică cu clientul: Return target (nominal sau real, % pe an), Time horizon (câți ani până la nevoia de lichiditate), Liquidity needs (ce % din portofoliu poate fi nevoie în <12 luni), Purpose specifică a portofoliului (pensionare / moștenire / finanțare cheltuieli operaționale / endowment). Documentează toate obiectivele în ordine de prioritate. Conflict de obiective (return mare + lichiditate mare + risc mic) trebuie rezolvat explicit cu clientul.

### Pas 2: Documentare toleranță la risc
Administrează chestionarul standardizat de risk tolerance: capacitate obiectivă (câte pierderi poate suporta financiar fără a afecta planurile de viață) și disponibilitate subiectivă (câte pierderi acceptă psihologic fără a reacționa emoțional). Calculează Maximum Tolerable Loss (MTL): scenariul cel mai nefavorabil care nu ar duce la lichidare forțată. Exprimă toleranța la risc ca % maxim de drawdown acceptabil (ex: -20% worst year) și volatilitate anuală maximă.

### Pas 3: Stabilire asset allocation target
Folosind obiectivele (Pas 1) și toleranța la risc (Pas 2), stabilește alocarea strategică pe clase de active: Equity (domestic/international/EM), Fixed Income (government/corporate/high yield), Alternative Investments (real estate/commodities/infrastructure), Cash & equivalents. Exprimă ca %, cu band de rebalancing: target ±5% (sau ±10% pentru clase mai volatite). Documentează raționamentul pentru fiecare alocare.

### Pas 4: Specificare constrângeri de investiție
Documentează toate restricțiile: ESG/SRI (excluderi sectoare: armament/tutun/gambling/fossil fuels), Fiscale (evitare short-term gains, maximizare tax-loss harvesting, structuri fiscale), Legale (restricții de concentrare, insider trading policies, restricted lists), Lichiditate minimă (% minim în active lichide în <30 zile), Concentrare maximă (max % într-un singur emitent, industrie, geografie), Instrumente interzise (derivate cu leverage, structuri opace).

### Pas 5: Definire reguli de rebalancing
Specifică: metoda de rebalancing (calendar-based: trimestrial/semestrial vs. threshold-based: când deviezi >5% de la target), procesul (cine inițiază, cine aprobă), costul estimat (taxe de tranzacționare, implicații fiscale ale rebalancing-ului). Documentează ordinea de rebalancing preferată: mai întâi redistribuie cash flow-ul nou (dividende, contribuții), apoi vinde câștigătorii, în final cumpără perdanții. Evită rebalancing excesiv (costuri > beneficii).

### Pas 6: Stabilire benchmark-uri de performanță
Definește un benchmark relevant pentru fiecare clasă de active și un benchmark compozit pentru întreg portofoliul. Benchmark-ul trebuie să fie: investibil (poți replica direct), specificat în avans (nu ales ex-post), relevant pentru strategia urmată. Exemplo: 60% MSCI World + 40% Bloomberg Global Aggregate = benchmark compozit pentru portofoliu 60/40. Stabilește perioada de evaluare (1 an rolling, 3 ani rolling) și pragul de semnificație al deviației.

### Pas 7: Revizie anuală și update IPS
Programează revizia anuală a IPS: au apărut schimbări în obiective? (schimbări de viață, horizon temporal mai scurt). S-a schimbat toleranța la risc? (experiențe de piață, schimbări financiare). Sunt constrângerile la zi? Documentează toate modificările cu dată și raționament. IPS-ul este un document viu — nu o decizie unică. Comunică orice modificare relevantă tuturor parților implicate (consilier, custodian, manager).

---

## 3. Cortex Logging

```json
{
  "text": "Investment Policy Statement (IPS) Creation executat: obiective definite, toleranță la risc documentată, SAA stabilit, constrângeri specificate, reguli rebalancing definite, benchmark-uri stabilite",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-050",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["IPS", "investment-policy", "asset-allocation", "portfolio-management", "intermediate"],
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
- IPS fără toleranță la risc documentată (subiectiv + obiectiv) → violation
- IPS fără reguli explicite de rebalancing → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-051 → Strategic Asset Allocation (implementare SAA din IPS)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-050 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Investment Policy Statement (IPS) Creation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Risk tolerance questionnaire | Evaluare subiectivă și obiectivă |
| Situația financiară a clientului | Determinare obiective și constrângeri |
| CFA Institute IPS guidelines | Standard de referință pentru structura documentului |
| Benchmark data (Bloomberg/MSCI/FTSE) | Definire benchmark-uri de performanță |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Secțiuni IPS completate | 100% (obiective, risc, SAA, constrângeri, rebalancing, benchmark) |
| Revizie anuală programată | Data specifică setată |
| Constrângeri documentate | Toate tipurile relevante (ESG, fiscal, legal, lichiditate) |
