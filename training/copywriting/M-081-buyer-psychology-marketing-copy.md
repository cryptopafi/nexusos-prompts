---
type: procedure
created: 2026-03-22
status: active
slug: m-081-buyer-psychology-marketing-copy
tags: [procedure, nexus]
---

# Buyer Psychology Application in Marketing Copy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Integrarea principiilor de psihologie a cumpărătorului în copy pentru a crește persuasivitatea și conversia în mod etic.

---

## 1. Problema

Copy-ul care ignoră psihologia deciziei de cumpărare se limitează la argumente raționale care rareori motivează acțiunea. Deciziile de cumpărare sunt 80-95% emoționale, justificate ulterior rațional, iar un copywriter care nu cunoaște triggerele psihologice lasă conversii pe masă. Procedura asigură aplicarea corectă și etică a principiilor psihologice pentru maximizarea persuasivității.

Situații acoperite:
- Crearea de copy pentru o pagină de vânzare sau ad care nu convertește suficient
- Revizuirea copy-ului existent pentru a integra triggere psihologice
- Adaptarea mesajului pentru diferite stadii de awareness ale audienței

---

## 2. Procedura

### Pas 1: Identificarea stadiului de awareness al audienței
Clasifică audiența pe scala Schwartz: (1) Unaware — nu știe că are problema; (2) Problem Aware — știe problema, nu știe soluțiile; (3) Solution Aware — știe că există soluții; (4) Product Aware — știe de produsul tău; (5) Most Aware — gata să cumpere. Copy-ul și triggerele psihologice diferă radical în funcție de stadiu. Greșeala de a folosi copy de "Product Aware" pe audiență "Problem Aware" este cea mai frecventă cauză de conversie slabă.

### Pas 2: Aplicarea principiului reciprocității
Oferă valoare reală înainte de a cere ceva. Integrează în copy: conținut educativ gratuit, o perspectivă unică sau insight care rezolvă o problemă micro, un calcul sau tool util. Reciprocitatea crește probabilitatea de conversie cu până la 40% conform studiilor Cialdini. Aplică mai ales în email-uri de nurturing și conținut de blog.

### Pas 3: Activarea dovezii sociale (Social Proof)
Include minim un element de social proof specific și credibil: număr de clienți/utilizatori cu cifre reale, testimoniale cu nume complet, fotografie și rezultat măsurabil, studii de caz cu date concrete, logos de brand-uri cunoscute (dacă aplicabil), rating-uri cu număr de review-uri. Evită testimonialele vagi ("Super produs! — Ana M.") — specificitățea este esențială pentru credibilitate.

### Pas 4: Aplicarea scarcity și urgency autentice
Folosește scarcity (număr limitat) sau urgency (timp limitat) NUMAI dacă sunt reale. Scarcity falsă distruge încrederea pe termen lung. Formulele eficiente: "Ultimele X locuri disponibile", "Prețul crește la [dată]", "Bonus disponibil doar primilor X cumpărători". Adaugă countdown timer vizual dacă platforma permite. Urgency fără validitate reală = manipulation care se plătește cu reputația.

### Pas 5: Reducerea aversiunii la pierdere (Loss Aversion)
Reformulează beneficiile ca prevenire a pierderii: nu "Câștigă mai mulți clienți" ci "Nu mai pierde clienți din cauza unui site lent". Oamenii sunt de 2x mai motivați să evite o pierdere decât să obțină un câștig echivalent (Kahneman). Identifică ce pierde cititorul dacă NU cumpără și articulează concret acea pierdere.

### Pas 6: Eliminarea freei cognitive prin simplificare
Reduce freea deciziei prin: o singură ofertă principală (nu mai multe), comparator de prețuri simplu (max. 3 opțiuni, una evidențiată ca "Recomandat"), eliminiarea jargonului, bullet points scurte în loc de paragrafe, răspunsuri la obiecțiile principale în format FAQ. Fiecare element de freă cognitivă suplimentar reduce conversia cu 5-10%.

### Pas 7: Validarea etică a copy-ului finalizat
Verifică că: nicio afirmație nu este exagerată sau neadevărată, scarcity/urgency reflectă situația reală, testimonialele sunt autentice și verificabile, copy-ul nu exploatează vulnerabilități ale unor grupuri vulnerabile. Copy-ul persuasiv etic construiește încredere pe termen lung și reduce refund rate. Semnează mental "aș fi confortabil dacă clienții ar vedea cum am scris asta".

---

## 3. Cortex Logging

```json
{
  "text": "Psihologie cumpărător aplicată în copy conform M-081: stadiu awareness identificat, reciprocitate/social proof/scarcity/loss aversion integrate etic, freă cognitivă redusă, validare etică realizată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-081",
    "domain": "copywriting",
    "rule_id": "META-H-002",
    "tags": ["buyer-psychology", "persuasion", "copywriting", "cialdini", "conversion-optimization"],
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
- Copy fără social proof inclus (Pas 3 sărit) → violation
- Scarcity sau urgency folosite fără a fi autentice → violation
- Stadiul de awareness al audienței neidentificat înainte de scriere (Pas 1 sărit) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum copywriting

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Stadiul de awareness identificat și copy aliniat cu acesta?
- [ ] Validarea etică realizată la Pas 7?

**VK-uri obligatorii:**
1. `✅ [PROC] M-081 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Buyer Psychology Application in Marketing Copy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Customer research (interviews/surveys) | Date pentru stadiul de awareness și obiecții |
| Review mining tool (Helium10/G2/etc.) | Testimoniale și social proof autentice |
| Analytics / heatmap | Validarea eficacității triggerelor |
| Countdown timer tool | Implementarea urgency vizuale |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Elemente psihologice integrate | ≥ 4 din 5 principale |
| Conversion rate îmbunătățit | ≥ +25% față de copy fără psihologie |
| Refund rate / chargeback | ≤ 2% (indicator de validare etică) |
