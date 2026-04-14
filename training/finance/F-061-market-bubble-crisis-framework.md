---
type: procedure
created: 2026-03-22
status: active
slug: f-061-market-bubble-crisis-framework
tags: [procedure, nexus]
---

# Market Bubble and Crisis Identification Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Identificarea sistematică a condițiilor de bulă speculativă sau criză financiară iminentă pentru ajustarea corespunzătoare a pozițiilor de portofoliu.

---

## 1. Problema

Bulele de piață și crizele financiare sunt identificate de regulă în retrospectivă, nu în timp real. Investitorii care nu monitorizează indicatori de exces speculativ suferă pierderi severe. Această procedură acoperă: monitorizarea indicatorilor de valorizare, semnele de exces speculativ, analiza levierului sistemic, comparația cu bule istorice, ciclul Minsky, și pozitionarea portofoliului.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Monitorizarea indicatorilor de valorizare a pieței
Verifică lunar acești indicatori: (1) **Shiller CAPE Ratio** (Cyclically Adjusted P/E) — valori > 30 semnalează supravalorizare istorică (media istorică ~16); (2) **Tobin's Q-Ratio** — valoarea de piață a companiilor vs costul de înlocuire a activelor, > 1.5 = supravalorizare; (3) **Buffett Indicator** (Market Cap / GDP) — > 150% = zona de pericol (media istorică 75-90%). Documentează valorile curente și tendința.

### Pas 2: Identificarea semnelor de exces speculativ
Scanează pentru: (1) **Retail investor frenzy** — Google Trends pentru termeni ca "cum să cumpăr acțiuni", forumuri Reddit, conturi noi la brokeri; (2) **IPO surge** — număr record de IPO-uri la valorizări maxime, SPACs proliferând; (3) **Narrative-driven prices** — prețuri justificate exclusiv prin povești/viziuni, fără fundamente; (4) **Asset class rotation** — bani masivi care se mișcă în active nelichide (crypto obscure, NFT-uri, meme stocks). Scor agregat: câți dintre cei 4 factori sunt prezenti.

### Pas 3: Verificarea ratelor de datorie/levier în sistem
Analizează: credit growth vs PIB (accelerare rapidă = semnal de risc), margin debt la brokeri (niveluri record = semnal), corporate leverage (debt/EBITDA mediu S&P 500 > 3.5x = risc), consumer credit (datorii gospodărești ca % din venit disponibil). Compară cu nivelurile din 2007-2008 și 2000. Dacă 3+ indicatori sunt la niveluri record sau aproape de acestea, activează alertă.

### Pas 4: Compararea cu bule istorice (1929/2000/2008)
Aplică checklist-ul pattern-urilor comune: inovație tehnologică care justifică "noua paradigmă" (1929: electricitate/radio, 2000: internet, 2021: crypto/AI), democratizarea investițiilor aducând bani neavizați, leverage excesiv alimentând creșteri, presă de masă cu titluri euforice. Calculează similaritate procentuală cu fiecare bulă istorică. Dacă similaritatea cu 2000 sau 2008 > 60%, escaladează la Pas 5.

### Pas 5: Aplicarea analizei ciclului Minsky
Identifică faza curentă din ciclul Minsky: (1) **Displacement** — eveniment care creează oportunitate nouă; (2) **Boom** — prețuri cresc, optimism; (3) **Euphoria** — toată lumea cumpără, "data e diferită"; (4) **Profit Taking** — insiderii vând, semnale mixte; (5) **Panic** — colaps, vânzare forțată. Localizează piața în ciclu. Faza 3-4 = risc maxim de bulă.

### Pas 6: Determinarea pozitionării portofoliului
Pe baza analizei anterioare, selectează una din strategii: **Green** (faza 1-2): portofoliu normal, eventual supraponderare risc; **Yellow** (faza 3): reducere risc 15-25%, creștere cash/obligatiuni, oprire leverage; **Red** (faza 4-5): reducere masivă risc 30-50%, hedging activ (put options), cash/gold/T-bills. Documentează decizia cu raționalul complet și data revizuirii.

---

## 3. Cortex Logging

```json
{
  "text": "Market Bubble & Crisis Framework F-061 executat: indicatori valorizare verificati, exces speculativ analizat, ciclu Minsky identificat, portofoliu repoziţionat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-061",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["market-bubble", "crisis-framework", "Minsky-cycle", "CAPE-ratio", "risk-management", "macro"],
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
1. `✅ [PROC] F-061 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Market Bubble and Crisis Identification Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Robert Shiller CAPE data (multpl.com) | Date Shiller CAPE Ratio actualizate |
| Federal Reserve data (FRED) | Date levier sistemic și credit |
| Hussman Funds / GMO research | Analize valorizare piață |
| Macro dashboards (Bloomberg / Refinitiv) | Date agregatoare indicatori macro |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Frecvență monitorizare | Lunar (sau săptămânal în faze 3-4) |
| Timp de reacție la semnal Red | < 5 zile lucrătoare |
