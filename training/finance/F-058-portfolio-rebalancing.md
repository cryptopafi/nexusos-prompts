---
type: procedure
created: 2026-03-22
status: active
slug: f-058-portfolio-rebalancing
tags: [procedure, nexus]
---

# Portfolio Rebalancing Procedure — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Procedura de rebalansare periodică și bazată pe praguri a unui portofoliu de investiții pentru a menține alocările țintă.

---

## 1. Problema

Un portofoliu de investiții se abate în timp de la alocările țintă din cauza diferențelor de performanță între active. Fără rebalansare regulată, riscul portofoliului crește necontrolat. Această procedură acoperă: rebalansarea calendaristică (lunar/trimestrial), rebalansarea bazată pe praguri (deviere ±5%), și optimizarea fiscală în cadrul procesului.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definirea triggerelor de rebalansare
Stabilește două tipuri de triggere: (1) Calendaristic — rebalansare lunară sau trimestrială, indiferent de deviere; (2) Threshold-based — rebalansare imediată dacă orice clasă de active deviază cu ±5% față de alocarea țintă. Documentează în Investment Policy Statement (IPS) care trigger se aplică.

### Pas 2: Calculul greutăților curente vs țintă
Extrage valorile de piață actuale ale tuturor pozițiilor. Calculează greutatea fiecărei clase de active: `weight_actual = valoare_pozitie / valoare_totala_portofoliu`. Compară cu greutatea țintă definită în IPS. Calculează devierea: `deviere = weight_actual - weight_target`.

### Pas 3: Identificarea pozițiilor supra/subponderate
Clasifică fiecare clasă de active: SUPRAPONDERATĂ dacă deviere > +5%, SUBPONDERATĂ dacă deviere < -5%, ÎN ECHILIBRU dacă deviere ±5%. Listează pozițiile în ordinea priorității de tranzacționare — cele cu cea mai mare deviere sunt primele.

### Pas 4: Generarea listei de tranzacții
Calculează suma de vândut din pozițiile supraponderate: `suma_vanzare = (weight_actual - weight_target) × valoare_totala`. Calculează suma de cumpărat în pozițiile subponderate. Generează lista completă de tranzacții cu ticker, direcție (BUY/SELL), și valoare în RON/USD. Verifică că suma totală vânzări = suma totală cumpărări.

### Pas 5: Analiza implicațiilor fiscale și tax-loss harvesting
Verifică câștigurile/pierderile latente pentru fiecare poziție ce urmează a fi vândută. Identifică oportunități de tax-loss harvesting — vinde pozițiile cu pierderi latente pentru a compensa câștigurile realizate. Evaluează dacă amânarea vânzării unei poziții cu câștig mare până la noul an fiscal reduce sarcina fiscală. Documentează impactul fiscal estimat.

### Pas 6: Executarea tranzacțiilor
Execută vânzările înainte de cumpărări pentru a asigura lichiditatea. Folosește ordine limit pentru a evita impact de piață excesiv pe pozițiile mici. Confirmă execuția fiecărei tranzacții. Actualizează foaia de calcul cu prețurile medii de execuție.

### Pas 7: Documentarea noilor greutăți și a raționalului
Recalculează greutățile post-rebalansare și verifică că toate clasele sunt în range-ul țintă ±3%. Documentează: data rebalansării, triggerul activat, tranzacțiile executate, costurile de tranzacționare totale, impactul fiscal. Salvează raportul în sistemul de evidență al portofoliului.

---

## 3. Cortex Logging

```json
{
  "text": "Portfolio Rebalancing Procedure F-058 executat: triggere definite, greutati calculate, tranzactii generate si executate cu analiza fiscala.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-058",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["portfolio", "rebalancing", "asset-allocation", "tax-loss-harvesting", "investment"],
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
1. `✅ [PROC] F-058 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Portfolio Rebalancing Procedure" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Portfolio tracking software / Excel | Calculul greutăților și generarea listei de tranzacții |
| Broker platform | Executarea tranzacțiilor |
| Tax reporting system | Calculul implicațiilor fiscale |
| Investment Policy Statement | Definirea alocărilor țintă și triggerelor |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Deviere post-rebalansare | < ±3% față de țintă |
| Costuri tranzacționare | < 0.1% din AUM per rebalansare |
