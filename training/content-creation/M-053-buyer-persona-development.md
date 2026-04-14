---
type: procedure
created: 2026-03-22
status: active
slug: m-053-buyer-persona-development
tags: [procedure, nexus]
---

# Buyer Persona Development — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces de cercetare și construire a profilurilor detaliate de buyer persona pentru a ghida toate deciziile de conținut, mesagerie și targeting.

---

## 1. Problema

Fără buyer personas definite cu date reale, conținutul și mesajele de marketing sunt generice și nu rezonează cu audiența corectă. Companiile pierd bugete în campanii slab targetate deoarece nu înțeleg în profunzime motivațiile, fricile și comportamentul de cumpărare al clienților ideali.

Situații acoperite:
- Lansarea unui produs sau serviciu nou care necesită definirea audienței
- Reorientarea strategiei de marketing pentru un business existent
- Segmentarea bazei de clienți existente pentru personalizare avansată

---

## 2. Procedura

### Pas 1: Colectarea datelor demografice și comportamentale
Extrage date din CRM, Google Analytics, platforme sociale și studii de piață existente: vârstă, gen, locație, nivel de educație, venit, ocupație. Identifică pattern-uri comportamentale: dispozitive folosite, ore de activitate online, platforme preferate, tipuri de conținut consumate. Datele cantitative formează scheletul obiectiv al personei.

### Pas 2: Interviuri calitative cu clienți existenți
Realizează minimum 5-10 interviuri cu clienți actuali care reprezintă clienții ideali. Întreabă despre: provocările profesionale și personale, cum au găsit soluția ta, ce alternative au considerat, ce i-a convins să cumpere, cum folosesc produsul. Interviurile dezvăluie motivații reale care nu apar în date cantitative — citatele directe sunt aur pentru copywriting.

### Pas 3: Analiza frustrărilor și pain points
Identifică top 3-5 pain points principale prin: analiza recenziilor de pe G2/Trustpilot/Amazon pentru produse similare, monitorizarea discuțiilor din comunități (Reddit, grupuri Facebook, forumuri de nișă), analiza întrebărilor frecvente din support și sales. Pain points-urile trebuie exprimate în limbajul exact al clientului — nu în jargon intern de marketing.

### Pas 4: Definirea obiectivelor și aspirațiilor
Documentează ce vrea să obțină persona: obiective profesionale (promovare, eficiență, venituri), obiective personale (timp liber, recunoaștere, securitate financiară), and succes definit din perspectiva lor. Înțelegerea aspirațiilor permite construirea mesajelor de marketing care vorbesc despre transformare, nu despre features.

### Pas 5: Mapping journey de cumpărare
Documentează cum trece persona prin procesul de cumpărare: awareness (cum descoperă problema), consideration (cum caută soluții), decision (cum alege furnizorul). Identifică touchpoint-urile cheie, obiecțiile specifice per etapă și tipul de conținut care funcționează la fiecare etapă. Acest map ghidează strategia de conținut pe toate etapele de funnel.

### Pas 6: Construirea profilului complet de persona
Compilează toate datele într-un document structurat de persona cu: nume fictiv și foto reprezentativă, sumarul demografic, quotes reprezentative din interviuri, goals principale, pain points principale, obiecții frecvente la cumpărare, canale preferate de informare, și mesaje cheie care rezonează. Adaugă o secțiune "Ce îi ține noaptea treaz" și "Ce victorie arată pentru ei."

### Pas 7: Validarea și distribuirea personelor
Prezintă personas echipei de marketing, sales și product pentru validare și îmbogățire cu perspective noi. Verifică că personas sunt credibile și recunoscute de echipa de sales ("da, acesta e clientul nostru tipic"). Distribuie persona finalizată ca document de referință utilizat activ în briefing-uri de conținut, campanii și dezvoltare de produs. Planifică revizuire semestrială.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-053 executată: Buyer persona development complet — profil detaliat construit din date cantitative, interviuri calitative și journey mapping.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-053",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["buyer-persona", "audience-research", "customer-journey", "segmentation", "content-strategy"],
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
- Persona construită fără date reale (doar presupuneri) → violation
- Conținut planificat fără referință la persona definită → violation
- Persona distribuită echipei fără validare de la sales → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 5 interviuri calitative realizate?
- [ ] Persona validată de echipa de sales?

**VK-uri obligatorii:**
1. `✅ [PROC] M-053 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Buyer Persona Development" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics / CRM | Date demografice și comportamentale |
| Typeform / Calendly | Organizare interviuri cu clienți |
| SparkToro / SimilarWeb | Research comportament online audiență |
| Notion / Google Docs | Documentare și distribuire persona |
| Content Ideation (M-050) | Persona ghidează selecția topicurilor |
| Brand Story Framework (M-054) | Persona este audiența pentru brand story |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Interviuri calitative realizate | Minimum 5 per persona |
| Personas definite per business | 2-4 personas principale |
| Adoption rate în briefing-uri | 100% campanii noi referențiază persona |
| Revizuire periodică | Semestrial |
