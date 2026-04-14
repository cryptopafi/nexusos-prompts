---
type: procedure
created: 2026-03-22
status: active
slug: m-114-marketing-campaign-planning
tags: [procedure, nexus]
---

# Marketing Campaign Planning and Execution Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Planificarea structurată, execuția coordonată și evaluarea completă a campaniilor de marketing multi-canal pentru atingerea obiectivelor de business măsurabile.

---

## 1. Problema

Campaniile de marketing lansate fără un framework structurat de planificare suferă de: obiective vagi, mesaje inconsistente cross-canal, lipsă de coordonare a echipei, imposibilitate de atribuire a rezultatelor și incapacitate de iterare informată. Rezultatul sunt bugete irosite și oportunități pierdute.

Situații acoperite:
- Planificarea și execuția unui launch de produs sau serviciu nou
- Campanie promoțională sezonieră sau bazată pe un eveniment (Black Friday, Q4 push)
- Campanie de creștere a awareness sau de generare de lead-uri cu obiectiv specific

---

## 2. Procedura

### Pas 1: Definirea obiectivelor SMART și a KPI-urilor
Stabilește obiectivul campaniei în format SMART: Specific, Measurable, Achievable, Relevant, Time-bound. Exemplu: "Generarea a 500 lead-uri calificate în 30 de zile cu CPL ≤ 15€". Definește KPI-uri primare (obiectiv direct) și secundare (indicatori de sănătate). Documentează baseline-ul actual pentru fiecare KPI. Fără SMART → campania nu avansează.

### Pas 2: Definirea audienței și a persona-ului țintă
Specifică explicit: ce persona(e) vizează această campanie (referință la M-109), segmentul de audiență (frig, warm, hot), dimensiunea audienței estimată per canal. Creează mesajul core al campaniei adaptat la stadiul de conștientizare al audienței (awareness → consideration → decision). Validează că există suficient trafic/audiență pentru a atinge obiectivele.

### Pas 3: Selectarea canalelor și a mix-ului de campanie
Alege canalele pe baza: unde se află audiența țintă, costul per canal față de buget, capacitatea de execuție disponibilă. Definește rolul fiecărui canal: canal primar (volum și conversie), canal de suport (retargeting, nurturing), canal de amplificare (PR, social organic). Nu mai mult de 4 canale active simultan fără capacitate de execuție dedicată.

### Pas 4: Crearea brief-ului de campanie și a calendarului
Redactează brief-ul complet: obiectiv, audiență, mesaj core, canale, buget per canal, timeline, responsabili, deliverables per canal. Creează calendarul de conținut cu toate datele cheie: pre-campanie (teasing), lansare, momente de intensificare, follow-up post-campanie. Distribuie brief-ul tuturor celor implicați și obține confirmare de înțelegere.

### Pas 5: Producția asset-urilor și verificarea tehnică
Creează sau coordonează producția tuturor asset-urilor necesare conform brief-ului și calendarului. Verifică lista de pre-lansare: tracking setat și testat, landing page funcțional, emailuri testate pe clienți de mail, ads aprobate, UTM-uri corecte pe toate linkurile. Nici un canal nu se lansează fără verificare tehnică completă.

### Pas 6: Lansare fazată și monitorizare activă
Lansează campania fazat: 20% din buget în primele 48h pentru validare, scale la 100% după confirmarea performanței. Monitorizează zilnic în primele 7 zile: spend vs. buget, CPL/CPA față de target, CTR și CR per canal. Setează alerte automate pentru abateri de >20% față de targets. Optimizează zilnic în faza de lansare (bid-uri, audiențe, creative rotation).

### Pas 7: Post-mortem și documentare learninguri
La finalizarea campaniei, realizează post-mortem complet: rezultate finale vs. obiective SMART, performanță per canal (ce a funcționat / ce nu), insight-uri despre audiență, recomandări pentru campanii viitoare. Documentează în Cortex și actualizează procedure-health.json. Arhivează toate asset-urile cu labeluri clare pentru reutilizare.

---

## 3. Cortex Logging

```json
{
  "text": "Campanie marketing planificată și executată: obiective SMART definite, brief creat, calendar executat, post-mortem realizat cu learninguri documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-114",
    "domain": "marketing-strategy",
    "rule_id": "META-H-002",
    "tags": ["campaign-planning", "marketing-strategy", "multi-channel", "launch", "performance-marketing"],
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
- Campanie lansată fără obiective SMART documentate → violation
- Campanie lansată fără verificare tehnică completă (tracking, UTMs, landing page) → violation
- Lipsă post-mortem documentat după finalizarea campaniei → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum marketing-strategy

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Brief de campanie distribuit și confirmat de toți responsabilii?
- [ ] Post-mortem realizat și documentat în Cortex?

**VK-uri obligatorii:**
1. `✅ [PROC] M-114 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Marketing Campaign Planning and Execution Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Project management tool (Notion/Asana) | Calendarul campaniei și tracking deliverables |
| Google Analytics 4 / ad platforms | Monitorizarea zilnică a KPI-urilor |
| UTM builder | Tracking atribuire per canal |
| Creative tools (Canva/Figma) | Producția asset-urilor de campanie |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Obiective SMART atinse | ≥80% din KPI-uri |
| Post-mortem documentat | 100% din campanii |
| Timp de la brief la lansare | Conform calendar planificat |
