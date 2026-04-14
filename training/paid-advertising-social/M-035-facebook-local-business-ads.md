---
type: procedure
created: 2026-03-22
status: active
slug: m-035-facebook-local-business-ads
tags: [procedure, nexus]
---

# Local Business Facebook Ads Campaign — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea și rularea campaniilor Facebook Ads pentru business-uri locale cu targetare geografică precisă și obiective de trafic fizic sau conversii locale.

---

## 1. Problema

Business-urile locale risipesc bugetul publicitar prin targetare geografică prea largă sau prin folosirea obiectivelor de campanie nepotrivite. Fără o strategie adaptată la contextul local (raza de acțiune, ore de funcționare, oferte locale), campaniile generează impresii fără impact real în trafic fizic sau apeluri telefonice.

Situații acoperite:
- Lansare campanie pentru business local nou (restaurant, salon, clinică, magazin fizic)
- Optimizare campanie locală cu cost per lead ridicat sau trafic fizic scăzut
- Campanie promoțională sezonieră sau eveniment local cu window scurt de timp

---

## 2. Procedura

### Pas 1: Definire zona geografică și profilul clientului local
Stabilește raza de targetare realistă pentru business: 2-5 km pentru servicii zilnice (restaurant, coafor), 10-20 km pentru servicii specializate (clinici, service auto), 30-50 km pentru destinații (centre comerciale, parcuri de agrement). Definește profilul demografic al clientului local: vârstă, gen, interese relevante pentru zona respectivă. Evită targetarea națională chiar dacă business-ul are notorietate mai largă.

### Pas 2: Configurare targetare locală avansată
Creează ad set-ul cu targetare precisă: selectează "People living in this location" (nu "People recently in this location" sau "People traveling in this location", decât dacă e cazul specific). Adaugă locația cu pin de adresă exactă + raza km. Exclude zone în care business-ul nu poate livra servicii. Activează Local Awareness targeting dacă este disponibil în contul tău.

### Pas 3: Alegerea obiectivului de campanie potrivit
Selectează obiectivul în funcție de scopul campaniei: Store Traffic (dacă locația fizică este verificată în Business Manager) pentru a maximiza vizitele în magazin, Lead Generation pentru programări și rezervări, Calls (obiectiv apeluri) pentru servicii care funcționează pe baza de consultare telefonică. Evită obiectivul Traffic (link clicks) pentru business-uri locale — nu generează acțiuni cu valoare reală.

### Pas 4: Creare reclame cu elemente locale
Scrie copy cu referințe geografice concrete: menționează cartierul, strada sau landmark-urile apropiate (ex: "În inima Floreasca", "La 2 minute de Piața Unirii"). Include în reclamă: adresa fizică, orele de program, numărul de telefon cu CTA "Sună acum". Folosește imagini reale ale locației sau echipei — nu stock photos. Video de 15-30 secunde cu locația și atmosfera reală crește semnificativ rata de conversie locală.

### Pas 5: Configurare call-to-action și landing page local
Setează CTA button-ul relevant: "Get Directions" (direcții Google Maps), "Call Now" (apel direct), "Book Now" (rezervare), "Send Message" (Messenger). Dacă CTA-ul duce la site, asigură-te că landing page-ul este mobile-optimized și conține harta Google Maps embed, numărul de telefon clickabil și un formular de rezervare simplu. Timpii de răspuns la mesaje sub 1 oră cresc scorurile reclamelor locale.

### Pas 6: Setare buget și programare temporală
Alocă bugetul zilnic în funcție de dimensiunea audienței locale (regula: minim 5 RON per 1.000 persoane în audiență, pentru audiențe mici bugetul minim de 20-30 RON/zi este necesar). Activează Ad Scheduling pentru a rula reclamele doar în orele relevante (ex: restaurant → prânz 10-14 și seară 17-21, nu la 3 AM). Setează o limită de frecvență de maxim 2-3 afișări per persoană pe săptămână pentru audiențe mici.

### Pas 7: Tracking rezultate locale și ajustare
Conectează Facebook Page cu profilul Google Business Profile pentru a urmări corelația între reclame și reviews/vizite. Urmărește metricele specifice local: Cost per Store Visit (dacă activat), Cost per Direction Click, Cost per Call, Cost per Message. Rulează campania minim 14 zile înainte de ajustări majore. Analizează zilele și orele cu cel mai bun cost per acțiune și concentrează bugetul în acele ferestre.

---

## 3. Cortex Logging

```json
{
  "text": "Campanie Facebook Ads pentru business local configurată — targetare geografică precisă cu raza km, obiectiv adecvat (Store Traffic/Leads/Calls), reclame cu elemente locale, ad scheduling activ, tracking local configurat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-035",
    "domain": "paid-advertising-social",
    "rule_id": "META-H-002",
    "tags": ["facebook", "local-ads", "local-business", "geo-targeting", "store-traffic"],
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
- Obiectiv Traffic (link clicks) folosit pentru business local în loc de Store Traffic/Leads/Calls → violation
- Targetare geografică setată pe nivel național pentru business cu acoperire locală → violation
- Reclame fără referințe geografice sau elemente vizuale locale → violation
- Ad Scheduling absent pentru business cu ore de program fixe → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-social

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Raza geografică definită realist pentru tipul de business?
- [ ] Ad Scheduling configurat pentru orele de funcționare?

**VK-uri obligatorii:**
1. `✅ [PROC] M-035 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Local Business Facebook Ads Campaign" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Ads Manager | Creare și management campanii locale |
| Facebook Page (locație verificată) | Activare obiectiv Store Traffic |
| Google Business Profile | Corelarea datelor de vizite locale |
| Facebook Messenger | Canal de conversie pentru mesaje directe |
| Google Maps Embed | Element esențial pe landing page local |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Cost per Direction Click | Stabilit per industrie locală |
| Cost per Call | < valoarea unui client nou / 10 |
| Cost per Store Visit | Stabilit per industrie |
| Frecvență săptămânală | < 3.0 afișări per persoană |
| Rată răspuns Messenger | < 1 oră |
