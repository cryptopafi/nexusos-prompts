---
type: procedure
created: 2026-03-22
status: active
slug: f-055-portfolio-risk-assessment
tags: [procedure, nexus]
---

# Portfolio Risk Assessment (MPT / Efficient Frontier) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea completă a riscului unui portofoliu existent folosind Modern Portfolio Theory, cu identificarea surselor de risc și a oportunităților de optimizare.

---

## 1. Problema

Un portofoliu poate părea diversificat dar să fie concentrat pe factori de risc comuni (ex: toate titlurile sunt sensibile la rata dobânzii). Fără o evaluare MPT structurată, riscurile ascunse rămân neidentificate până când se materializează. Această procedură acoperă situațiile de audit de portofoliu, onboarding client nou, sau revizie anuală.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectare date portofoliu — holdings și ponderi
Extrage din custodian sau sistem de portfolio management: lista completă de titluri / fonduri, ponderile curente (%), prețurile curente, și seria de prețuri lunare pe minimum 3 ani (36 observații minim pentru corelații valide). Verifică că suma ponderilor = 100%. Identifică dacă există cash nealocat și tratează-l separat. Normalizează pentru dividende și split-uri.

### Pas 2: Calculare rendamente și volatilitate individuale
Pentru fiecare titlu: calculează randamentele lunare: `r_t = (P_t - P_{t-1}) / P_{t-1}`. Calculează mean return lunar și anualizează: `μ_annual = (1 + μ_monthly)^12 - 1`. Calculează volatilitatea lunară (σ) și anualizează: `σ_annual = σ_monthly × √12`. Identifică outlier-ii — titluri cu volatilitate > 2× media portofoliului necesită analiză separată (pot fi candidați pentru reducere sau hedging).

### Pas 3: Construire matrice de covarianță / corelație
Calculează matricea de corelație: `ρ_{ij} = Cov(r_i, r_j) / (σ_i × σ_j)`. Verifică corelațiile extreme: corelații >0.85 între titluri sugerează diversificare insuficientă (dețin de fapt același risc factor). Identifică titlurile care au corelație negativă sau scăzută cu restul portofoliului (contributorii la diversificare). Construiește heatmap vizual pentru a identifica clustere de corelație.

### Pas 4: Calculare varianță și deviație standard portofoliu
Formula: `σ²_p = Σ_i Σ_j w_i × w_j × Cov(r_i, r_j) = w^T Σ w`. Calculează `σ_p = √(σ²_p)` — volatilitatea anualizată a portofoliului. Compară cu volatilitatea medie ponderată a titlurilor individuale: diferența = beneficiul de diversificare. Dacă `σ_p ≈ Σ w_i σ_i` → diversificarea este insuficientă (titlurile sunt prea corelate). Calculează Diversification Ratio: `DR = Σ(w_i σ_i) / σ_p` → target > 1.3 pentru un portofoliu bine diversificat.

### Pas 5: Plasare portofoliu pe Efficient Frontier
Generează Efficient Frontier: rulează optimizarea pentru target return-uri diferite și calculează volatilitatea minimă pentru fiecare. Vizualizează frontier. Plasează portofoliu curent pe grafic: este pe frontier (optim) sau sub frontier (suboptimal — poate fi îmbunătățit)? Calculează „efficiency gap": cât de mult return adițional ar putea fi generat la același nivel de risc, sau cât de mult risc ar putea fi redus la același return. Identifică portofoliul optim pe frontier (Tangency Portfolio = max Sharpe Ratio).

### Pas 6: Calculare contribuții de risc pe titlu (MCTR)
Marginal Contribution to Risk (MCTR) per titlu: `MCTR_i = (Cov(r_i, r_p) / σ_p) × w_i`. Suma MCTR_i = σ_p (verificare). Calculează % contribuție la risc per titlu: `% Risk_i = MCTR_i / σ_p`. Compară cu % ponderea: dacă un titlu = 5% din portofoliu dar = 15% din risc → supracontribuit la risc. Titlurile cu contribuție de risc disproporționată sunt candidații primari pentru reducere.

### Pas 7: Recomandări de optimizare
Formulează acțiuni specifice: (1) Reduce ponderile titlurilor cu MCTR disproporționat, (2) Adaugă titluri cu corelație scăzută pentru a îmbunătăți Diversification Ratio, (3) Dacă portofoliu e sub Efficient Frontier — propune realocarea concretă (care titluri crești/scazi). Estimează impactul îmbunătățirilor propuse pe σ_p și pe Expected Return. Prezintă portofoliu curent vs. portofoliu propus pe același grafic al Efficient Frontier.

---

## 3. Cortex Logging

```json
{
  "text": "Portfolio Risk Assessment (MPT / Efficient Frontier) executat: varianță și σ calculate, matrice corelații construită, portofoliu plasat pe Efficient Frontier, MCTR calculate per titlu, recomandări de optimizare formulate",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-055",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["portfolio-risk", "MPT", "Efficient-Frontier", "covariance-matrix", "risk-assessment", "advanced"],
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
- Risc calculat fără matrice de corelație completă → violation
- Plasare pe Efficient Frontier absentă → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-056 → VaR & Expected Shortfall (calcul adițional de tail risk)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-055 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Portfolio Risk Assessment (MPT / Efficient Frontier)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Price history (36+ luni) per titlu | Date pentru calcul corelații |
| Python (numpy, scipy.optimize) / Excel | Calcul covarianță și optimizare frontier |
| Bloomberg / Refinitiv | Sursa prețuri ajustate |
| Risk management system (Barra/Axioma) | Factor risk decomposition |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Diversification Ratio | > 1.3 |
| Titluri cu MCTR disproporționat identificate | Toate cele cu %Risk > 2× %Weight |
| Portofoliu plasat pe Efficient Frontier | Da, cu distanța față de frontier calculată |
