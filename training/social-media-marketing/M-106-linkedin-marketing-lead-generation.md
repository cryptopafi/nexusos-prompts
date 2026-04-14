---
type: procedure
created: 2026-03-22
status: active
slug: m-106-linkedin-marketing-lead-generation
tags: [procedure, nexus]
---

# LinkedIn Marketing and Lead Generation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește procesul sistematic de construire a autorității pe LinkedIn și generare de lead-uri B2B prin content marketing, networking strategic și LinkedIn Sales Navigator.

---

## 1. Problema

LinkedIn este cea mai valoroasă platformă pentru lead generation B2B, dar majoritatea utilizatorilor fie nu postează deloc, fie postează conținut fără strategie — rezultând profile inactive sau mesaje de vânzare directă neinvitate care alienează potențialii clienți. O strategie bine definită transformă LinkedIn dintr-o carte de vizită digitală într-un motor de lead generation constant.

Situații acoperite:
- Profil LinkedIn sub-optimizat care nu apare în căutările potențialilor clienți
- Lipsă strategie de conținut care să atragă audiența B2B țintă
- Outreach de vânzări nepersonalizat cu rate de răspuns sub 5%

---

## 2. Procedura

### Pas 1: Optimizare profil LinkedIn pentru căutare și conversie
Completează profilul la 100% (LinkedIn All-Star). Elementele critice: headline (nu doar titlul job-ului — include beneficiul: "Ajut SaaS-urile B2B să crească MRR prin email marketing | 50+ clienți"), foto profesională (fond curat, față vizibilă), banner personalizat 1584×396px cu propunerea de valoare, secțiunea About cu problema clientului în primul paragraf (nu autobiografie). Activează modul Creator pentru acces la funcții avansate de distribuție conținut.

### Pas 2: Definire ICP (Ideal Customer Profile) și construire listă target
Definește ICPul cu maximum de specificitate: industrie, dimensiunea companiei (număr angajați sau ARR), titlu job al decidentului, locație geografică, semnale de cumpărare (ex: companie în creștere, hiring activ, a postat despre o problemă pe care o rezolvi). Construiește lista de profiluri target folosind LinkedIn Search avansat sau Sales Navigator. Target inițial: 200-500 profiluri calificate.

### Pas 3: Strategie de conținut LinkedIn (thought leadership)
Publică de 3-5 ori pe săptămână: mix de formate (postări text lungi 800-1500 cuvinte, carusele document 5-10 slide-uri, sondaje, video scurt 1-3 min). Tipuri de conținut performante pe LinkedIn: "lecții din eșecuri" (share vulnerabil), studii de caz cu cifre reale, postări cu opinie contra-intuitivă, tutoriale pas-cu-pas. Primele 2-3 rânduri sunt decisive — scrie un hook care stopează scroll-ul.

### Pas 4: Construire conexiuni strategice (nu oricine)
Trimite minimum 20-30 cereri de conexiune pe zi personalizate (nu mesajul default) exclusiv către ICP-ul definit. Mesaj de conexiune recomandat: scurt (3-4 propoziții), relevant contextual (ai văzut postarea lor / suntem în același grup / comentariu recent), fără pitch de vânzare. LinkedIn limitează cererile la ~100/săptămână — folosește-le strategic, nu aleatoriu.

### Pas 5: Outreach conversațional (nu pitch direct)
La 5-7 zile după acceptarea conexiunii (dacă persoana nu a inițiat conversația), trimite primul mesaj: de valoare, nu de vânzare. Exemple: "Am văzut că lucrezi pe [topic] — am scris recent despre [subiect relevant], cred că îți va fi util: [link]" sau o întrebare specifică legată de business-ul lor. Obiectivul primelor 2-3 mesaje: conversație, nu vânzare. Pitchul vine natural după stabilirea relației.

### Pas 6: LinkedIn Newsletter și Company Page pentru amplificare
Lansează un LinkedIn Newsletter (funcție disponibilă pentru conturi Creator) cu frecvență săptămânală sau bi-lunară — abonații primesc notificare directă, similar email. Optimizează Company Page dacă ai business: logo, descriere cu keyword-uri, publicare regulată de conținut diferit față de profilul personal. Cere angajaților să interacționeze cu postările companiei pentru amplificare organică.

### Pas 7: Tracking pipeline și metrici LinkedIn
Monitorizează săptămânal în LinkedIn Analytics: impresii, engagement rate, creștere conexiuni, SSI (Social Selling Index — target > 70). Creează un CRM simplu (Notion sau HubSpot free) pentru tracking conversații: status (conexiune trimisă / acceptată / mesaj trimis / răspuns primit / meeting programat / lead calificat / deal). Target lunar: 50 conexiuni noi din ICP, 10-15 conversații inițiate, 3-5 discovery calls programate.

---

## 3. Cortex Logging

```json
{
  "text": "LinkedIn Marketing & Lead Generation executat: profil optimizat, ICP definit, strategie content + outreach implementată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-106",
    "domain": "social-media-marketing",
    "rule_id": "META-H-002",
    "tags": ["linkedin", "lead-generation", "b2b", "thought-leadership", "outreach", "ssi"],
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
- Outreach cu pitch direct de vânzare la primul mesaj → violation
- Cereri de conexiune fără mesaj personalizat (default LinkedIn) → violation
- ICP nedefinit sau prea general (ex: "toți antreprenorii") → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum social-media-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] ICP definit cu minimum 5 criterii specifice?
- [ ] CRM/tracking pipeline activ pentru conversații LinkedIn?

**VK-uri obligatorii:**
1. `✅ [PROC] M-106 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "LinkedIn Marketing and Lead Generation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| LinkedIn Premium / Sales Navigator | Căutare avansată ICP + mesagerie extinsă |
| Dux-Soup / Expandi (cu grijă) | Automatizare parțială outreach |
| HubSpot Free / Notion | CRM tracking pipeline LinkedIn |
| Canva | Design carousel-uri și banner profil |
| LinkedIn Analytics | Monitorizare SSI și performance conținut |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| SSI (Social Selling Index) | > 70 |
| Rata de răspuns outreach | > 20% |
| Discovery calls din LinkedIn lunar | 3-5 |
