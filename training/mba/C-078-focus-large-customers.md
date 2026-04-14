---
type: procedure
created: 2026-03-22
status: active
slug: c-078-focus-large-customers
tags: [procedure, nexus]
---

# Focus on Large Customers Over Small Ones — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea principiului Pareto (80/20) pentru tiering-ul bazei de clienți și realocarea resurselor spre segmentele cu impact maxim.

---

## 1. Problema

Distribuția efortului egal între toți clienții este o capcană frecventă: 20% din clienți generează tipic 80% din venituri, dar consumă sub 20% din atenție. Mica fracțiune de clienți mari are impact disproporționat și merită resurse, atenție și relații disproporționate.

Situații acoperite:
- Echipa de vânzări petrece timp egal cu clienți de toate dimensiunile
- Clienți mari nu primesc atenție dedicată sau account management personalizat
- Resurse limitate (timp, suport, produs) distribuite fără prioritizare pe valoare

---

## 2. Procedura

### Pas 1: Segmentează baza de clienți după venit și potențial
Calculează: venit anual per client (ARR/LTV), frecvența de cumpărare, potențialul de upsell. Grupează în trei tiere: A (top 20% după venit), B (mijloc 30%), C (baza 50%). Această segmentare stă la baza tuturor deciziilor de alocare resurse.

### Pas 2: Calculează costul de servire per client
Nu toți clienții mari sunt egali în profitabilitate. Calculează costul real de suport, onboarding, customizare și account management per client. Clienții C cu cereri mari de suport pot fi activ neprofitabili. Tiering-ul după profit, nu doar venit.

### Pas 3: Definește tratamentul diferențiat pe tiere
Tier A: account manager dedicat, acces prioritar la suport, invitații la events exclusive, QBR (Quarterly Business Reviews). Tier B: suport standard cu SLA mai bun, newsletter dedicat. Tier C: self-service, automatizare, resurse comune. Documentează politica în scris.

### Pas 4: Realocă 60–70% din resurse comerciale spre Tier A
Calculează câte ore/săptămână echipa petrece per tier. Ajustează: Tier A trebuie să primească cel puțin 60% din atenția comercială directă. Acest pas necesită decizii dificile de deprioritizare a clienților C.

### Pas 5: Identifică "hidden Tier A" în baza Tier B/C
Caută clienți din Tier B/C cu: creștere rapidă, industrie strategică sau potențial de referral ridicat. Aceștia merită investiție proactivă acum pentru că sunt Tier A în 12–18 luni. Creează un watchlist trimestrial.

### Pas 6: Stabilește criterii de exit pentru clienți neprofitabili
Unii clienți Tier C costă mai mult decât aduc. Definește criterii clare: LTV < 3x CAC, rata de suport > X tickets/lună, plăți întârziate recurente. La atingerea criteriilor: creșteri de preț, downgrade la self-service sau offboarding amiabil.

### Pas 7: Review trimestrial al tiering-ului
Clienții se mișcă între tiere. Revizuiește segmentarea la fiecare 90 zile. Promovează clienții B care au crescut în A, reevaluează C cu consum mare de resurse. Tiering-ul static nu reflectă realitatea dinamică a portofoliului.

---

## 3. Cortex Logging

```json
{
  "text": "C-078 Focus on Large Customers executat: tiering 80/20 aplicat, resurse realocate spre Tier A, review trimestrial efectuat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-078",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["customer-success", "80-20", "pareto", "resource-allocation", "account-management", "tiering"],
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

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Segmentare realizată fără calcul de profit per client (doar venit) → violation metodologică
- Mai puțin de 60% din resurse comerciale alocate Tier A → violation alocare
- Review trimestrial omis → violation cadență
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Tiering documentat cu criterii clare per nivel?
- [ ] Watchlist "hidden Tier A" creat și actualizat?

**VK-uri obligatorii:**
1. `✅ [PROC] C-078 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Focus on Large Customers Over Small Ones" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CRM cu date financiare per client | Calculul ARR/LTV și cost de servire |
| Politică de tiering documentată | Referință pentru tratament diferențiat |
| Watchlist "hidden Tier A" | Tracking clienți cu potențial de creștere |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Procent resurse comerciale spre Tier A | > 60% |
| Churn rate Tier A | < 5% anual |
| Review trimestrial tiering efectuat | 4x/an |
