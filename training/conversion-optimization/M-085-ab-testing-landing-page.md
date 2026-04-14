---
type: procedure
created: 2026-03-22
status: active
slug: m-085-ab-testing-landing-page
tags: [procedure, nexus]
---

# A/B Testing and Landing Page Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proiectarea, lansarea și analiza riguroasă a testelor A/B pe landing pages pentru maximizarea ratei de conversie.

---

## 1. Problema

Deciziile de optimizare a landing pages bazate pe intuiție sau pe date insuficiente duc la modificări costisitoare care reduc în loc să crească conversia. Fără un proces riguros de testare, resursele de trafic sunt irosite, iar câștigurile rămân nerealizate.

Situații acoperite:
- Landing page cu rată de conversie sub benchmark-ul industriei sau sub target
- Lansare pagină nouă fără validare empirică a elementelor cheie (headline, CTA, layout)
- Optimizare continuă a paginilor active cu trafic semnificativ pentru creștere incrementală

---

## 2. Procedura

### Pas 1: Audit și diagnosticare inițială
Analizează landing page-ul curent folosind heatmaps (Hotjar/Microsoft Clarity), session recordings și date analitice pentru a identifica punctele de abandon și fricțiune. Documentează rata de conversie baseline, bounce rate și timp mediu pe pagină. Creează o listă prioritizată de ipoteze de testare bazate pe impactul estimat.

### Pas 2: Calculul sample size și duratei testului
Înainte de orice lansare, calculează sample size-ul necesar folosind un calculator de semnificație statistică (minimum detectable effect, power 80%, confidence 95%). Determină durata minimă a testului în zile (minim 7 zile pentru a acoperi cicluri săptămânale complete). Documentează: trafic zilnic estimat per variantă, MDE ales, sample size necesar.

### Pas 3: Formularea ipotezei de test
Scrie ipoteza în formatul: "Dacă schimbăm [element] din [stare curentă] în [stare nouă], atunci [metrică] va crește cu [X]% deoarece [raționament bazat pe date/psihologie]." Ipoteza trebuie să fie falsificabilă și să aibă o singură variabilă schimbată per test.

### Pas 4: Crearea variantelor și setarea testului
Construiește varianta B respectând principiul unui singur element modificat per test (headline, CTA, imagine hero, formular, layout). Setează testul în tool-ul ales (Google Optimize, VWO, Optimizely) cu distribuție 50/50 și tracking corect al goal-ului primar. Verifică funcționalitatea pe desktop și mobil înainte de lansare.

### Pas 5: Monitorizarea testului activ
Verifică zilnic că testul rulează corect: trafic distribuit echilibrat, tracking funcțional, fără anomalii tehnice. Nu analiza rezultatele prematur — evită "peeking problem". Documentează orice evenimente externe (campanii, seasonality) care ar putea contamina rezultatele.

### Pas 6: Analiza rezultatelor și decizia
La atingerea sample size-ului calculat SAU după durata minimă stabilită, analizează rezultatele. Declară câștigătorul doar dacă confidence level este ≥95%. Dacă testul nu a atins semnificație statistică, documentează learningul și închide testul. Nu extinde testul indefinit sperând la semnificație.

### Pas 7: Implementare, documentare și iterare
Implementează varianta câștigătoare ca nouă versiune de control. Documentează complet testul în registrul de teste: ipoteză, rezultate, uplift, learninguri. Adaugă insight-ul în backlog-ul de optimizare și planifică următorul test pe baza learningurilor acumulate. Actualizează procedure-health.json.

---

## 3. Cortex Logging

```json
{
  "text": "A/B test executat complet: ipoteză formulată, sample size calculat, test lansat și analizat la semnificație statistică 95%, rezultate documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-085",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["ab-testing", "landing-page", "cro", "statistical-significance", "optimization"],
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
- Test lansat fără sample size calculat → violation
- Test oprit înainte de 95% confidence → violation
- Varianta B modifică mai mult de un element simultan → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Sample size calculat documentat înainte de lansare?
- [ ] Test oprit doar după atingerea confidence 95% sau sample size complet?

**VK-uri obligatorii:**
1. `✅ [PROC] M-085 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "A/B Testing and Landing Page Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Optimize / VWO / Optimizely | Platform de A/B testing |
| Hotjar / Microsoft Clarity | Heatmaps și session recordings pentru diagnosticare |
| Google Analytics 4 | Tracking conversii și metrici baseline |
| Calculator semnificație statistică | Calculul sample size și power analysis |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Confidence level la declarare câștigător | ≥95% |
| Sample size acoperit | 100% din calculul inițial |
| Teste documentate în registru | 100% |
