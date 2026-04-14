---
type: procedure
created: 2026-03-22
status: active
slug: f-060-cognitive-bias-audit
tags: [procedure, nexus]
---

# Cognitive Bias Audit for Investment Decisions — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Identificarea, scorarea și mitigarea bias-urilor cognitive care distorsionează deciziile de investiție.

---

## 1. Problema

Bias-urile cognitive sunt surse sistematice de erori în deciziile de investiție, ducând la cumpărări la maxime, vânzări la panice, sau concentrare excesivă în poziții familiare. Fără un audit periodic, investitorul nu poate distinge între convicție rațională și raționalizare bias-ată. Această procedură acoperă: identificarea celor 5 bias-uri principale, auditul deciziilor recente, scorare, și implementarea regulilor de mitigare.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificarea celor 5 bias-uri principale de auditat
Definește și documentează bias-urile relevante pentru investiții: (1) **Anchoring** — fixarea pe un preț de referință (ex: prețul de cumpărare); (2) **Confirmation bias** — căutarea doar a informațiilor care confirmă teza existentă; (3) **Herding** — urmarea mulțimii fără analiză independentă; (4) **Loss aversion** — frica de pierderi de 2x mai puternică decât bucuria câștigurilor; (5) **Overconfidence** — supraevaluarea abilității de a prezice piața.

### Pas 2: Revizuirea ultimelor 5 decizii de investiție
Listează ultimele 5 decizii majore (cumpărare/vânzare/hold). Pentru fiecare decizie documentează: data, instrumentul financiar, raționalul documentat la momentul deciziei, surse de informații consultate, și outcome-ul actual. Asigură-te că ai jurnalul de investiții disponibil pentru această analiză.

### Pas 3: Scorarea prezenței fiecărui bias (scala 0-3)
Aplică scala: 0 = absent, 1 = posibil prezent, 2 = probabil prezent, 3 = cert prezent. Pentru fiecare din cele 5 decizii, scorează fiecare bias. Completează matricea 5×5 (decizii × bias-uri). Exemplu de întrebări: "Am ignorat știri negative despre această poziție?" (confirmation), "Am vândut pentru că toți vândeau?" (herding).

### Pas 4: Calculul scorului total de expunere la bias
Sumează scorurile pe coloane (per bias) și pe rânduri (per decizie). `Scor total = Σ toate scorurile` (max 75 pentru 5 decizii × 5 bias-uri × max 3). Clasificare: 0-15 = expunere redusă, 16-35 = expunere moderată (atenție), 36-75 = expunere ridicată (acțiune imediată). Identifică bias-ul dominant (coloana cu cel mai mare scor).

### Pas 5: Implementarea regulilor de mitigare a bias-urilor
Adoptă reguli specifice pentru bias-urile cu scor ridicat: **Pre-mortem analysis** — înainte de orice decizie, documentează 3 scenarii de eșec; **Devil's advocate** — desemnează un prieten/advisor să argumenteze contra poziției tale; **Rules-based triggers** — definiți în avans regulile de exit (stop-loss, take-profit) fără emoție; **Waiting period** — 48h de așteptare pentru decizii mari; **Checklist** — checklist standard înainte de fiecare tranzacție.

### Pas 6: Documentarea auditului și planul de remediere
Completează raportul de audit: bias-uri identificate, scoruri, decizii afectate, și planul de acțiune. Stabilește date de revizuire (lunar). Dacă un bias specific are scor > 10, implică un external accountability partner. Documentează ce reguli noi introduci și cum le vei verifica.

---

## 3. Cortex Logging

```json
{
  "text": "Cognitive Bias Audit F-060 executat: 5 bias-uri analizate pe 5 decizii recente, scor total calculat, reguli de mitigare implementate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-060",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["cognitive-bias", "behavioral-finance", "investment-psychology", "pre-mortem", "decision-making"],
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
1. `✅ [PROC] F-060 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Cognitive Bias Audit for Investment Decisions" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Jurnal de investiții | Documentația deciziilor anterioare pentru audit |
| Matricea de scoring 5×5 | Instrument de scoring bias-uri |
| Accountability partner | Validare externă pentru bias-urile severe |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Scor total bias | < 20 (expunere redusă-moderată) |
| Frecvență audit | Lunar |
