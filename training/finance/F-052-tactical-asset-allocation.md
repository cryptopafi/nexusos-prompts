---
type: procedure
created: 2026-03-22
status: active
slug: f-052-tactical-asset-allocation
tags: [procedure, nexus]
---

# Tactical Asset Allocation (TAA) Overlay — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea unui overlay tactic pe Strategic Asset Allocation pentru exploatarea oportunităților de scurtă durată prin semnale de valuation, momentum, și macroeconomice.

---

## 1. Problema

Alocarea strategică este optimă pe termen lung, dar piețele create anomalii pe termen scurt (6-18 luni) care pot fi exploatate prin ajustări tactice. Această procedură acoperă situațiile în care un portfolio manager sau investment committee trebuie să determine și să implementeze un overlay tactic față de SAA, menținând disciplina de a reveni la strategic când semnalele se inversează.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Stabilire SAA ca punct de referință
Pornește de la SAA documentat (F-051) — acesta este benchmark-ul și punctul neutru. Definește rangurile tactice permise per clasă de active: typical ±5pp față de SAA (ex: SAA = 40% equity → TAA range = 35%-45%). Documentează: cine are autoritatea de a implementa devieri tactice (CIO / Investment Committee / portfolio manager cu limite pre-aprobate). Frequency: TAA se revizuiește lunar sau trimestrial — nu zilnic.

### Pas 2: Identificare semnale de piață pentru pariuri tactice
Sistematizează sursele de semnale: (1) Valuation signals: P/E ratio față de medie istorică (Shiller CAPE), credit spreads vs. average, yield curve shape. (2) Momentum signals: relative performance clasă de active în ultimele 6-12 luni (momentum factor funcționează cross-asset). (3) Macro signals: leading economic indicators (PMI, yield curve inversă), inflație vs. așteptări, politica monetară (hiking vs. cutting cycle). Documentează fiecare semnal cu sursa și frecvența de update.

### Pas 3: Definire framework de decizie tactică
Construiește un scorecard cu semnalele identificate: fiecare semnal primeșe un scor (-2 = very bearish, -1 = bearish, 0 = neutral, +1 = bullish, +2 = very bullish) pentru fiecare clasă de active. Calculează scorul compozit per clasă (medie sau ponderată). Regulă de decizie: Scor compozit > +1 → overweight maxim (+5pp). Scor 0 to +1 → overweight moderat (+2pp). Scor -1 to 0 → neutral. Scor < -1 → underweight (-2 la -5pp). Documentează decizia finală și raționamentul.

### Pas 4: Calculare poziții de overweight/underweight
Traduce semnalele în ajustări concrete de portofoliu: dacă SAA = 40% equities și scorul tactic este +2 → alocă 45% (overweight +5pp). Ajustarea compensatorie: dacă crești equities cu +5pp, trebuie să reduci altceva cu -5pp (de obicei cash sau clasa cu semnal cel mai negativ). Calculează impactul pe expected return al portofoliului și pe volatilitate (deviația de la SAA crește tracking error față de benchmark).

### Pas 5: Implementare prin instrumente lichide
Implementează devierile tactice prin instrumente cu cost redus și lichiditate ridicată: ETF-uri broad market (cel mai eficient), futures pe indici (pentru sume mari), opțiuni (pentru expresii asimetrice ale viziunii). Evită implementarea tactică în instrumente ilicide (real estate fizic, PE) — costul de intrare/ieșire distruge alpha tactic. Calculează costul de tranzacție al implementării și compară cu alpha așteptat.

### Pas 6: Monitorizare performanță vs. SAA benchmark
Track lunar: return portofoliu TAA vs. return portofoliu SAA pur. Calculează: alpha generat (TAA return - SAA return), tracking error al portofoliului TAA față de SAA, Information Ratio al TAA (alpha / tracking error). Raportează Investment Committee trimestrial cu: semnalele actuale, pozițiile tactice active, și performanța historică a TAA (care semnale au generat alpha și care nu).

### Pas 7: Revertire la strategic când semnalele se inversează
Definește regula de exit: când scorul compozit revine la 0 (neutral), revino la alocația SAA. Nu lăsa pozițiile tactice să persiste în absența semnalelor — disciplina de exit este la fel de importantă ca disciplina de entry. Setează stop-loss implicit: dacă o poziție tactică a pierdut mai mult decât alpha așteptat × 2 → exit forțat indiferent de semnale. Documentează fiecare ciclu TAA: entry date, exit date, alpha realizat.

---

## 3. Cortex Logging

```json
{
  "text": "Tactical Asset Allocation (TAA) Overlay executat: semnale de piață analizate, scorecard completat, poziții tactice calculate și implementate prin ETF-uri, performanță monitorizată vs. SAA benchmark",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-052",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["tactical-asset-allocation", "TAA", "market-timing", "portfolio-management", "advanced"],
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
- Devieri tactice fără scorecard de semnale documentat → violation
- Poziții tactice fără regulă de exit definită → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-051 → SAA (punct de pornire pentru TAA)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-052 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Tactical Asset Allocation (TAA) Overlay" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| SAA policy (F-051) | Benchmark și punct de referință tactic |
| Bloomberg / Reuters | Semnale de valuation și macro |
| ETF-uri broad market | Instrumente de implementare tactică |
| Risk system | Calculare tracking error și VaR |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Semnale analizate per decizie | Minimum 3 categorii (valuation + momentum + macro) |
| Information Ratio TAA | > 0.3 pe perioadă de 3 ani |
| Tracking error față de SAA | ≤ rangul tactic permis (±5pp) |
