---
type: procedure
created: 2026-03-22
status: active
slug: f-053-portfolio-construction
tags: [procedure, nexus]
---

# Portfolio Construction and Diversification — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui portofoliu diversificat de la selecția universului de investiții până la alocarea ponderilor și calculul caracteristicilor de risc-randament.

---

## 1. Problema

Selecția și ponderarea incorectă a titlurilor sau fondurilor duce la concentrare nedorită, corelații ascunse care se materializează în crieze, și un profil risc-randament suboptimal. Această procedură acoperă situațiile de construcție a unui portofoliu nou sau reconstrucție a unuia existent, bazate pe principiile MPT și diversificarea efectivă.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire univers de investiție
Pe baza IPS și SAA, definește universul eligibil: geografii (US only / Global Developed / + Emerging Markets), instrumente (individual stocks / ETF-uri / mutual funds / bonds / alternatives), criterii de eligibilitate (market cap minim, liquiditate minim - $1M ADTV, rating minim BBB- pentru corporate bonds). Aplică excluderile din IPS (ESG screens etc.). Rezultă un univers de candidați investibili.

### Pas 2: Selectare titluri sau fonduri individuale
Aplică criterii de selecție pentru a reduce universul la portofoliul final: pentru stockuri — screening cantitativ (P/E, ROE, momentum, quality score), urmat de analiză calitativă (competitive moat, management quality). Pentru ETF-uri / fonduri — evaluează expense ratio (cel mai mic), tracking error față de index (cel mai mic), AUM și lichiditate (cel mai mare), metodologie de rebalancing a indexului. Documentează raționamentul de selecție pentru fiecare titlu / fond inclus.

### Pas 3: Alocarea ponderilor
Evaluează metode de ponderate: (1) Market Cap Weighting: ponderi proporționale cu cap de piață — simplu, low-cost, dar concentrat în winners recent. (2) Equal Weighting: fiecare titlu = 1/N — mai diversificat, dar costuri de rebalancing mai mari și tilt spre small cap. (3) Risk Parity: ponderi inversate față de contribuția la volatilitate — fiecare titlu contribuie egal la riscul total. (4) Custom / Factor-based: ponderi bazate pe factor scores (value, quality, momentum). Selectează metoda aliniată cu filozofia de investiție și IPS.

### Pas 4: Calculare expected return al portofoliului
`Expected Return Portfolio = Σ (w_i × Expected Return_i)`, unde w_i = ponderea titlului i. Calculează pentru scenariul Base și stres (Downside scenario). Verifică că expected return al portofoliului satisface obiectivul din IPS. Dacă nu: ajustează ponderile sau reconsideră universul de investiție (creșterea return-ului necesită de obicei mai mult risc — prezintă trade-off-ul clientului explicit).

### Pas 5: Calculare risc portofoliu (matricea de corelație)
Construiește matricea de corelație pentru toate titlurile din portofoliu: `Variance Portfolio = w^T × Σ × w`, unde Σ este matricea de covarianță. Calculează Standard Deviation: `σ_p = √(w^T Σ w)`. Calculează contribuția marginală la risc a fiecărui titlu (MCTR). Identifică titlurile cu contribuție de risc disproporționat față de ponderea lor (candidați pentru reducere). Verifică că nici un titlu nu contribuie >20% din riscul total al portofoliului.

### Pas 6: Optimizare pentru target risc-randament
Compară portofoliu curent cu Efficient Frontier din F-051: este portofoliu curent pe frontier sau sub ea? Dacă sub frontier → poate fi îmbunătățit prin realocare fără a crește riscul. Ajustează ponderile marginal: crește ponderile titlurilor cu Sharpe individual ridicat și corelație scăzută cu restul portofoliului. Rulează optimizare (Python / Excel Solver) cu constrângeri practice (min/max per titlu, sector limits). Documentează portofoliul final cu justificarea fiecărei ponderi.

### Pas 7: Documentare și planificare implementare
Creează documentul de portofoliu: lista completă de titluri cu ponderi target și range de rebalancing, costul estimat de implementare (bid-ask spread + comisioane), implicații fiscale ale tranzițiilor (dacă portofoliu existent), timeline de implementare (phased approach pentru sume mari pentru a reduce market impact), și frecvența de revizie a portofoliului.

---

## 3. Cortex Logging

```json
{
  "text": "Portfolio Construction and Diversification executat: univers definit, titluri selectate, ponderi calculate, expected return și risc calculate, portofoliu optimizat și documentat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-053",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["portfolio-construction", "diversification", "MPT", "risk-parity", "portfolio-management", "intermediate"],
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
- Ponderi alocate fără calculul matricei de corelație → violation
- Portofoliu cu concentrare >20% per titlu neexplicitată → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-051 → SAA (definește clasele de active per portofoliu)
- F-055 → Portfolio Risk Assessment (analiză detaliată MPT post-construcție)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-053 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Portfolio Construction and Diversification" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| IPS (F-050) și SAA (F-051) | Constrângeri și target de alocare |
| Bloomberg / Morningstar | Date titluri și corelații |
| Python (numpy, scipy) / Excel Solver | Optimizare ponderi |
| Custodian / broker | Date de lichiditate și costuri tranzacționare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Număr titluri în portofoliu | 15-30 pentru diversificare efectivă |
| Concentrare per titlu | Max 10-15% în portofoliu diversificat |
| Portofoliu pe / aproape de Efficient Frontier | Verificat și documentat |
