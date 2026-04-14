---
type: procedure
created: 2026-03-22
status: active
slug: leo-rev-001-classification-general-vs-albastru
tags: [procedure, nexus]
---

# Classification Guide: General vs Business-Specific Procedures — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Framework de clasificare pentru determinarea daca o procedura de marketing este General (reutilizabila cross-business) sau Business-Specific (legata de un singur client/brand), cu protocol de parametrizare si migrare.
**Origine**: Leo PROPOSAL (approved 2026-03-18), enriched with Coursera/MIT/Wharton extractions

---

## §1 Problema

Agentia de marketing gestioneaza proceduri care pot fi refolosite pentru orice client (General) si proceduri care contin date, strategii sau configuratii specifice unui singur business (Business-Specific). Fara un sistem clar de clasificare:
- Se duplica efort: acelasi playbook se rescrie de la zero pentru fiecare client — agentiile pierd 15-20% din orele facturabile pe reinventare
- Se scurge IP proprietar: informatii confidentiale ale unui client ajung in template-uri generice — risc legal si reputational
- Se pierde scalabilitate: agentia nu poate livra rapid pentru clienti noi (onboarding 2-4 saptamani vs. 3-5 zile cu biblioteca)
- Se amesteca datele: KPI-uri, ICP-uri si strategii specifice polueaza procedurile reutilizabile
- Se pierde cunostinta institutionala: cand un account manager pleaca, procedurile client-specifice dispar cu el

Situatii acoperite:
- Evaluarea oricarei proceduri existente pentru clasificare General vs Specific
- Crearea unei proceduri noi cu clasificare corecta din start
- Migrarea procedurilor de la un client la biblioteca generala a agentiei
- Auditul periodic al bibliotecii de proceduri pentru consistenta
- Onboarding nou account manager cu biblioteca pre-clasificata

---

## §2 Procedura

### Pas 1: Identificare — colectarea metadatelor procedurii
Extrage din procedura evaluata: titlul, domeniul, scopul declarat, orice referinte la un business/brand/client specific. Identifica: contine nume de brand? contine date financiare specifice (bugete, marje, revenue)? contine ICP definit pentru un singur business? contine strategii care depind de un produs/serviciu particular? Colecteaza toate referintele specifice intr-o lista separata — aceasta devine baza de parametrizare la Pas 4.

### Pas 2: Aplicare framework de reutilizabilitate (STP + 4P Test)
Evalueaza procedura prin lentila marketingului strategic:
- **Segmentation Test**: Procedura descrie segmentarea pentru un singur business sau ofera framework-ul generic de segmentare (demographic, psychographic, behavioral)?
- **Targeting Test**: Targeteaza un ICP specific sau ofera metoda de definire ICP?
- **Positioning Test**: Contine positioning statement specific sau ofera framework de creare?
- **4P/7P Test**: Descrie Product/Price/Place/Promotion specifice sau ofera framework de analiza?
- **Industry Test**: Functioneaza pentru cel putin 2 industrii diferite fara modificari de substanta?

### Pas 3: Scor si clasificare cu matrice de decizie
Aplica matricea:
- **5/5 teste GENERAL** → GENERAL — procedura merge in `training/[domain]/` fara modificari
- **3-4/5 GENERAL** → GENERAL cu PARAMETRIZARE — sectiunile specifice se inlocuiesc cu placeholderi `[VARIABLE]`, se creaza companion doc cu exemple
- **1-2/5 GENERAL** → BUSINESS-SPECIFIC — ramane in directorul clientului, tag `scope: [client-name]-only`
- **0/5 GENERAL** → CONFIDENTIAL — contine IP proprietar, `confidential: true`, acces restrictionat

**Benchmark agentic**: Agentiile de top (McKinsey, BCG) mentin raport 70% general / 30% specific in biblioteca lor de frameworks. Sub 50% general = semn de scalabilitate slaba.

### Pas 4: Parametrizare si crearea versiunii generice
Pentru clasificare GENERAL cu PARAMETRIZARE:
- Inlocuieste referinte specifice cu placeholderi standard: `[BUSINESS_NAME]`, `[ICP_DESCRIPTION]`, `[BUDGET_RANGE]`, `[KPI_TARGET]`, `[INDUSTRY]`, `[MARKET_SIZE]`, `[COMPETITIVE_SET]`
- Creaza sectiune "Variabile de Instanta" la finalul procedurii cu descrierea fiecarui placeholder
- Adauga minimum 2 exemple de completare din industrii diferite (ex: SaaS B2B si hospitality)
- Pastreaza versiunea specifica in directorul clientului cu link catre procedura generica

### Pas 5: Validare cu Porter's 5 Forces Check
Verifica ca procedura generalizata nu pierde context competitiv esential:
- **Threat of substitutes**: Procedura mentioneaza substitute specifice industriei? → Parametrizeaza
- **Buyer power**: Contine dinamica de negociere specifica? → Parametrizeaza
- **Supplier power**: Contine relatii cu furnizori specifici? → Muta la client-specific
- **Competitive rivalry**: Contine analiza competitiva directa? → SWOT framework generic, date specifice la client
- **Entry barriers**: Contine bariere specifice industriei? → Parametrizeaza cu nota

### Pas 6: Documentare clasificare si metadata
Adauga in headerul procedurii:
- `Classification: GENERAL | GENERAL-PARAM | BUSINESS-SPECIFIC | CONFIDENTIAL`
- `Parametrizata din: [CLIENT_NAME]` (daca aplicabil)
- `Industrii validate: [lista]` (pentru GENERAL)
Logheaza decizia in Cortex cu rationalul de clasificare si scorul testului.

### Pas 7: Integrare in pricing model si onboarding agentie
Procedurile GENERAL sunt parte din valoarea intelectuala a agentiei:
- Include numarul de proceduri GENERAL in propunerile pentru clienti noi ("agentia are 200+ playbooks validate cross-industry")
- La onboarding client nou: identifica proceduri GENERAL relevante, creeaza instante cu date clientului
- La offboarding client: evalueaza procedurile BUSINESS-SPECIFIC pentru posibila generalizare
- **Pricing context**: Agentiile cu biblioteca structurata pot justifica retainer-ul prin time-to-value mai rapid

### Pas 8: Review periodic si lifecycle management
La fiecare AUDIT-PRO trimestrial:
- Reevalueaza procedurile BUSINESS-SPECIFIC: daca clientul nu mai este activ, evalueaza daca procedura poate fi generalizata
- Verifica procedurile GENERAL pentru actualitate (frameworks invechite, platforme schimbate)
- Masoara raportul GENERAL/SPECIFIC si ajusteaza targetul
- Identifica pattern-uri: daca acelasi tip de procedura apare la 3+ clienti, creeaza versiune GENERAL

---

## §3 Cortex Logging

```json
{
  "text": "Clasificare procedura executata: [PROCEDURE_ID] clasificata ca [GENERAL|GENERAL-PARAM|BUSINESS-SPECIFIC|CONFIDENTIAL] pe baza STP+4P Test (scor [X]/5). Parametrizare: [DA/NU]. Industrii validate: [lista].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "LEO-REV-001",
    "domain": "marketing-strategy",
    "rule_id": "TRAIN-S-001",
    "tags": ["classification", "general-vs-specific", "procedure-management", "scalability", "stp", "porter", "agency-operations"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

### WHERE
WISH Step H + Training Mode + AUDIT-PRO batch

### WHEN
La fiecare creare de procedura noua + audit trimestrial al bibliotecii

### HOW (violation detection)
- Procedura creata fara clasificare in header → violation
- Procedura cu date specifice clientului publicata in `training/` → violation critica (data leak)
- Procedura GENERAL care contine nume de brand hardcodate → violation
- Clasificare executata fara STP+4P Test complet (5 intrebari) → violation
- Procedura GENERAL-PARAM fara sectiune "Variabile de Instanta" → violation
- Raport GENERAL/SPECIFIC sub 50% fara plan de corectie → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- TRAIN-S-001 → enforcement FORGE standard
- `procedure-health.json` → adauga entry la fiecare executie
- Training Mode → curriculum marketing-strategy

### VERIFY
- [ ] Toti pasii din §2 executati?
- [ ] STP+4P Test (5 intrebari) completat integral?
- [ ] Clasificare corecta aplicata in header?
- [ ] Nicio data specifica clientului nu a scapat in procedura GENERAL?
- [ ] Porter's 5 Forces Check executat pentru GENERAL-PARAM?
- [ ] VK emis in sesiune?

**VK-uri obligatorii:**
1. `VK [PROC] LEO-REV-001 | S1 S2 S3 S4 VER | complete`
2. `VK [CORTEX] "Classification General vs Specific" | FORGE | rule: TRAIN-S-001 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Executie | Sonnet |
| Audit | Opus |

---

## §5 Dependente

| Componenta | Rol |
|-----------|-----|
| Notion / knowledge base | Stocarea si organizarea bibliotecii de proceduri |
| AUDIT-PRO batch | Audit trimestrial clasificare |
| Client directories | Stocarea procedurilor BUSINESS-SPECIFIC |
| CRM / project management | Tracking instante per client |
| procedure-health.json | Tracking executie procedura |

---

## §6 Metrics

| Metrica | Target |
|---------|--------|
| Completion rate | 100% pasi |
| VK emisie | 2/2 |
| Raport GENERAL/SPECIFIC | Minim 60% GENERAL |
| Proceduri fara clasificare | 0 |
| Timp onboarding client (cu biblioteca) | Sub 5 zile |
| Review trimestrial executat | 100% |
