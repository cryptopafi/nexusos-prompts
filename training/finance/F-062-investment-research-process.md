---
type: procedure
created: 2026-03-22
status: active
slug: f-062-investment-research-process
tags: [procedure, nexus]
---

# 8-Step Investment Research Process — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces complet și sistematic de cercetare a investițiilor înainte de luarea oricărei decizii de cumpărare a unui instrument financiar.

---

## 1. Problema

Deciziile de investiție luate fără un proces sistematic sunt vulnerabile la bias-uri cognitive și informații incomplete. Un proces în 8 pași asigură că nicio dimensiune critică — de la business model la valorizare și sizing — nu este omisă. Această procedură acoperă: screening, analiză fundamentală, valorizare, sizing și scrierea tezei de investiție.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Universe Screening — Filtrare inițială
Aplică filtre cantitative pentru identificarea candidaților: valorizare (P/E < 20, sau EV/EBITDA < 12 pentru value, sau PEG < 1.5), calitate (ROE > 15%, ROIC > 10%, debt/equity < 1x), și dimensiune (market cap > $500M pentru lichiditate). Folosește screener-e (Finviz, Morningstar, Bloomberg). Listează top 5-10 candidați care trec filtrele.

### Pas 2: Analiza modelului de business și moat-ului competitiv
Răspunde la întrebările: Ce vinde compania și cui? Cum câștigă bani (unit economics)? Care este moat-ul competitiv (switching costs/network effects/cost advantage/intangibles)? Cât de mare este piața adresabilă (TAM)? Care sunt principalii competitori și cum se diferențiază? Citește raportul anual (10-K), prezentările pentru investitori, și 2-3 rapoarte de industrie.

### Pas 3: Analiza situațiilor financiare (3 ani)
Analizează ultimii 3 ani de: Cont de profit și pierdere (trend revenue, marje brute/operaționale/nete, EPS growth), Bilanț (structura activelor, datorii, poziție cash, working capital), Cash flow (FCF = Operating CF - CapEx, calitatea earnings), Indicatori cheie (ROIC, asset turnover, interest coverage). Identifică tendințe pozitive/negative și anomalii contabile.

### Pas 4: Evaluarea calității managementului
Evaluează: track record (au livrat pe ceea ce au promis în ultimii 3 ani?), structura de compensare (aliniată cu acționarii sau excesivă?), ownership (managerii dețin acțiuni semnificative?), capital allocation history (cum au folosit cash — acquisitions, buybacks, dividende, CapEx?), declarații publice și coerența acestora. Consultă proxy statement și istoricul guidance-ului.

### Pas 5: Valorizarea — DCF și Comparabile
Construiește: (1) **DCF simplu** — proiectare FCF 5 ani + terminal value cu WACC, calculează prețul intrinsec; (2) **Comparable companies** — EV/EBITDA, P/E, P/S față de competitori direcți; (3) **Precedent transactions** — dacă există M&A recente în sector. Calculează marja de siguranță: `(Valoare intrinsecă - Preț curent) / Valoare intrinsecă`. Cumpără dacă marja > 20-30%.

### Pas 6: Identificarea riscurilor — Top 3 scenarii bear
Documentează cele mai probabile 3 scenarii negative: (1) Riscul specific companiei (competitor disruptiv, pierdere client major, management scandal); (2) Riscul de industrie (reglementare, commodity prices, ciclu economic); (3) Riscul macro (recesiune, dobânzi, valută). Pentru fiecare scenariu, estimează impactul asupra valorizării și probabilitatea.

### Pas 7: Position Sizing — Kelly Criterion sau procent fix
Calculează dimensiunea poziției: (1) **Kelly Criterion**: `f = (bp - q) / b`, unde b = profit/loss ratio, p = probabilitate câștig, q = probabilitate pierdere; (2) **Alternativ**: procent fix (1-5% din portofoliu per poziție, ajustat pentru conviction level). Aplică Half-Kelly pentru siguranță. Verifică că poziția nu depășește concentrarea maximă din IPS.

### Pas 8: Scrierea tezei de investiție
Scrie un document de 1-2 pagini cu: rezumat executiv (buy/sell/hold + preț țintă), business model în 3 fraze, cei 3 catalizatori principali de upside, cele 3 riscuri principale și cum le monitorizezi, valorizare sintetică, sizing și orizontul de timp așteptat. Stochează teza în sistemul de tracking. Revizuiește trimestrial sau la apariția de informații semnificative noi.

---

## 3. Cortex Logging

```json
{
  "text": "8-Step Investment Research Process F-062 executat: screening, analiza business/financiara/management, valorizare DCF+comps, riscuri identificate, sizing calculat, teza scrisa.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-062",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["investment-research", "fundamental-analysis", "DCF", "position-sizing", "due-diligence"],
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
1. `✅ [PROC] F-062 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "8-Step Investment Research Process" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Finviz / Morningstar / Bloomberg | Screening și date financiare |
| SEC EDGAR (10-K, 10-Q, Proxy) | Documente oficiale pentru analiza fundamentală |
| Excel / Google Sheets | Model DCF și comparable analysis |
| Investment thesis template | Format standard pentru documentarea tezei |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Marjă de siguranță minimă | > 20% față de prețul intrinsec |
| Timp mediu per cercetare | 8-16 ore pentru o analiză completă |
