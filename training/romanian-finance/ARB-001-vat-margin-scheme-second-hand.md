---
id: ARB-001
title: VAT Margin Scheme Second-Hand (Art. 312 CF)
domain: romanian-finance
source: Arbitrage Pro Research — Legal Compliance Guide 2026-03-21
version: "2.0"
forge_score: "3.80"
business_mapping: ["arbitrage-pro"]
---

# ARB-001: VAT Margin Scheme Second-Hand (Art. 312 CF)

## Obiectiv
Aplica corect regimul special TVA al marjei de profit (schema marjei) conform Art. 312 Cod Fiscal Romania (transpunere Directiva 2006/112/CE, art. 311-325) pentru bunuri second-hand achizitionate din licitatii EU si revandute pe piata romaneasca sau EU, calculand TVA doar pe marja de profit (diferenta pret vanzare - pret cumparare) si NU pe pretul integral de vanzare, prevenind dubla impozitare si reducand sarcina fiscala efectiva cu 30-50% comparativ cu regimul standard TVA.

## Pasi

### Pasul 1 — Verifica eligibilitatea bunului pentru schema marjei
**Conditii obligatorii — bunul trebuie cumparat de la:**
1. O persoana neimpozabila (persoana fizica / particulara)
2. O entitate scutita de TVA (microintreprindere sub plafonul de scutire)
3. Alt revanzator TVA care a aplicat si el schema marjei (factura fara TVA separat)
4. O persoana impozabila care a cumparat bunul pentru uz personal (TVA nedeductibil la achizitie)

**NU poti aplica schema marjei cand:**
- Casa de licitatii a facturat TVA standard pe pretul ciocanului si ti-a emis factura normala cu TVA deductibil
- Bunurile sunt noi (nu second-hand)
- Vanzatorul si-a dedus TVA-ul la achizitia initiala

**Regula practica pentru licitatii EU:** Verifica factura de la casa de licitatii. Daca primesti factura cu TVA standard deductibil → regim standard TVA la revanzare. Daca primesti factura margin-scheme sau bunuri din lichidari private → aplica schema marjei.

### Pasul 2 — Calculeaza TVA prin formula "suta marita"
**Formula:**
```
Marja = Pret vanzare (inclusiv comisioane) - Pret cumparare (inclusiv buyer's premium)
TVA = Marja × (19/119)    — la cota standard de 19%
TVA = Marja × (21/121)    — la cota de 21% (verificare 2026, post OUG 22/2025)
```

**Cazuri speciale:**
- Daca Marja ≤ 0: TVA = 0 (zero pentru tranzactia respectiva)
- Buyer's premium de la casa de licitatii = inclus in pretul de cumparare pentru calculul marjei
- Comisioanele platformei de vanzare (OLX/eBay) = cheltuiala deductibila separata, NU reduc marja

**Exemplu practic:**
| Element | Valoare |
|---------|---------|
| Pret cumparare Troostwijk (hammer + premium) | €500 |
| Transport NL→RO (1 pallet) | €120 |
| Pret vanzare OLX | €900 |
| Marja = €900 - €500 | €400 |
| TVA datorat = €400 × (19/119) | €63.87 |
| vs. TVA regim standard = €900 × 19% | €171.00 |
| **Economie TVA** | **€107.13** |

Nota: transportul (€120) este cheltuiala deductibila separata, nu intra in calculul marjei.

### Pasul 3 — Inregistreaza in Registrul bunurilor second-hand cumparate
**Obligatoriu conform Art. 312 CF. Registrul trebuie sa contina INDIVIDUAL pe fiecare bun:**
- Numar de ordine
- Data achizitiei
- Furnizor (nume, adresa, cod fiscal daca exista)
- Descrierea bunului (categorie, stare, numar de serie daca exista)
- Pretul de cumparare (inclusiv buyer's premium)
- Numarul facturii de achizitie
- Mentiunea "schema marjei" pe fiecare intrare

### Pasul 4 — Inregistreaza in Registrul bunurilor second-hand vandute
**La fiecare vanzare, registrul trebuie sa contina:**
- Numar de ordine corelat cu registrul de cumparari
- Data vanzarii
- Cumparator (date identificare)
- Descrierea bunului
- Pretul de vanzare
- Marja calculata
- TVA calculat prin "suta marita"
- Numarul facturii de vanzare

### Pasul 5 — Emite factura de vanzare conform regulilor margin scheme
**Reguli critice pentru factura:**
- NU afisa TVA separat pe factura (cumparatorul NU poate deduce TVA)
- Inscrie mentiunea: "Regim special de taxare — bunuri second-hand (Art. 312 Cod Fiscal)"
- Include pretul total (TVA inclus in pret, fara defalcare)
- Aceasta regula se aplica indiferent daca vinzi pe OLX, eBay sau direct

### Pasul 6 — Declara in D300 (Decontul de TVA)
- Inscrie TVA colectat din schema marjei in randul corespunzator din D300
- TVA-ul calculat prin "suta marita" se declara lunar/trimestrial conform periodicitatea firmei
- Schema marjei este OPTIONALA — poti oricand reveni la regimul standard TVA, dar nu poti alterna pe acelasi bun

### Pasul 7 — Aplica actualizarea OUG 22/2025 (efectiva 1 sept. 2025)
**Noutate importanta:** Art. 312 a fost extins pentru a include opere de arta cumparate de un revanzator de la alta persoana impozabila care NU este revanzator. Aceasta largeste aplicabilitatea schemei marjei pentru categoriile de antichitati si arta.

**Implicatie practica:** Daca cumperi de la un artist/producator direct (persoana impozabila, dar nu revanzator), poti aplica acum schema marjei — anterior nu era posibil.

### Pasul 8 — Audit si reconciliere periodica
- Reconciliaza lunar registrul de cumparari cu cel de vanzari
- Verifica ca fiecare bun vandut sub schema marjei are corespondent valid in registrul de cumparari
- Pastreaza documentatia (facturi, registre) minim 10 ani
- La inspectie ANAF, registrele sunt primele documente cerute

## Verificare
- [ ] Fiecare bun vandut sub schema marjei are sursa eligibila documentata
- [ ] Registrul de cumparari si registrul de vanzari sunt completate individual pe bun
- [ ] TVA calculat prin "suta marita" (19/119 sau 21/121), nu prin aplicare directa
- [ ] Facturile de vanzare NU contin TVA separat si au mentiunea Art. 312
- [ ] D300 depus la termen cu TVA-ul din schema marjei
- [ ] OUG 22/2025 verificat pentru categoriile de arta/antichitati

## Instrumente
- ANAF SPV (anaf.ro) — depunere D300, verificare vector fiscal
- Software contabilitate (Saga, Oblio, SmartBill) — registre automate margin scheme
- Art. 312 Cod Fiscal — text integral actualizat
- Directiva 2006/112/CE, art. 311-325 — sursa EU
- OUG 22/2025 — actualizare septembrie 2025

## Note
- Schema marjei este OPTIONALA. Daca nu o aplici, folosesti regimul standard TVA + reverse charge (ARB-002).
- Pentru SRL micro (1% impozit pe cifra de afaceri), cheltuielile nu reduc direct impozitul, dar buyer's premium formeaza baza de cost.
- Buyer's premium (15-25% la casele de licitatii) = 100% cheltuiala deductibila pentru SRL sau PFA.
- Surse consultanta: laboratorfiscal.com, teaha.ro, valentinasaygo.ro (analiza OUG 22/2025).
- Conectat: ARB-002 (reverse charge cand NU aplici margin scheme), ARB-005 (structura entitate).
