---
id: ARB-002
title: Reverse Charge intra-EU Auction Purchases
domain: eu-commercial-law
source: Arbitrage Pro Research — Legal Compliance Guide 2026-03-21
version: "2.0"
forge_score: "3.80"
business_mapping: ["arbitrage-pro"]
---

# ARB-002: Reverse Charge intra-EU Auction Purchases

## Obiectiv
Aplica corect mecanismul de reverse charge (taxare inversa) conform Art. 307 Cod Fiscal Romania si Directiva EU 2006/112/CE, Art. 194-199 pentru achizitii intracomunitare de bunuri din licitatii EU (Troostwijk, BVA Auctions, Surplex, Catawiki), asigurand efect TVA zero la cumparare prin auto-declarare simultana ca TVA colectat si TVA deductibil, si raportare corecta in D301 (Declaratia recapitulativa).

## Pasi

### Pasul 1 — Verifica conditiile pentru reverse charge
**Ambele conditii trebuie indeplinite simultan:**
1. **Tu (cumparatorul)** = inregistrat TVA in Romania (SRL cu cod RO + CUI)
2. **Vanzatorul (casa de licitatii)** = inregistrat TVA in alt stat membru EU (NL, DE, AT, BE etc.)

**Scenarii practice cu casele de licitatii relevante:**

| Casa de licitatii | Tara | TVA | Reverse charge? |
|-------------------|------|-----|-----------------|
| Troostwijk (troostwijk.com) | NL/DE/BE | Da, inregistrat TVA | DA — emite factura TVA 0% |
| BVA Auctions (bfrauctions.com) | NL | Da | DA |
| Surplex (surplex.com) | DE | Da | DA |
| Catawiki (catawiki.com) | NL | Da | DA |

**NU se aplica reverse charge daca:**
- Tu nu esti inregistrat TVA in Romania (PFA fara TVA sau SRL sub plafon)
- Casa de licitatii este din Romania
- Vanzatorul este persoana fizica (nu e B2B)

### Pasul 2 — Comunica numarul de TVA la achizitie
- La inregistrarea pe platforma de licitatii, introduce numarul de TVA romanesc: `RO` + CUI (ex: RO12345678)
- Casa de licitatii verifica in VIES (VAT Information Exchange System)
- Dupa validare, facturile vor fi emise FARA TVA-ul tarii vanzatorului
- **Verificare proprie**: confirma pe ec.europa.eu/taxation_customs/vies/ ca numarul tau apare valid

### Pasul 3 — Primeste si verifica factura de la casa de licitatii
**Factura trebuie sa contina:**
- Mentiunea: "TVA 0% — reverse charge — intra-community supply" (sau echivalent in limba locala)
- Numarul tau de TVA romanesc
- Numarul de TVA al casei de licitatii
- Baza impozabila (pretul ciocanului + buyer's premium, FARA TVA)

**Exemplu BVA Auctions NL:**
| Element | Valoare |
|---------|---------|
| Hammer price | €2,000 |
| Buyer's premium (21%) | €420 |
| TVA NL (21%) | €0 (reverse charge) |
| Total factura | €2,420 |

### Pasul 4 — Auto-declara TVA in Romania (efect zero)
**Mecanism dual obligatoriu in contabilitate:**
1. Inregistreaza **TVA colectat** (output): €2,420 × 19% = €459.80
2. Inregistreaza **TVA deductibil** (input): €2,420 × 19% = €459.80
3. **Efect net = €0** (zero flux de numerar)

**Nota cota TVA:** Verifica rata aplicabila — 19% standard sau 21% daca s-a modificat in 2026 prin OUG 22/2025.

**Inregistrare contabila:**
```
Debit 371 "Marfuri"           €2,420
Debit 4426 "TVA deductibila"    €459.80
    Credit 401 "Furnizori"      €2,420
    Credit 4427 "TVA colectata"  €459.80
```

### Pasul 5 — Declara achizitia intracomunitara in D300
- In decontul de TVA (D300), raporteaza:
  - Randul achizitii intracomunitare de bunuri: baza impozabila €2,420
  - TVA colectat: €459.80
  - TVA deductibil: €459.80
- Depune D300 lunar sau trimestrial conform periodicitatea firmei

### Pasul 6 — Completeaza si depune D301 (Declaratia recapitulativa)
**D301 se depune LUNAR daca ai achizitii intracomunitare.**

**Continut D301:**
- Codul de tara al furnizorului (NL, DE, BE etc.)
- Numarul de TVA al furnizorului
- Valoarea totala a achizitiilor intracomunitare pe furnizor
- Codul tranzactiei: "A" (achizitie intracomunitara de bunuri)

**Termen depunere:** pana la data de 25 a lunii urmatoare perioadei de raportare.
**Penalitati nedepunere:** amenda 1,000-5,000 RON + posibil refuz deductibilitate TVA.

### Pasul 7 — Gestioneaza scenariul B2C (fara TVA registration)
**Daca NU esti inregistrat TVA:**
- Casa de licitatii iti factureaza cu TVA-ul tarii lor (21% NL, 19% DE)
- NU poti recupera acest TVA
- **Dezavantaj cost semnificativ** — pe o achizitie de €2,000:
  - Cu reverse charge: cost TVA = €0
  - Fara TVA registration (NL): cost TVA = €420 nerecuperabil

**Concluzie:** Inregistrarea voluntara in scopuri de TVA este puternic recomandata pentru cumparatori activi de la licitatii EU.

### Pasul 8 — Actualizeaza conform OUG 22/2025
Art. 307 Cod Fiscal a fost clarificat: cand un furnizor strain (nestabilit in Romania) livreaza bunuri unui roman si NU aplica regimul de scutire IMM (art. 310^2), cumparatorul roman datoreaza TVA-ul. Aceasta intareste obligatia de reverse charge pentru achizitii B2B din licitatii EU.

## Verificare
- [ ] Numarul de TVA romanesc comunicat si validat VIES la fiecare casa de licitatii
- [ ] Facturi primite cu mentiune "reverse charge" / "TVA 0%"
- [ ] TVA auto-declarat dual (colectat + deductibil) in contabilitate
- [ ] D300 depus cu achizitii intracomunitare corect raportate
- [ ] D301 depus lunar cu toate achizitiile intracomunitare detaliate pe furnizor
- [ ] Niciun TVA strain platit pe facturi (confirma ca reverse charge este aplicat)

## Instrumente
- ANAF SPV (anaf.ro) — depunere D300, D301
- VIES (ec.europa.eu/taxation_customs/vies/) — validare numar TVA
- Software contabilitate (Saga, Oblio, SmartBill) — inregistrare automata reverse charge
- Platforme licitatii: troostwijk.com, bfrauctions.com, surplex.com, catawiki.com

## Note
- Reverse charge si schema marjei (ARB-001) sunt mutual exclusive pe acelasi bun. Daca aplici reverse charge (TVA deductibil la cumparare), TREBUIE sa vinzi sub regim standard TVA.
- Buyer's premium = 100% cheltuiala deductibila indiferent de regimul TVA ales.
- D301 nedepusa poate genera suspendare temporara a codului de TVA — risc major.
- Surse: VATupdate.com/romania, amavat.eu, kinstellar.com (OUG analyses).
- Conectat: ARB-001 (cand folosesti margin scheme in loc de reverse charge), ARB-003 (OSS la revanzare cross-border), ARB-005 (structura entitate).
