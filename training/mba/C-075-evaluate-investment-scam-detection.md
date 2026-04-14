---
type: procedure
created: 2026-03-22
status: active
slug: c-075-evaluate-investment-scam-detection
tags: [procedure, nexus]
---

# Evaluate Investment Opportunities (Scam Detection) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Cadru structurat de due diligence pentru evaluarea oportunităților de investiție și detectarea schemelor frauduloase.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Consultați un profesionist licențiat.

---

## 1. Problema

Schemele de investiții frauduloase sunt proiectate să exploateze FOMO, grăbeala și dorința de câștig rapid. Fără un checklist sistematic de red flags și un proces de due diligence, chiar și investitorii experimentați pot fi manipulați prin presiune socială și randamente aparent atractive.

Situații acoperite:
- Oportunitate de investiție prezentată cu randamente garantate sau neobișnuit de mari
- Presiune de decizie rapidă ("oferta expiră în 24h", "locuri limitate")
- Investment vehicle neclar sau greu de verificat independent

---

## 2. Procedura

### Pas 1: Aplică regula "If it sounds too good" imediat
Orice promisiune de randament garantat > 15% anual sau "fără risc" este automat suspect. Notează promisiunea exactă. Aceasta este prima linie de screening — nu continuă fără să ai această cifră documentată și evaluată în context de piață.

### Pas 2: Verifică identitatea și licențele emitentului
Caută entitatea în registrele oficiale: SEC EDGAR (SUA), FSA/FCA (UK), ASF (România). Verifică că emitentul este licențiat pentru tipul de activitate pe care o desfășoară. Absența licenței sau a înregistrării = red flag major.

### Pas 3: Rulează checklist-ul de red flags
Bifează prezența fiecărui semnal de alarmă:
- [ ] Randamente garantate sau "fără risc"
- [ ] Presiune de timp artificială
- [ ] Strategie de investiție vagă sau "secretă"
- [ ] Recrutare de noi investitori ca sursă de randament (semn Ponzi)
- [ ] Fondatori/management fără istoric verificabil
- [ ] Documente juridice inexistente sau incomplete
- [ ] Plăți solicitate în crypto fără trasabilitate
3+ bifări = stop, nu continua.

### Pas 4: Caută surse independente și negative
Google: "[numele companiei] + scam / complaint / fraud / SEC enforcement". Verifică pe Trustpilot, BBB, Reddit. Caută recenzii negative, nu doar testimoniale pozitive (care pot fi fabricate). O căutare de 15 minute poate preveni pierderi majore.

### Pas 5: Evaluează structura de fee-uri și lichiditate
Înțelege exact: cum poți ieși din investiție? Care sunt penalitățile de retragere? Cine deține activele — tu sau o entitate terță? Dacă nu poți răspunde la toate acestea din documente oficiale, nu investi.

### Pas 6: Consultă un advisor independent (nu afiliat cu oportunitatea)
Prezintă oportunitatea unui contabil, avocat sau advisor financiar licențiat care nu primește comision din tranzacție. Dacă oportunitatea nu poate fi explicată unui profesionist extern în termeni clari, nu este înțeleasă suficient.

### Pas 7: Documentează decizia și rațiunea
Indiferent de decizie (invest sau pass), documentează în scris: ce ai evaluat, ce red flags ai găsit sau nu ai găsit, rațiunea deciziei. Revizuiește după 6 luni. Aceasta construiește un jurnal de due diligence personal.

---

## 3. Cortex Logging

```json
{
  "text": "C-075 Evaluate Investment Opportunities executat: due diligence finalizat, red flag checklist aplicat, decizie documentată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-075",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["investing", "due-diligence", "scam-detection", "FOMO", "red-flags", "financial-literacy"],
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
- Decizie de investiție luată fără verificarea licențelor emitentului → violation critică
- Red flag checklist omis sau incomplet → violation
- Investiție efectuată fără consultarea unui advisor independent → violation recomandată
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Red flag checklist complet bifat și documentat?
- [ ] Decizie cu rațiune scrisă salvată în jurnal?

**VK-uri obligatorii:**
1. `✅ [PROC] C-075 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Evaluate Investment Opportunities (Scam Detection)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| SEC EDGAR / ASF / FCA registre | Verificare licențe și înregistrare |
| Jurnal de due diligence | Documentare istorică a deciziilor |
| Advisor financiar licențiat independent | Validare externă |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Red flag checklist aplicat la 100% oportunități evaluate | 100% |
| Timp minim de due diligence per oportunitate | > 2 ore |
| Decizii documentate în jurnal | 100% |
