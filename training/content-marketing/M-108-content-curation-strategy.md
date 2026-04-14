---
type: procedure
created: 2026-03-22
status: active
slug: m-108-content-curation-strategy
tags: [procedure, nexus]
---

# Content Curation Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Curatarea și redistribuirea strategică a conținutului de calitate din nișă pentru construirea autorității și menținerea consistenței de publicare.

---

## 1. Problema

Crearea de conținut original 100% este costisitoare ca timp și resurse, ceea ce duce la publicare inconsistentă sau burnout al creatorului. Content curation realizată corect completează conținutul original, adaugă perspectiva ta unică și construiește autoritate fără a crea totul de la zero. Fără un sistem, curation-ul devine aleatoriu și fără valoare adăugată percepută.

Situații acoperite:
- Crearea unui newsletter de curation săptămânal pentru audiența ta
- Completarea calendarului de conținut cu piese de curation de calitate
- Construirea autorității de nișă prin filtrarea și comentarea celor mai relevante resurse

---

## 2. Procedura

### Pas 1: Configurarea sistemului de monitorizare a surselor
Configurează RSS feeds în Feedly sau Inoreader pentru: top 20 bloguri din nișă, publicații de industrie, canale YouTube relevante, newslettere de autoritate. Adaugă Google Alerts pentru keyword-urile principale din nișă. Setează Twitter/X lists cu experții principali din domeniu. Scopul: un singur loc unde găsești tot conținutul relevant, în maximum 20 minute/zi de monitorizare.

### Pas 2: Definirea criteriilor de curatare (filtrul editorial)
Stabilești și documentezi criteriile pentru ce curatezi: relevanță directă pentru ICP-ul tău (nu orice conținut bun din nișă), publicat în ultimele 30 zile (sau evergreen de excepție), sursă credibilă și verificată, adaugă perspectivă unică pe care audiența ta nu o găsește singur. Respinge conținut: opinios fără date, promotional, irelevant pentru problemele audienței tale. Filtrul editorial este identitatea ta ca curator.

### Pas 3: Adăugarea perspectivei proprii la fiecare piesă curatată
Regula fundamentală a curation-ului de valoare: niciodată nu distribui fără a adăuga perspectiva ta. Formulele eficiente: "Iată de ce asta contează pentru [audiența ta]:", "Sunt de acord/dezacord cu X pentru că...", "Cel mai important takeaway pe care îl poți aplica azi:", "Ceea ce nu spune articolul este...". Comentariul tău transformă o simplă redistribuire într-o piesă de conținut cu valoare adăugată.

### Pas 4: Formatarea pentru canalul de distribuție
Adaptează formatul curation-ului pentru fiecare canal: Newsletter → titlu + paragraful tău de context + link + takeaway acționabil; LinkedIn → citat sau statistică cheie + perspectiva ta + link în primul comentariu; Twitter/X → thread cu 3-5 puncte cheie + perspectiva ta + link; Instagram → quote card + perspectiva ta în caption. Conținut distribuit fără adaptare per canal → violation.

### Pas 5: Crearea unui newsletter de curation ca asset principal
Dacă nu ai deja, lansează un newsletter săptămânal de curation cu structura: (1) Intro scurt — ce s-a întâmplat relevant în nișă săptămâna aceasta; (2) 3-5 resurse curatate cu perspectiva ta; (3) Un tool/resursă utilă; (4) Întrebarea săptămânii pentru engagement. Newsletterul de curation este mai sustenabil pe termen lung decât unul exclusiv de conținut original și construiește loialitate prin consistență.

### Pas 6: Crediting sursele și construirea relațiilor
Creditează întotdeauna sursa originală cu link și numele autorului. Tagează autorul pe rețelele sociale când distribui conținutul lor — aceasta crește vizibilitatea, construiește relații și poate genera reshare-uri de la autorități din nișă. Evită curation-ul fără credit (content stealing). Relațiile construite prin curation etică deschid porți pentru colaborări, guest posts și co-marketing.

### Pas 7: Măsurarea valorii și ajustarea mixului de conținut
Monitorizează lunar: engagement rate pe piesele de curation vs. conținut original, open rate newsletter (dacă curation-ul este componenta principală), feedback direct (reply-uri, comentarii despre utilitatea curation-ului). Ajustează raportul curation/original pe baza datelor. Benchmark recomandat: 30-40% curation, 60-70% conținut original. Revizuiește sursele monitorizate la fiecare 3 luni pentru relevanță.

---

## 3. Cortex Logging

```json
{
  "text": "Strategie content curation executată conform M-108: sistem monitorizare configurat, criterii filtru definite, perspectivă proprie adăugată, format adaptat per canal, newsletter creat, surse creditate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-108",
    "domain": "content-marketing",
    "rule_id": "META-H-002",
    "tags": ["content-curation", "newsletter", "content-marketing", "thought-leadership", "aggregation"],
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
- Conținut distribuit fără adaptare per canal → violation
- Conținut curat distribuit fără adăugarea perspectivei proprii → violation
- Sursă curatată distribuită fără creditarea autorului original → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Perspectiva proprie adăugată la fiecare piesă curatată?
- [ ] Sursele originale creditate cu link și autor?

**VK-uri obligatorii:**
1. `✅ [PROC] M-108 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Content Curation Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Feedly / Inoreader | Agregarea RSS feeds pentru monitorizare |
| Google Alerts | Monitorizarea keyword-urilor din nișă |
| Newsletter platform (ConvertKit/Substack) | Distribuția newsletterului de curation |
| Social media scheduler | Programarea postărilor de curation |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Piese curatate per săptămână | 5-10 |
| Open rate newsletter curation | ≥ 35% |
| Engagement rate per piesă curatată | ≥ 2% |
