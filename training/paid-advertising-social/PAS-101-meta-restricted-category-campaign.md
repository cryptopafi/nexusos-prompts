---
id: PAS-101
title: Meta Restricted Category Campaign Setup
domain: paid-advertising-social
source: TikTok Ads Mastery — Compliance & Restricted Verticals
version: 2.0
forge_score: "estimated M/3.5"
---

# Meta Restricted Category Campaign Setup

## Obiectiv
Configurarea corectă a campaniilor Meta Ads în categorii restricționate (Credit, Housing, Employment, Social Issues/Politics), cu compliance completă SAC, targeting permis, creative guidelines, și structuri de cont care protejează contul de suspendare permanentă.

## Pași

### 1. Identificarea categoriei restricționate (Special Ad Categories — SAC)
Meta definește 4 categorii speciale obligatorii:
- **Credit**: împrumuturi, carduri de credit, refinanțare, credite ipotecare, BNPL
- **Housing**: vânzare/închiriere imobile, servicii ipotecare, asigurări locuință
- **Employment**: oferte de muncă, platforme de recrutare, agenții HR
- **Social Issues, Elections, Politics**: conținut social/politic controversat

**Regula de identificare**: dacă anunțul discriminează pe baza locației (zip), vârstei sau sexului din motive non-comerciale → probabil SAC.

**iGaming/Gambling**: categorie SEPARATĂ de SAC — necesită aprobare specifică per țară (ONJN în RO), nu simpla bifă SAC. Vezi PAS-107 pentru detalii.

### 2. Activarea SAC în Campaign Setup
- Ads Manager → Create Campaign → Campaign Details → bifează "Special Ad Category"
- Selectează categoria corectă (una sau mai multe)
- **CRITICAL**: dacă nu selectezi SAC și Meta detectează categoria → suspendare cont FĂRĂ avertizare
- Consecințe activare SAC:
  - Nu mai poți targeta după vârstă specifică, sex, cod poștal
  - Lookalike Audiences se înlocuiesc cu "Special Ad Audience" (mai larg)
  - Unele categorii de interese devin indisponibile

### 3. Targeting permis în SAC (ce poți și ce nu)
**PERMIS**:
- Geografie pe raza de min. 15 mile (25 km) în jurul unui punct sau țări/regiuni
- Interese broad (fără categorii demografice sensibile)
- Custom Audiences din lista proprie de clienți (ATENȚIE: GDPR compliant upload)
- Special Ad Audience (echivalent Lookalike, criterii non-discriminatorii)
- Comportamente de device și conexiune
- Website Custom Audiences (pixelul funcționează normal)

**INTERZIS**:
- Excludere pe baza locației specifice (zip/sector)
- Targeting după sex sau vârstă (complet blocat)
- Categorii de interese discriminatorii ("low income households", etc.)
- Detailed targeting exclusions pe criterii sensibile

**Workaround targeting**: Website Custom Audiences + Special Ad Audience recuperează parțial precizia pierdută prin SAC restrictions.

### 4. Crearea reclamelor conforme SAC
**Limbaj compliant**:
- NU: "Ideal pentru familii cu copii", "Perfect pentru tineri sub 30 ani"
- DA: "Disponibil pentru toți", "Ofertă pentru întreaga comunitate"

**Vizuale compliant**:
- NU: imagini cu o singură demografie (doar femei, doar vârstnici)
- DA: diversitate în imagini, lifestyle general
- NU: before/after medical pentru health claims

**Disclaimer legal (credit/finance)**:
- Dobânda reală și APR (obligatoriu legal + cerut de Meta)
- "*Exemplu reprezentativ. Condițiile pot varia."

**Landing page compliance**:
- Trebuie să fie consistentă cu reclama
- Aceeași ofertă pentru toți (nu redirect discriminatoriu)
- Privacy policy vizibilă + GDPR consent

### 5. CBO vs ABO în context SAC
**ABO (Ad Set Budget Optimization)** — recomandat pentru SAC la început:
- Control manual per audiență, vezi exact ce funcționează
- Ideal pentru testing: €10-20/zi per AdSet
- Poți opri audiențe individuale fără a afecta restul

**CBO (Campaign Budget Optimization)** — recomandat la scaling:
- Meta alocă bugetul automat pe AdSet-urile performante
- Mai eficient la scale mare (€200+/zi)
- Risc: Meta poate aloca totul pe un singur AdSet și ignora restul

### 6. Budget allocation formula SAC campaigns
```
Budget formula (70/20/10):
├── 70% Proven audiences (Website Custom Audiences, Email List)
├── 20% Testing (Special Ad Audiences, noi interese broad)
└── 10% Experimental (noi creative concepts, noi geo)

SAC-specific:
- Start mic: €5-20/zi primele 30 zile (construiești trust score)
- Scale progresiv: max +20% la 3 zile
- Never >30% budget increase per day (resetează learning)
```

### 7. CPM benchmarks per categorie SAC
| Categorie | CPM Meta (€) | Notă |
|-----------|-------------|------|
| Credit/Finance | €15-35 | Cel mai competitiv |
| Housing | €8-20 | Variază sezonier |
| Employment | €5-15 | Mai ieftin, audiență large |
| Social Issues | €10-25 | Restricții maxime targeting |
| Standard (non-SAC) | €5-15 | Referință baseline |

### 8. Monitorizare și prevenire suspendare
- **Account Quality**: Business Settings → Account Quality — monitorizează zilnic primele 30 zile
- **Email alerts**: activează notificări pentru reclamă respinsă
- **Reclama respinsă**: NU re-submite imediat — corectează, așteaptă 24h, apoi re-submit
- **Cont restricționat**: Account Quality → "Request Review" — NU crea cont nou (ban permanent)
- **Appeal scris**: dacă formularul standard e respins, scrie appeal detaliat cu documente business
- **Protecție cont vechi**: conturi 2+ ani cu spending record bun au toleranță mai mare — protejează-le

## Verificare
- [ ] SAC category corect selectată (nu None dacă ești în categorie)
- [ ] Targeting verificat: nicio excludere discriminatorie
- [ ] Disclaimer legal adăugat (dacă credit/finance)
- [ ] Landing page conformă (ofertă identică pentru toți + privacy policy)
- [ ] Account Quality verificat (scor OK pre-lansare)
- [ ] Budget alocat conservator primele 30 zile (€5-20/zi)
- [ ] Creative reviewed intern pentru limbaj/vizual compliant
- [ ] Recovery plan documentat (ce faci la suspendare)

## Instrumente
- Meta Ads Manager — campaign setup cu SAC toggle
- Meta Business Help Center — politici SAC detaliate și actualizate
- Meta Transparency Center — research reclame competitori în SAC
- Facebook Ad Library — inspirație creative conforme în industria ta
- Meta Account Quality — monitoring status cont real-time

## Note
- iGaming/gambling = categorie SEPARATĂ (vezi PAS-107 + ONJN licență obligatorie RO)
- Workaround-ul principal: Website Custom Audiences + Special Ad Audience = recuperare precizie targeting
- Conturi vechi (2+ ani, spending good) = toleranță mai mare — protejează-le cu orice preț
- SAC nu e opțional — non-compliance = suspendare fără avertizare
