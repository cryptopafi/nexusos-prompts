---
type: procedure
created: 2026-03-22
status: active
slug: f-063-etf-selection-analysis
tags: [procedure, nexus]
---

# ETF Selection and Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Procesul de selecție și analiză comparativă a ETF-urilor pentru implementarea eficientă a alocărilor de portofoliu.

---

## 1. Problema

Piața are mii de ETF-uri pentru orice clasă de active sau strategie, cu diferențe semnificative în costuri, tracking error, și metodologie. Alegerea greșită poate eroda performanța cu 0.5-1% anual. Această procedură acoperă: definirea obiectivului de investiție, screening ETF-uri, analiza metodologiei și lichidității, compararea opțiunilor, și selecția finală.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definirea obiectivului de investiție și a expunerii necesare
Specifică cu precizie: (1) Clasa de active vizată (US Large Cap, Emerging Markets Bonds, Gold, etc.); (2) Stilul de investiție (blend/value/growth); (3) Geografic (SUA, Europa, Asia, Global); (4) Factor/smart beta dacă este cazul (momentum, low-volatility, quality); (5) Orizontul de timp și scopul (growth, hedging, income). Această claritate previne selectarea unui ETF care nu se potrivește cu nevoia reală.

### Pas 2: Screening ETF-uri pe criterii de bază
Aplică filtrele minime: AUM (Assets Under Management) > $100M — sub acest nivel riscul de lichidare a fondului crește; Expense Ratio < 0.5% pentru pasive (< 0.2% ideal pentru ETF-uri index principale); Tracking Error < 0.2% față de benchmark (disponibil pe ETF.com sau Morningstar); ETF cu cel puțin 3 ani de existență pentru istoric verificabil. Rezultat: o listă scurtă de 5-10 ETF-uri candidate.

### Pas 3: Analiza metodologiei de indexare
Compară metodologiile: **Cap-weighted** (MSCI, S&P) — standard, low cost, dar concentrare la top holdings; **Equal-weight** — diversificare mai mare, rebalansare mai costisitoare; **Smart Beta / Factor** — momentum/value/quality tilt, costuri mai mari, test dacă factorul este persistent. Verifică: compoziția top 10 holdings (% din fond), frecvența rebalansării indexului, tratamentul dividendelor (acumulating vs distributing).

### Pas 4: Verificarea lichidității — Volume zilnic mediu
Lichiditatea slabă înseamnă spread-uri mari la cumpărare/vânzare. Verifică: Average Daily Volume (ADV) — preferă ETF-uri cu ADV > $1M pentru investitori retail, > $10M pentru investitori instituționali; Bid-Ask Spread — < 0.05% pentru ETF-uri majore; Verifică că nu există concentrare excesivă în piețe sublichide (ex: ETF-uri pe piețe frontier cu spread-uri mari).

### Pas 5: Analiza suprapunerii cu portofoliul existent
Folosește ETF Overlap Tool (ETF.com, Morningstar Overlapping Calculator): calculează overlapping % cu pozițiile existente în portofoliu. Un overlap > 40% înseamnă diversificare redusă. Identifică dacă ETF-ul nou introduce concentrare în sectoare/companii deja supraponderate. Verifică că adăugarea ETF-ului îmbunătățește profilul risc/randament al portofoliului agregat.

### Pas 6: Compararea celor mai bune 3-5 opțiuni
Construiește tabelul comparativ: coloane = ETF ticker, coloane = AUM, Expense Ratio, Tracking Error, ADV, Distribuitor, Metodologie, Return 1Y/3Y/5Y. Calculează "Cost efectiv total" = Expense Ratio + Tracking Error + Impact spread. Clasează ETF-urile pe cost efectiv total.

### Pas 7: Selecția opțiunii cu cel mai bun raport cost/tracking
Selectează ETF-ul cu costul efectiv total cel mai mic care satisface toate criteriile din Pas 2. Dacă există ETF distributing vs accumulating, alege în funcție de situația fiscală (accumulating pentru conturi impozabile unde dividendele sunt taxate, distributing pentru conturi cu scutire de impozit pe dividende). Documentează decizia cu raționalul complet.

---

## 3. Cortex Logging

```json
{
  "text": "ETF Selection and Analysis F-063 executat: obiectiv definit, screening aplicat, metodologie analizata, lichiditate verificata, overlap calculat, ETF optim selectat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-063",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["ETF", "passive-investing", "index-funds", "tracking-error", "expense-ratio", "portfolio-construction"],
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
1. `✅ [PROC] F-063 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "ETF Selection and Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ETF.com / Morningstar ETF screener | Screening și date de bază ETF |
| ETF Overlap Calculator | Analiza suprapunerii cu portofoliu existent |
| Broker platform | Verificarea spread-urilor reale și ADV |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Expense ratio maxim acceptat | < 0.5% (< 0.2% pentru core holdings) |
| Tracking error maxim | < 0.2% |
