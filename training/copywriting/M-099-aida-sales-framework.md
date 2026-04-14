---
type: procedure
created: 2026-03-22
status: active
slug: m-099-aida-sales-framework
tags: [procedure, nexus]
---

# AIDA Sales Framework Application — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea framework-ului AIDA în contextul vânzărilor directe și al conversațiilor de sales, adaptat pentru interacțiuni umane și scripturi de vânzare.

---

## 1. Problema

Agenții de vânzări și antreprenorii care conduc conversații de sales fără o structură psihologică clară pierd oportunități din cauza prezentării premature a ofertei sau a eșecului de a construi dorință. AIDA aplicat în sales diferă de AIDA în copywriting prin natura interactivă și adaptivă. Procedura oferă un sistem de aplicare a AIDA în sales calls, pitch-uri și scripturi de vânzare.

Situații acoperite:
- Structurarea unui script pentru sales call sau discovery call
- Pregătirea unui pitch de vânzare pentru prezentare live sau video
- Revizuirea unui proces de vânzare existent cu rata de conversie sub target

---

## 2. Procedura

### Pas 1: Pregătirea pre-call — Research și personalizare
Înainte de orice interacțiune de sales, cercetează prospectul: industria și provocările specifice, posibila poziție în customer journey, orice informație publică relevantă (LinkedIn, site, conținut publicat). Personalizează abordarea AIDA pentru contextul specific al prospectului — un script generic fără personalizare are rata de conversie cu 40-60% mai mică.

### Pas 2: A — Attention: Deschiderea care captează în 30 secunde
Deschide cu o afirmație sau întrebare care conectează direct cu o problemă sau oportunitate a prospectului, nu cu prezentarea companiei tale. Formula eficientă: "Lucrez cu [tipul de business], iar cea mai frecventă problemă pe care o aud este [X]. Ați întâlnit și voi asta?" Alternativ: o statistică șocantă releventă pentru industria lor. Evită deschiderile despre tine sau produsul tău.

### Pas 3: I — Interest: Aprofundarea problemei prin întrebări
Folosește întrebări deschise pentru a aprofunda problema și a crea interes față de soluție. Tehnica SPIN: Situation (context), Problem (problema specifică), Implication (consecințele problemei), Need-Payoff (valoarea rezolvării). Nu oferi soluții în această etapă — scopul este ca prospectul să articuleze singur impactul problemei. Ascultă activ și ia note.

### Pas 4: D — Desire: Construirea dorinței pentru soluția ta
Prezintă soluția ca răspuns direct la problemele și implicațiile articulate de prospect. Conectează fiecare caracteristică la un beneficiu specific menționat de prospect: "Ai menționat că [problemă X] — exact pentru asta am construit [feature Y], care [rezultat Z]". Folosește studii de caz cu companii similare. Desire se construiește prin relevanță specifică, nu prin liste de caracteristici.

### Pas 5: A — Action: Ghidarea spre next step clar
Propune un next step specific, nu vag: nu "Ce ziceți?" ci "Haideți să programăm o demonstrație pentru săptămâna viitoare — marți sau joi vă convine mai bine?". Folosește alternative close (2 opțiuni) sau assumptive close (presupui că vor merge mai departe). Dacă există ezitare, adresează obiecția specifică cu risk-reversal relevant.

### Pas 6: Gestionarea obiecțiilor în cadrul AIDA
Obiecțiile apar de obicei în faza D sau A. Framework pentru obiecții: Acknowledge (recunoaște obiecția fără a o respinge) → Clarify (înțelege dacă e o obiecție reală sau un reflex) → Respond (oferă răspunsul relevant) → Confirm (verifică dacă obiecția a fost adresată). Nu trece mai departe fără a confirma că obiecția a fost rezolvată. Documentează obiecțiile recurente pentru îmbunătățirea scriptului.

### Pas 7: Post-call follow-up și documentarea în CRM
În maxim 2 ore după call: trimite email de follow-up cu summarul conversației, next steps agreate și materialele promise. Documentează în CRM: stadiul prospectului, obiecțiile întâlnite, data next step-ului. Actualizează scriptul AIDA dacă ai identificat o nouă obiecție frecventă sau o abordare care a funcționat mai bine.

---

## 3. Cortex Logging

```json
{
  "text": "AIDA Sales Framework aplicat conform M-099: research pre-call, toate 4 faze AIDA parcurse, obiecții gestionate, next step agresat, follow-up trimis, CRM actualizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-099",
    "domain": "copywriting",
    "rule_id": "META-H-002",
    "tags": ["AIDA", "sales", "framework", "script", "conversion", "copywriting"],
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
- Script de sales scris fără testarea a min. 3 variante de deschidere → violation
- Copy/script fără social proof (studii de caz) inclus în faza Desire → violation
- Follow-up post-call trimis după mai mult de 24 ore → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum copywriting

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Research pre-call realizat pentru personalizare?
- [ ] Next step specific agresat și confirmat la finalul interacțiunii?

**VK-uri obligatorii:**
1. `✅ [PROC] M-099 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AIDA Sales Framework Application" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CRM (HubSpot/Pipedrive/etc.) | Documentare și tracking prospeccți |
| Call recording tool (Gong/Chorus) | Analiza și îmbunătățirea call-urilor |
| LinkedIn / research tools | Research pre-call |
| Email platform | Follow-up automatizat post-call |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rata de conversie call → next step | ≥ 40% |
| Follow-up trimis în 2 ore | 100% din call-uri |
| Rata de închidere deal | ≥ benchmark industrie +15% |
