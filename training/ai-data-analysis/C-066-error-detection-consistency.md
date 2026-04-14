---
type: procedure
created: 2026-03-22
status: active
slug: c-066-error-detection-consistency
tags: [procedure, nexus]
---

# Apply Error Detection and Consistency Checking — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Verificarea sistematică a output-urilor AI pentru erori factuale, logice, de formatare și hallucinations, asigurând calitatea și consistența livrabilelor.

---

## 1. Problema

AI-urile generează output-uri cu erori subtile — fapte incorecte, inconsistențe logice între secțiuni, hallucinations prezentate cu încredere. Fără un proces structurat de verificare, aceste erori ajung în livrabile finale, decizii sau publicații. Această procedură creează un sistem de control al calității aplicabil oricărui output AI critic.

Situații acoperite:
- Rapoarte de analiză generate cu AI
- Conținut de marketing sau publicații bazate pe AI
- Cod generat de AI înainte de deployment
- Recomandări strategice produse cu asistență AI

---

## 2. Procedura

### Pas 1: Define — Definește tipologia erorilor relevante pentru contextul specific
Identifică categoriile de erori aplicabile: (a) Factuale — date, cifre, nume, date calendaristice incorecte; (b) Logice — contradicții interne, inferențe incorecte, cauzalitate falsă; (c) Formatare — structură, lungime, ton neconform cerințelor; (d) Hallucinations — referințe inexistente, citate false, statistici inventate; (e) Omisiuni — informații critice lipsă. Documentează checklist-ul specific domeniului.

### Pas 2: Setup — Construiește verificatorul de consistență
Pregătește: (1) surse de adevăr verificabile (documente originale, baze de date, surse autorizate); (2) template de checklist cu toate tipurile de erori din Pasul 1; (3) criterii de severitate (Minor / Major / Blocker). Minor = notă de corecție, Major = re-generare, Blocker = respingere completă.

### Pas 3: Run Consistency — Verifică consistența internă a output-ului
Compară toate secțiunile output-ului între ele. Caută: cifre menționate în mai multe locuri care diferă, afirmații contradictorii, timeline-uri inconsistente, terminologie folosită inconsecvent. Marchează fiecare inconsistență cu localizare exactă (secțiune, paragraf).

### Pas 4: Cross-reference — Verifică faptele față de surse externe
Selectează toate afirmările factuale (cifre, date, citate, statistici) și verifică-le manual față de sursele de adevăr stabilite în Pasul 2. Prioritizează: cifrele financiare > citatele atribuite > statisticile > afirmațiile generale. Documentează verificările efectuate.

### Pas 5: Flag — Cataloghează toate discrepanțele identificate
Creează un log de discrepanțe: tipul erorii | localizare | severitate | corecție propusă. Calculează rata de eroare globală (nr. erori / nr. afirmări verificate × 100).

### Pas 6: Correct and Re-validate — Corectează și re-validează
Aplică corecțiile, re-rulează verificările din Pașii 3-4 pe secțiunile corectate. Dacă rata de eroare depășește 10%, analizează cauza rădăcină (prompt slab, date de input incorecte, model nepotrivit) și re-generează output-ul complet cu îmbunătățiri structurale.

---

## 3. Cortex Logging

```json
{
  "text": "Error Detection aplicat pe [document/output]. Erori identificate: [X total] (factuale: [a], logice: [b], hallucinations: [c]). Rată eroare: [%]. Decizie: [aprobat/re-generat].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-066",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["error-detection", "consistency-checking", "hallucination", "quality-control", "ai-output-validation"],
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
La orice output AI critic înainte de livrare sau utilizare în decizie

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Cross-referencing (Pasul 4) omis pentru output cu afirmări factuale → violation critică
- Rată eroare >10% fără re-generare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] C-066 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Error Detection and Consistency Checking" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Surse de adevăr verificabile | Referință pentru cross-referencing |
| Checklist tipologie erori | Template verificare Pasul 1 |
| Log de discrepanțe | Output structurat Pasul 5 |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rată eroare output final | < 5% |
| Afirmări factuale verificate | Min. 100% din cele critice |
