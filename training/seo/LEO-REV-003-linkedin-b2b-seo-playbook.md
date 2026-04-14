---
type: procedure
created: 2026-03-22
status: active
slug: leo-rev-003-linkedin-b2b-seo-playbook
tags: [procedure, nexus]
---

# LinkedIn B2B & SEO Playbook — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-18
**Versiune**: 1.0
**Regula asociata**: TRAIN-S-001
**Scope**: Framework integrat pentru cresterea prezentei B2B pe LinkedIn si optimizarea SEO on-page pentru atragerea de lead-uri calificate din cautari organice.
**Origine**: Leo PROPOSAL (approved 2026-03-18)

---

## §1 Problema

Businessurile B2B din Romania (si nu doar) pierd lead-uri calificate si oportunitati de parteneriat deoarece:
- Nu au o pagina de companie LinkedIn optimizata — descriere vaga, fara cuvinte cheie, fara showcase pages
- Continutul publicat pe LinkedIn este sporadic si nealiniat cu funnel-ul de vanzari (awareness → consideration → decision)
- SEO on-page este ignorat: titlurile paginilor lipsesc, meta descriptions sunt auto-generate, nu exista strategie de cuvinte cheie
- Nu capteaza lead-uri de pe site (lipsa formulare, lipsa lead magnets, lipsa CTA-uri pe pagini cheie)
- Outreach-ul este impersonal si generic — mesaje copy-paste care genereaza rate de raspuns sub 5%

Situatii acoperite:
- Business B2B fara prezenta LinkedIn structurata
- Site de business fara SEO on-page de baza
- Business cu trafic organic dar fara conversii
- Business care vrea sa genereze lead-uri din LinkedIn + Google organic combinat

---

## §2 Procedura

### Pas 1: Optimizare pagina de companie LinkedIn
- Completeaza TOATE sectiunile: About (2000 caractere, cu keyword-uri), Website, Industry, Company size, Specialties (minim 10)
- Banner: design profesional cu tagline + CTA
- Button CTA: seteaza pe "Visit website" sau "Contact us" (link cu UTM)
- Showcase pages: creeaza pagini separate pentru servicii/produse distincte (daca ai 2+ linii de business)
- URL custom: claim /company/[brand-name]

### Pas 2: Strategie continut LinkedIn (plan lunar)
Publica minim 8 postari/luna pe pagina de companie + incurajeaza employee advocacy:
- 2x postari thought leadership (insights industrie, opinii, date)
- 2x case studies sau rezultate clienti (cu permisiune)
- 2x continut educational (how-to, framework-uri, checklists)
- 1x postare cultura companiei (echipa, events, behind the scenes)
- 1x postare promotional directa (serviciu, oferta, invitatie la call)

Formate prioritizate: document carousel (PDF upload) > text post cu hook > articol LinkedIn > video nativ. Employee advocacy: fiecare angajat distribuie 2 postari/luna.

### Pas 3: SEO on-page audit si implementare
Auditeaza si optimizeaza fiecare pagina importanta a site-ului:
- Title tag: include keyword principal + brand name, max 60 caractere
- Meta description: include keyword + CTA, max 155 caractere
- H1: un singur H1 per pagina, contine keyword-ul principal
- H2/H3: structura logica cu keyword-uri secundare
- URL: slug scurt, descriptiv, cu keyword (/servicii/[keyword])
- Internal linking: fiecare pagina are minim 3 link-uri interne relevante
- Image alt text: descriptiv, include keyword unde natural
- Page speed: target sub 3 secunde load time (testare cu PageSpeed Insights)
- Mobile responsive: verificare pe 3 dispozitive (telefon, tableta, desktop)
- Schema markup: adauga Organization, LocalBusiness, FAQ sau Product schema

### Pas 4: Strategie de keyword-uri
Construieste keyword map (1 keyword principal per pagina):
- Identifica 20-30 keyword-uri relevante cu Google Keyword Planner sau Ubersuggest
- Clasifica: informational (blog), commercial (servicii), navigational (brand)
- Atribuie 1 keyword principal fiecarei pagini importante
- Identifica long-tail keywords (3-5 cuvinte) pentru blog posts
- Analizeaza competitorii: ce keyword-uri rankeaza ei dar tu nu?
- Creeaza content gap list: keyword-uri cu volum dar fara pagina dedicata

### Pas 5: Lead capture system
Implementeaza pe site minim 3 mecanisme de captura lead-uri:
- Homepage: formular de contact vizibil above the fold + CTA principal
- Blog/Resources: lead magnet (PDF, checklist, template) cu email gate
- Pagini servicii: CTA "Solicita oferta" sau "Programeaza call" cu Calendly embed
- Exit intent popup: oferta de valoare (ghid, consultare gratuita) pe pagini cu bounce rate mare
- Footer: newsletter signup pe toate paginile

### Pas 6: LinkedIn outreach protocol
Secventa de outreach pentru generare lead-uri B2B:
1. Research: identifica 50 prospecti/luna din ICP (filtru LinkedIn: Job Title + Industry + Location)
2. Connect: trimite connection request FARA mesaj (sau cu mesaj scurt sub 30 cuvinte — "Lucram in aceeasi industrie, ma bucur sa conectam")
3. Engage: dupa acceptare, interactioneaza cu 2-3 postari ale prospectului (like + comentariu relevant)
4. Value message: dupa 5-7 zile, trimite mesaj cu valoare (articol relevant, invitatie la webinar, insight)
5. Ask: dupa 10-14 zile, propune conversatie (nu vinde direct — "Ai 15 min pentru un schimb de idei pe [topic]?")
6. Follow-up: daca nu raspunde, 1 follow-up la 7 zile, apoi stop
Rate target: 30%+ accept, 15%+ reply la mesaj de valoare, 5%+ conversie la call.

### Pas 7: Monitorizare si raportare
Lunar:
- LinkedIn: impressions, engagement rate, follower growth, click-through pe link-uri, lead-uri generate
- SEO: pozitii keyword-uri (Search Console), trafic organic, pagini indexate, Core Web Vitals
- Lead capture: formulare completate, lead magnets downloads, Calendly bookings
- Outreach: connections trimise, acceptate, reply rate, calls booked

---

## §3 Cortex Logging

```json
{
  "text": "LinkedIn B2B & SEO Playbook executat pentru [BUSINESS_NAME]: pagina LinkedIn optimizata, [X] postari planificate, SEO audit pe [X] pagini, keyword map [X] keywords, [X] mecanisme lead capture, outreach [X] prospecti. Metrici: [ORGANIC_TRAFFIC], [LINKEDIN_FOLLOWERS], [LEADS_GENERATED].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "LEO-REV-003",
    "domain": "seo",
    "rule_id": "TRAIN-S-001",
    "tags": ["linkedin", "b2b", "seo", "on-page", "lead-generation", "outreach", "keyword-strategy"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## §4 Enforcement Loop

### WHERE
Training Mode + Client onboarding B2B + SEO audit cycle

### WHEN
La onboarding client B2B + review lunar + SEO audit trimestrial complet

### HOW (violation detection)
- Pagina LinkedIn de companie cu sectiuni incomplete dupa 14 zile de la onboarding → violation
- Sub 6 postari LinkedIn/luna (din minim 8) → violation
- Pagini site fara title tag si meta description optimizate dupa audit → violation
- Niciun lead magnet activ pe site dupa 30 zile → violation
- Outreach sub 30 connections/luna → violation
- Raport lunar de performanta nerealizat → violation
- Runner: Opus audit lunar + AUDIT-PRO batch trimestrial

### CONNECT
- TRAIN-S-001 → enforcement FORGE standard
- `procedure-health.json` → entry la fiecare executie
- Relatii: M-106 (LinkedIn strategy), M-038 (SEO audit), M-010 (technical SEO)

### VERIFY
- [ ] Pagina LinkedIn completata 100%?
- [ ] Calendar continut LinkedIn creat (minim 8 postari)?
- [ ] SEO on-page audit executat pe toate paginile principale?
- [ ] Keyword map creat cu 20+ keyword-uri?
- [ ] Minim 3 mecanisme lead capture active pe site?
- [ ] Protocol outreach definit si lista prospecti creata?
- [ ] Raportare lunara configurata?
- [ ] VK emis in sesiune?

**VK-uri obligatorii:**
1. `VK [PROC] LEO-REV-003 | S1 S2 S3 S4 VER | complete`
2. `VK [CORTEX] "LinkedIn B2B & SEO Playbook" | FORGE | rule: TRAIN-S-001 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| SEO audit + keyword research | Sonnet |
| Strategie continut + outreach | Opus |
| Raportare lunara | Sonnet |
