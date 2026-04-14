---
type: procedure
created: 2026-03-22
status: active
slug: c-040-coinbase-affiliate-program
tags: [procedure, nexus]
---

# Earn Through Coinbase Affiliate Program — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru înscrierea, configurarea și optimizarea câștigurilor prin programul de afiliere Coinbase, cu respectarea cerințelor de disclosure.

---

## 1. Problema

Programul de afiliere Coinbase oferă comisioane pentru referrals, dar mulți creatori de conținut crypto nu îl configurează corect, nu îl integrează strategic în conținut, sau nu respectă cerințele de disclosure FTC — ceea ce poate duce la câștiguri ratate sau la consecințe legale.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Creator de conținut crypto (YouTube, blog, Twitter) care vrea să monetizeze audiența
- Educator crypto care recomandă exchanges în cursuri sau tutoriale
- Developer sau operator de website cu trafic relevant crypto

---

## 2. Procedura

### Pas 1: Verifică eligibilitatea și aplică la programul Coinbase Affiliates
Accesează affiliate.coinbase.com și verifică cerințele curente: trebuie să ai un website sau canal cu trafic relevant, să fii din o țară eligibilă (verifică lista — variază), și să respecți politicile de conținut Coinbase. Completează formularul de aplicare cu informații reale și corecte despre platforma ta.

### Pas 2: Înțelege structura comisioanelor
Coinbase Affiliates plătește de obicei o sumă fixă per user nou care se înregistrează și tranzacționează (nu % din tranzacții). Verifică termenii actuali pe dashboard-ul de afiliere pentru ratele exacte — se modifică periodic. Calculează câți referrals ai nevoie lunar pentru un venit semnificativ față de traficul tău actual.

### Pas 3: Generează link-urile de afiliere personalizate
În dashboard-ul Coinbase Affiliates, generează link-uri cu parametrii de tracking pentru fiecare canal (YouTube description, blog post, Twitter bio). Creează link-uri diferite per canal pentru a putea analiza care sursă convertește cel mai bine. Salvează link-urile într-un document organizat.

### Pas 4: Integrează link-urile strategic în conținut existent
Adaugă link-urile de afiliere în: descrierile de YouTube ("Cumpără crypto pe Coinbase → [link]"), articolele de blog relevante (tutoriale "cum cumperi Bitcoin"), secțiunea bio Twitter (prin Linktree), și email newsletter dacă există. Prioritizează plasamentul în conținut evergreen cu trafic constant.

### Pas 5: Adaugă disclosure obligatoriu pe tot conținutul cu link-uri afiliate
Conform cerințelor FTC și GDPR: orice conținut cu link-uri de afiliere trebuie să declare explicit relația comercială. Formulare acceptate: "Acest link este un link de afiliere — primesc un comision dacă te înregistrezi prin el, fără costuri suplimentare pentru tine." Adaugă în descrierile video, la începutul articolelor, și în bio.

### Pas 6: Monitorizează performanța și optimizează
Verifică dashboard-ul Coinbase Affiliates săptămânal: clicks, conversions, earnings. Identifică ce canale și ce tipuri de conținut convertesc cel mai bine. Crează mai mult conținut de același tip. Testează diferite plasamente ale link-urilor (CTA la început vs final). Documentează what works.

### Pas 7: Compilează raportul lunar și procesează plata
La finalul fiecărei luni verifică balanța și solicită plata (sau configurează auto-payout dacă disponibil). Documentează venitul din affiliate pentru evidența fiscală (este venit impozabil). Salvează în Cortex raportul lunar cu earnings și top performing content.

---

## 3. Cortex Logging

```json
{
  "text": "Program afiliere Coinbase configurat. Link-uri generate per canal, disclosure adăugat pe tot conținutul, monitorizare săptămânală activată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-040",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "affiliate", "coinbase", "personal-finance", "monetization", "compliance"],
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
- Link-uri de afiliere publicate fără disclosure FTC/GDPR → violation legală
- Link-uri generate fără tracking per canal → violation (imposibil de optimizat)
- Venituri afiliate nedocumentate pentru evidența fiscală → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Disclosure prezent pe TOATE canalele cu link-uri afiliate?
- [ ] Link-uri separate per canal pentru tracking?

**VK-uri obligatorii:**
1. `✅ [PROC] C-040 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Earn Through Coinbase Affiliate Program" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Coinbase Affiliates Dashboard | Management link-uri și earnings |
| Linktree | Centralizare link-uri în bio social |
| Google Analytics | Tracking trafic și conversii |
| Spreadsheet | Organizare link-uri per canal |
| Evidență fiscală | Documentare venit impozabil |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Canale cu link-uri activate | Toate canalele relevante |
| Disclosure prezent | 100% din conținut cu link-uri |
| Click-through rate (CTR) | Documentat per canal |
| Conversie (signup/click) | Benchmarked și optimizat |
| Raport lunar earnings | Documentat pentru fiscal |
