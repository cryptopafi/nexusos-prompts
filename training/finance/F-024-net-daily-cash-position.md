---
type: procedure
created: 2026-03-22
status: active
slug: f-024-net-daily-cash-position
tags: [procedure, nexus]
---

# Net Daily Cash Position Forecasting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui forecast zilnic al pozițiilor de cash pentru identificarea gap-urilor de lichiditate, planificarea finanțării pe termen scurt, și optimizarea rendamentului pe surplusurile temporare.

---

## 1. Problema

Gestionarea lichidității zilnice fără un forecast structurat expune compania la riscuri de overdraft, penalități, sau oportunități de investment ratate. Cash flow timing mismatch-ul este cauza principală a insolvențelor tehnice — companii profitabile falimentează din lipsă de cash. Această procedură acoperă situațiile de treasury management unde trebuie să existe vizibilitate zilnică pe cash pentru minim 13 săptămâni (orizont standard rolling). Context românesc: penalitățile BNR pentru debit neautorizat sunt semnificative, iar facilitățile de overdraft au costuri de 8-12% ROBOR + marjă.

> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Construire forecast încasări zilnice cu probabilități de colectare
Identifică sursele de intrare cash: colectări AR (din aging buckets + termene de plată clienți), avansuri clienți, rambursări TVA (România: lunar sau trimestrial, depinde de statut), dobânzi primite, subvenții, alte intrări. Aplică probabilități de colectare pe fiecare aging bucket: Current (0-30 zile) → 95%, 31-60 zile → 80%, 61-90 zile → 60%, >90 zile → 30%. Distribuie încasările pe zile conform scadențelor contractuale. Pentru clienții mari (top 10 = 80% din AR): forecast individual pe client cu termene reale.

**Formula Excel — Încasări estimate zilnice:**
```
=SUMPRODUCT(AR_Bucket_Amounts, Collection_Probabilities) / Working_Days_in_Period
```

**Eroare frecventă de evitat:** Nu presupune că toți clienții plătesc la termen. Calculează DSO real per client (nu doar media) și folosește-l pentru forecast.

### Pas 2: Construire forecast plăți zilnice cu calendar fix
Identifică toate obligațiile de plată cu datele exacte: salarii (25 sau ultima zi lucrătoare), contribuții sociale CAS/CASS (25 luna următoare în România), furnizori (conform termeni DPO negociați), rate împrumuturi (schedule fix din contracte), TVA (25 luna următoare), impozit pe profit (trimestrial: 25 luna următoare trimestrului), chirii (conform contract, tipic 1-5 a lunii), utilități (conform facturi, tipic net 15-30 zile), dividende (conform hotărâre AGA). Mapează fiecare plată la data exactă sau la intervalul probabil.

**Context fiscal românesc:**
- TVA: declarare + plată până pe 25 luna următoare (lunar pentru cifra de afaceri >300,000 RON/an)
- Impozit pe profit: 16% standard, plată trimestrială
- Microîntreprindere: 1% din venituri, plată trimestrială
- CAS/CASS angajator: ~2.25% + 10% contribuție asiguratorie de muncă
- Impozit dividende: 8% (reținut la sursă)

### Pas 3: Calculare poziție netă zilnică și identificare pattern-uri
`Net Daily Cash = Opening Balance + Încasări_zilnice - Plăți_zilnice`. Calculează poziția cumulativă zilnică pe orizontul de 13 săptămâni. Identifică zilele cu poziție negativă (cash gaps) și zilele cu surplus maxim. Marchează peak-urile negative ca puncte de atenție critică. Construiește grafic de tip waterfall zilnic.

**Formula Excel (drag formula pe 91 zile):**
```
Closing_Balance_t = Closing_Balance_{t-1} + Inflows_t - Outflows_t
```

**Observă pattern-uri lunare:** tipic, zilele 25-28 au outflows majore (salarii + taxe), iar zilele 1-10 au inflows de colectare. Acest pattern creează un „valley" recurent.

### Pas 4: Identificare și clasificare gap-uri de cash
Pentru fiecare zi cu poziție negativă: calculează suma necesară de finanțare, durata gap-ului (câte zile consecutive), și dacă gap-ul este recurent (pattern lunar). Clasifică: (A) Gap structural — nevoie permanentă de finanțare (business model consumă cash), (B) Gap sezonier — recurent în anumite luni (agricultura, retail Q4), (C) Gap punctual — cauză unică identificabilă (plată echipament, dividend). Prioritizează gap-urile după magnitudine × durata × recurență.

### Pas 5: Planificare finanțare și optimizare surplus
**Pentru gap-uri:** Evaluează sursele de acoperire în ordinea costului: (1) Cash pooling inter-companii (cost zero), (2) Linie de credit revolving pre-aprobată (ROBOR + marjă, tipic 3-6%), (3) Factoring AR urgent (comision 1-3% din factura), (4) Amânare plăți furnizori cu acordul acestora, (5) Accelerare colectare (discount early payment: 2/10 net 30). Selectează sursa cu costul cel mai mic și viteza de activare necesară.

**Pentru surplusuri:** Nu lăsa cash inactiv — investește surplusul temporar: (1) Depozite overnight/1 săptămână (cel mai lichid), (2) Certificate de trezorerie (România: randament ~5-6%), (3) Money market funds, (4) Reverse repo. Calculează randamentul pierdut prin cash inactiv: `Lost_Yield = Avg_Surplus × Current_Rate × Days/365`.

### Pas 6: Menținere buffer minim de cash și stress test
Stabilește buffer-ul minim ca 5-10 zile de cheltuieli operaționale (sau conform policy intern): `Min Cash Buffer = (Total OpEx anual / 365) × zile_buffer`. Adaugă buffer-ul ca restricție în model — poziția netă nu poate scădea sub acest nivel. Rulează stress test: (1) Ce se întâmplă dacă top 3 clienți întârzie 30 zile? (2) Ce se întâmplă dacă apare o cheltuială neprevăzută de 5% din revenue? (3) Ce se întâmplă dacă linia de credit este retrasă? Alertează automat dacă forecast-ul arată breach în oricare scenariu.

### Pas 7: Automatizare, reconciliere zilnică și raportare
Construiește dashboard în Excel/Google Sheets cu: (1) Grafic 13-week rolling cash forecast, (2) Tabel gap-uri cu plan de acoperire, (3) Actual vs Forecast variance (actualizează zilnic cu datele reale). Reconciliază zilnic: soldul bancar real vs forecast. Calculează acuratețea: `Forecast_Accuracy = 1 - |Actual - Forecast| / |Actual|`. Dacă accuracy < 90% pe 3 săptămâni consecutive, revizuiește ipotezele de colectare.

**KPI-uri de raportat CFO-ului săptămânal:**
- Cash position curent vs buffer minim
- Gap-uri anticipate în următoarele 4 săptămâni
- Surplus mediu și randamentul generat
- Forecast accuracy (rolling 4 weeks)

### Pas 8: Integrare cu Cash Conversion Cycle
Calculează și monitorizează CCC: `CCC = DSO + DIO - DPO`. Unde: DSO = (AR/Revenue) × 365, DIO = (Inventory/COGS) × 365, DPO = (AP/COGS) × 365. CCC arată câte zile de cash sunt „blocate" în ciclul operațional. Fiecare zi de reducere a CCC = cash suplimentar disponibil = `Daily_Revenue × 1_zi`. Setează target de reducere CCC și monitorizează trimestrial.

**Formula Google Sheets — CCC:**
```
=DSO + DIO - DPO
```

---

## 3. Cortex Logging

```json
{
  "text": "Net Daily Cash Position Forecasting v2.0 executat: forecast 13 săptămâni construit cu probabilități de colectare, gap-uri clasificate și planificate, surplus optimizat, stress test rulat, CCC integrat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-024",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["cash-forecasting", "liquidity-management", "treasury", "CCC", "stress-test", "corporate-finance", "intermediate"],
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
- Forecast livrat fără identificarea explicită a gap-urilor → violation
- Plan de finanțare absent când există gap-uri → violation
- Stress test omis → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-025 → Short-Term Financing Evaluation (selectare surse pentru gap-uri)
- F-026 → AR Management (optimizare colectare pentru reducere DSO)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?
- [ ] Stress test cu minimum 3 scenarii?
- [ ] CCC calculat și monitorizat?

**VK-uri:**
1. `✅ [PROC] F-024 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Net Daily Cash Position Forecasting" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| AR aging report (din ERP: SAP/Oracle/Saga) | Sursa datelor de colectare cu probabilități |
| AP schedule / purchase orders | Sursa datelor de plăți furnizori |
| Payroll calendar și Declarația 112 (România) | Date fixe salarii și contribuții |
| Linie de credit revolving (contract bancar) | Instrument de acoperire gap-uri |
| Excel / Google Sheets / Treasury Management System | Construire model forecast |
| Calendar fiscal ANAF (România) | Datele limită pentru TVA, impozit profit, CAS/CASS |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași (8/8) |
| VK emisie | 2/2 |
| Orizont forecast | Minimum 13 săptămâni rolling |
| Acuratețe forecast vs. actual | Deviație < 5% pe săptămână |
| Gap-uri acoperite cu plan | 100% din gap-urile identificate |
| Stress test scenarii | Minimum 3 |
| CCC monitorizat | Trimestrial cu target de reducere |
