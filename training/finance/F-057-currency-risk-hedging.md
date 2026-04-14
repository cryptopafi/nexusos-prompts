---
type: procedure
created: 2026-03-22
status: active
slug: f-057-currency-risk-hedging
tags: [procedure, nexus]
---

# Currency Risk Assessment and Hedging — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Identificarea și cuantificarea expunerilor valutare, selectarea instrumentelor de hedging optime, și monitorizarea eficacității hedging-ului pentru companii sau portofolii cu expunere multi-currency.

---

## 1. Problema

Volatilitatea cursurilor valutare poate eroda semnificativ profitabilitatea companiilor cu operațiuni internaționale sau a portofoliilor cu active în valute străine. Fără un program structurat de hedging, o companie cu 50% din venituri în USD și costuri în EUR este expusă la impacte P&L majore la mișcări FX de 5-10%. Această procedură acoperă situațiile de corporate treasury (hedging operational) și portfolio management (hedging valutar al expunerii la active externe).


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificare și clasificare expuneri valutare
Clasifică expunerea valutară pe 3 tipuri: (1) Transaction exposure: tranzacții contractate dar nedecontate (facturi de export/import în curs, contracte semnate). Este cea mai directă și ușor de hedged. (2) Translation exposure: conversia situațiilor financiare ale filialelor externe la cursul de închidere (impactează bilanțul consolidat, dar nu cash flow). (3) Economic exposure: impactul pe termen lung al FX asupra competitivității și valorii companiei (cel mai complex — hedging prin diversificare geografică a producției). Documentează suma în fiecare valută per tip de expunere.

### Pas 2: Cuantificare expunere neta per valută pereche
Calculează expunerea netă: `Net Exposure (USD/EUR) = Total inflows USD - Total outflows USD` (pentru o companie europeană). Grupează pe scadențe: <3 luni, 3-12 luni, 1-3 ani, >3 ani. Natural hedging: dacă ai venituri și costuri în aceeași valută — netezi expunerea înainte de a hedgea (ex: dacă ai venituri USD 1M și costuri USD 600K → expunerea netă e doar USD 400K). Documentează net exposure pe fiecare pereche valutară semnificativă (>5% din revenue sau active).

### Pas 3: Analiză sensitivity P&L la mișcări FX
Calculează impactul pe P&L al mișcărilor FX: `FX P&L Impact = Net Exposure × ΔFX`. Exemplu: net exposure EUR/USD = €10M și EUR slăbește cu 5% față de USD → pierdere de €500K dacă ești exportator european. Construiește tabelă de sensitivitate: ±1%, ±3%, ±5%, ±10% mișcare FX → impact P&L în EUR/USD. Identifică nivelul de mișcare la care impactul devine material (>5% din EBITDA). Aceasta definește nevoia de hedging.

### Pas 4: Selectare instrumente de hedging
Evaluează instrumentele disponibile: (1) **FX Forward**: contractezi cursul de azi pentru o dată viitoare. Elimină total incertitudinea. Cost = diferențial de dobândă (bid-offer spread). Ideal pentru transaction exposure cu date certe. (2) **FX Options (put/call)**: plătești o primă pentru dreptul (nu obligația) de a cumpăra/vinde la un curs prestabilit. Oferă protecție + upside. Mai scump, ideal dacă nu ești sigur că tranzacția va fi finalizată. (3) **FX Swap**: swap de cash flows în valute diferite. Ideal pentru financing cross-currency. (4) **Natural hedge**: relocare producție / sourcing în aceeași valută cu vânzările. Cel mai eficient pe termen lung, dar costisitor de implementat rapid. Selectează instrumentul în funcție de certitudinea cash flow-ului, cost, și horizon temporal.

### Pas 5: Determinare hedge ratio
Nu hedgezi mereu 100% din expunere — poate fi suboptimal. Evaluează: Hedge ratio 100% → elimini total riscul FX, dar și potențialul upside dacă FX evoluează favorabil. Hedge ratio 50% → hedgezi jumătate (reduce riscul, păstrezi upside parțial). Natural hedge (dacă există) → reduces mai întâi prin netare. Hedge ratio optim depinde de: toleranța la risc FX a companiei (din politica de hedging), costul hedging-ului față de potențialul beneficiu, volatilitatea implicată a perechii valutare (dacă vol e scăzută → costul options e mai mic). Documentează hedge ratio ales și raționamentul.

### Pas 6: Implementare și documentare hedge
Tranzacționează instrumentele de hedging cu banca sau brokerul. Documentează: data tranzacției, cursul forward / premia opțiunilor, scadența, suma noțională, contraparte. Aplică hedge accounting (IFRS 9 / ASC 815) dacă relevanță: desemnează formal relația de hedging, testează efectivitatea (range 80-125%). Hedge ineficient din perspectivă contabilă: variația valorii hedging instrument se recunoaște în P&L direct (volatilitate contabilă). Hedge eficient: variația se recunoaște în OCI (Other Comprehensive Income) până la decontarea itemului hedged.

### Pas 7: Monitorizare eficacitate hedging lunar
Calculează lunar: Mark-to-market al instrumentelor de hedging, variația cursului spot față de cursul forward inițial, P&L realizat pe hedges scadente, Hedge Effectiveness Ratio (actual / expected hedge gain). Raportează CFO-ului: expuneri acoperite vs. neacoperite, costul total al programului de hedging în cursul anului, impactul net al programului vs. scenariul fără hedging. Ajustează hedge ratio sau instrumente la reînnoire dacă condițiile s-au schimbat.

---

## 3. Cortex Logging

```json
{
  "text": "Currency Risk Assessment and Hedging executat: expuneri valutare clasificate și cuantificate, sensitivity P&L calculată, instrumente hedging selectate (forward/options), hedge ratio determinat, program de monitorizare definit",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-057",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["currency-risk", "FX-hedging", "treasury", "forward-contracts", "FX-options", "intermediate"],
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
- Expunere calculată fără natural hedge netting → violation
- Hedge ratio ales fără justificare față de toleranța la risc și costul hedging-ului → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-057 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Currency Risk Assessment and Hedging" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ERP / Treasury Management System | Inventar expuneri valutare pe scadențe |
| Bloomberg FX module | Prețuri forward, implied vol pentru options |
| Bancă / broker FX | Contrapartea pentru instrumente de hedging |
| IFRS 9 / ASC 815 | Standard contabil pentru hedge accounting |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Expuneri identificate | Toate > 5% din revenue / active |
| Hedge Effectiveness Ratio | 80%-125% (range IFRS 9) |
| Raportare lunară CFO | Raport standardizat cu P&L impact net |
