---
type: procedure
created: 2026-03-22
status: active
slug: f-020-lease-analysis
tags: [procedure, nexus]
---

# Lease Analysis (Operating vs. Finance) — IFRS 16 / ASC 842 — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Clasificarea și contabilizarea contractelor de leasing conform IFRS 16 / ASC 842, cu calculul ROU Asset, Lease Liability, impactul pe EBITDA/EBIT, ajustarea ratiourilor financiare, și implicații pentru evaluarea companiei.

---

## 1. Problema

Implementarea IFRS 16 și ASC 842 a adus toate leasingurile operaționale pe bilanț, modificând fundamental ratiourile de leverage și acoperire. O companie cu leasing-uri operaționale semnificative (retail, airlines, real estate) poate vedea Debt/EBITDA crescând cu 1-3x post-IFRS 16. Fără înțelegerea impactului, un analist poate subevalua riscul de leverage sau supraestima EBITDA. Această procedură acoperă situațiile de analiză financiară, due diligence, restatement contabil, și comparare cross-company unde analistul trebuie să identifice, clasifice și contabilizeze corect toate contractele de leasing. Context românesc: companiile listate la BVB aplică IFRS, inclusiv IFRS 16.

> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificare și inventariere contracte de leasing
Caută în notele la situațiile financiare secțiunile dedicate leasingului (IFRS 16 / ASC 842 disclosures). Extrage: valoarea totală a obligațiilor de leasing, scadențele (sub 1 an / 1-5 ani / peste 5 ani), rata dobânzii implicite sau incrementale (IBR), și activele în leasing (categorie: clădiri, vehicule, echipamente, teren). Creează un registru de leasing complet cu: contract ID, activ, data start, durată, plată lunară/anuală, opțiuni de prelungire/terminare, opțiuni de cumpărare.

**Surse date:** Note la situații financiare (Nota privind Leasingul), contracte din dataroom (due diligence), ERP system lease module (SAP RE-FX, Oracle Lease Management).

### Pas 2: Clasificare operating vs. finance lease
Conform IFRS 16: pentru lessee, distincția operating/finance dispare — TOATE leasingurile se capitalizează (cu excepția short-term <12 luni și low-value <$5,000). Conform ASC 842 (US GAAP): un leasing este finance dacă îndeplinește oricare din 5 criterii: (1) transferă titlul de proprietate, (2) conține opțiune de cumpărare certă (bargain purchase option), (3) acoperă ≥75% din durata de viață utilă a activului, (4) PV plăți ≥90% din fair value, (5) activul este specializat fără utilitate alternativă pentru lessor.

**Eroare frecventă de evitat:** Sub IFRS 16, clasificarea operating/finance contează pentru LESSOR, nu pentru lessee. Lessee capitalizează totul. Sub ASC 842, clasificarea contează și pentru lessee (tratament P&L diferit).

### Pas 3: Calculare PV plăți viitoare și determinare rată de actualizare
Determină rata de actualizare: (1) rata implicită din contract (implicit rate) — rar disponibilă; (2) rata incrementală de împrumut (IBR — Incremental Borrowing Rate) — rata la care compania ar putea împrumuta o sumă similară, pe o durată similară, cu garanție similară. IBR = Risk-free rate + Credit spread companie + Adjustment pentru durata și moneda contractului.

**Formula PV:**
```
PV = Σ [Payment_t / (1 + IBR)^t] pentru t = 1 la N
```

**Excel:**
```
=PV(IBR_lunar, Numar_Luni, -Plata_Lunara)
```

sau pentru plăți variabile:
```
=NPV(IBR_lunar, Payment1, Payment2, ..., PaymentN)
```

Calculează separat: lease liability fără opțiuni și cu opțiuni de extindere (dacă sunt „rezonabil certe" de exercitat). Reconciliază cu suma din note — diferența trebuie explicată (discounturi, escalații, lease incentives).

### Pas 4: Înregistrare ROU asset și lease liability cu amortizare schedule
La recunoaștere inițială:
`Right-of-Use Asset = Lease Liability + Prepaid Rent - Lease Incentives + Initial Direct Costs + Restoration Costs (estimat)`

Înregistrează în bilanț: ROU Asset (imobilizări corporale) și Lease Liability (datorii: curentă = plățile din anul următor, non-curentă = restul).

**Amortization schedule (construiește în Excel):**
- Coloana: Perioada | Opening Liability | Payment | Interest (Opening × IBR/12) | Principal (Payment - Interest) | Closing Liability
- ROU Depreciation: straight-line pe durata contractului (sau viața utilă a activului, care e mai mică)
- Verificare: Closing Liability ultima perioadă = 0

**Formula Excel — Interest component:**
```
=Opening_Liability * IBR / 12
```

### Pas 5: Determinare impact EBITDA, EBIT și Net Income
Pre-IFRS 16: plata de leasing operațional merge integral în cheltuieli operaționale → reduce atât EBIT cât și EBITDA.
Post-IFRS 16: plata se divide în: (1) Depreciere ROU (reduce EBIT, NU reduce EBITDA), (2) Dobândă financiară pe lease liability (sub EBIT, nu în OpEx).

**Impact pe EBITDA:** CREȘTE cu suma totală a plăților de leasing (deprecierea se adaugă înapoi, dobânda este sub operating). Pentru o companie cu leasing semnificativ (ex: retailer cu 100 magazine închiriate), EBITDA poate crește cu 20-40%.

**Impact pe EBIT:** Efect mixt — deprecierea + dobânda pot fi mai mari sau mai mici decât plata operațională anterioară (front-loaded: în primii ani, cost IFRS 16 > payment, în ultimii ani invers).

**Impact pe Net Income:** Neglijabil pe durata contractului (total cost identic), dar profil temporal diferit (front-loaded sub IFRS 16).

**Tabel comparativ (construiește în Excel):**
| Metrică | Pre-IFRS 16 | Post-IFRS 16 | Diferență |
|---------|-------------|--------------|-----------|
| EBITDA | X | X + Lease Payment | +Y% |
| EBIT | X | X - Depr + Lease | ±Z% |
| Debt | X | X + Lease Liability | +W% |

### Pas 6: Ajustare ratiouri financiare — pre și post IFRS 16
Recalculează ratiouri critice:

**Debt/EBITDA:** Numărătorul crește (+ lease liability), numitorul crește (+ deprecierea ROU). Efect net: crește semnificativ (datoria crește mai mult decât EBITDA).
```
Debt_EBITDA_adj = (Financial_Debt + Lease_Liability) / (EBITDA + Lease_Payments)
```

**Interest Coverage (EBIT/Interest):** Scade deoarece apare dobânda pe lease liability.
```
Interest_Coverage_adj = EBIT / (Interest_Expense + Lease_Interest)
```

**Current Ratio:** Scade deoarece apare lease liability curentă.
**ROIC:** Se modifică (capital invested include ROU, NOPAT exclude lease expense dar include depreciere).

Prezintă ratiouri pre și post-IFRS 16 în paralel pentru comparabilitate. Menționează că covenant-urile bancare pot fi definite pre sau post-IFRS 16 — verifică definiția din contractul de credit!

**Eroare frecventă de evitat:** Compararea Debt/EBITDA pre-IFRS 16 al unei companii cu Debt/EBITDA post-IFRS 16 al alteia. Ajustează ambele pe aceeași bază.

### Pas 7: Comparare cu perioadele anterioare și impact pe valorizare
Construiește o tabelă de reconciliere: perioada pre-IFRS 16 vs. post-IFRS 16. Ajustează perioadele anterioare pentru comparabilitate. Impact pe DCF: (1) FCFF (Free Cash Flow to Firm): se schimbă dacă folosești EBITDA - CapEx - ΔWC (lease payments ies din OpEx și intră în „financing"), (2) EV/EBITDA: scade mecanic (EV crește cu lease liability, dar EBITDA crește mai puțin), (3) Costul capitalului: dacă incluzi lease liability în debt, WACC se modifică.

**Best practice pentru valorizare:** Tratează lease liability ca datorie financiară (adaugă la EV), dar ajustează EBITDA consistent (adaugă lease payments). Astfel EV/EBITDA devine comparabil pre și post-IFRS 16.

**Context românesc:** Companiile BVB raportează sub IFRS, deci IFRS 16 se aplică integral. Atenție la impozitul pe profit (16% standard, 1% microîntreprindere pe venituri): deprecierea ROU este deductibilă fiscal, dar dobânda pe lease are limitări de deductibilitate (thin capitalization rules).

---

## 3. Cortex Logging

```json
{
  "text": "Lease Analysis v2.0 (Operating vs. Finance) executat: contracte identificate și clasificate conform IFRS 16/ASC 842, PV calculat cu IBR, ROU asset și lease liability cu amortization schedule, impact EBITDA/EBIT cuantificat, ratiouri ajustate pre/post, impact pe valorizare DCF analizat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-020",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["lease-analysis", "IFRS-16", "ASC-842", "ROU-asset", "lease-liability", "financial-statement-analysis", "EBITDA-adjustment", "advanced"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
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
- Clasificare fără referință la criteriile IFRS 16/ASC 842 → violation
- Ratiouri prezentate fără comparare pre/post-IFRS 16 → violation
- PV calculat fără justificarea IBR → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-065 → Three-Statement Model (integrare lease liability)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-020 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Lease Analysis (Operating vs. Finance) — IFRS 16 / ASC 842" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Note la situații financiare (lease disclosures) | Sursă contracte și termeni |
| IFRS 16 / ASC 842 standard text | Criterii de clasificare |
| Excel / Google Sheets (NPV, PV functions) | PV al plăților viitoare și amortization schedule |
| Bloomberg / Reuters / BNR | Rata incrementală de împrumut (IBR) și spread-uri credit |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași (7/7) |
| VK emisie | 2/2 |
| Contracte inventariate | 100% din cele din note |
| Ratiouri ajustate prezentate | Minimum 4 (Debt/EBITDA, Interest Coverage, Current Ratio, ROIC) |
| Reconciliere PV cu valoare din note | Diferență < 5% |
| Amortization schedule construit | Da, cu verificare closing liability = 0 |
