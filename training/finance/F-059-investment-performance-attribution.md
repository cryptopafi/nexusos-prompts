---
type: procedure
created: 2026-03-22
status: active
slug: f-059-investment-performance-attribution
tags: [procedure, nexus]
---

# Investment Performance Attribution and Reporting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Descompunerea și raportarea performanței unui portofoliu de investiții față de benchmark folosind modelul Brinson-Hood-Beebower.

---

## 1. Problema

Simpla comparare a returnului total cu benchmark-ul nu explică de unde vine alpha sau underperformance. Fără atribuire granulară nu poți determina dacă performanța vine din decizii de alocare (asset class weighting) sau din selecție (security selection). Această procedură acoperă: calcul return ponderat în timp, decompoziție BHB, și raportare formală.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Calculul returnului total al portofoliului (time-weighted)
Folosește Time-Weighted Return (TWR) pentru a elimina impactul fluxurilor de numerar externe. Calculează sub-perioadele: `TWR = [(1+R1) × (1+R2) × ... × (1+Rn)] - 1`. Folosește datele zilnice ale NAV sau valorile de piață la fiecare contribuție/retragere. Documentează perioada de raportare (1M/3M/1Y/Since Inception).

### Pas 2: Calculul returnului benchmark-ului
Identifică benchmark-ul relevant (S&P 500, MSCI World, blended benchmark). Descarcă returnurile benchmark-ului pentru aceeași perioadă. Calculează returnul benchmark ponderat în funcție de alocarea strategică a portofoliului. Documentează benchmark-ul ales și justificarea.

### Pas 3: Calculul returnului activ
`Active Return = Return Portofoliu - Return Benchmark`. Interpretează: dacă pozitiv = alpha generat, dacă negativ = underperformance. Descompune returnul activ în componentele sale pentru analiză granulară.

### Pas 4: Decompoziția efectului de alocare
Aplică modelul Brinson-Hood-Beebower. **Efectul de alocare** = `(w_p - w_b) × (R_b_segment - R_b_total)`. Unde: `w_p` = greutatea portofoliului în segment, `w_b` = greutatea benchmark în segment, `R_b_segment` = returnul benchmark al segmentului. Calculează pentru fiecare clasă de active/sector.

### Pas 5: Decompoziția efectului de selecție și interacțiune
**Efectul de selecție** = `w_b × (R_p_segment - R_b_segment)`. Unde `R_p_segment` = returnul portofoliului în segment. **Efectul de interacțiune** = `(w_p - w_b) × (R_p_segment - R_b_segment)`. Verifică: `Active Return = Σ(Allocation Effect) + Σ(Selection Effect) + Σ(Interaction Effect)`.

### Pas 6: Prezentarea raportului de atribuire
Construiește tabelul de atribuire BHB cu toate segmentele și efectele. Adaugă vizualizare: grafic waterfall arătând contribuția fiecărui efect la returnul activ. Evidențiază top 3 contributori pozitivi și top 3 negativi. Include secțiune de commentary explicând deciziile principale care au generat alpha sau underperformance.

### Pas 7: Arhivarea și follow-up
Salvează raportul în format standard (PDF + Excel). Actualizează tracking-ul performance history. Identifică lecțiile de învățat: ce decizii de alocare/selecție au funcționat, care nu. Planifică ajustările pentru perioada următoare.

---

## 3. Cortex Logging

```json
{
  "text": "Investment Performance Attribution F-059 executat: TWR calculat, benchmark comparat, decompoziție BHB completata cu efecte alocare/selectie/interactiune.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-059",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["performance-attribution", "BHB-model", "portfolio-management", "benchmark", "alpha"],
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
1. `✅ [PROC] F-059 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Investment Performance Attribution and Reporting" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Portfolio management system / Excel | Calculul TWR și decompoziției BHB |
| Bloomberg / FactSet / Yahoo Finance | Date benchmark și returnuri segmente |
| Reporting template | Format standard raport atribuire |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Acuratețe decompoziție | Active Return = Σ efecte (reconciliere 100%) |
| Frecvență raportare | Lunar sau trimestrial |
