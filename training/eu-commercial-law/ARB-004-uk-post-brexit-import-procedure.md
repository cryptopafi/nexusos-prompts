---
id: ARB-004
title: UK Post-Brexit Import Procedure
domain: eu-commercial-law
source: Arbitrage Pro Research — Legal Compliance Guide 2026-03-21
version: "2.0"
forge_score: "3.65"
business_mapping: ["arbitrage-pro"]
---

# ARB-004: UK Post-Brexit Import Procedure

## Obiectiv
Importa bunuri second-hand din licitatii UK (post-Brexit) in Romania cu calcularea completa a costurilor (duty, import VAT, customs, transport, currency risk), recuperarea TVA-ului de import ca TVA deductibil (pentru SRL-uri inregistrate TVA), si evaluarea viabilitatii financiare a tranzactiei comparativ cu surse EU echivalente, utilizand TARIC pentru clasificarea vamala si hedging GBP/RON pentru protectia valutara.

## Pasi

### Pasul 1 — Calculeaza costul total de import (full cost stack)
**Tabel complet costuri UK→Romania:**

| Element cost | Detalii | Estimare |
|-------------|---------|----------|
| Hammer price | Pretul la ciocan, negociat | Variabil |
| Buyer's premium | 15-25% din hammer price | +15-25% |
| VAT UK pe premium | 20% pe buyer's premium | +20% din premium |
| Declaratia de export UK | Obligatorie pentru toate livrari comerciale | Inclus de vanzator |
| Freight/transport | UK→RO, variabil | €200-800 |
| Taxa vamala Romania | 0-6.5% pentru cele mai multe bunuri SH | 0-6.5% din CIF |
| TVA import Romania | 19% (sau 21% din 2026) pe (CIF + taxa vamala) | 19-21% |
| Taxa fixa colete mici | €4.90 pentru colete <€150 (din nov. 2025) | €4.90 |

**Formula cost total:**
```
Cost total = Hammer + Premium + (Premium × 20% UK VAT) + Transport +
             (CIF + Duty) × 19% TVA import + Duty
unde CIF = Hammer + Premium + Transport + Asigurare
unde Duty = CIF × rata vamala TARIC (0-6.5%)
```

### Pasul 2 — Verifica clasificarea vamala in TARIC
**TARIC = Tariful integrat al Uniunii Europene**
- Acceseaza: ec.europa.eu/taxation_customs/dds2/taric/
- Cauta codul NC (Nomenclatura Combinata) al bunului
- Verifica rata aplicabila pentru import din UK (tara terta post-Brexit)

**Rate vamale orientative bunuri SH comune:**

| Categorie | Cod NC orientativ | Rata vamala |
|-----------|-------------------|-------------|
| Utilaje industriale | 8458-8466 | 0-2.7% |
| Generatoare electrice | 8502 | 0-2.7% |
| Echipamente sudura | 8515 | 0% |
| Electronice consumer | 8471, 8528 | 0% (ITA) |
| Mobilier | 9403 | 0-2.7% |
| Vehicule (piese) | 8708 | 3-4.5% |

**Nota:** UK are acord TCA (Trade and Cooperation Agreement) cu EU — multe bunuri au taxa vamala 0% daca se demonstreaza originea UK (Rules of Origin).

### Pasul 3 — Gestioneaza declaratia vamala la import
**Documente necesare:**
- Factura comerciala de la casa de licitatii UK
- Packing list
- Conosament / CMR (transport)
- Declaratia de export UK (referinta MRN)
- Certificat de origine (daca se solicita preferinta tarifara TCA)
- Declaratia vamala de import DAU (Documentul Administrativ Unic)

**Optiuni procesare vamala:**
- Broker vamal (comisionar) — recomandat, cost €50-150 per declaratie
- Direct prin e-Customs Romania (customs.ro) — necesita acreditare

### Pasul 4 — Plateste si recupereaza TVA-ul de import
**Daca esti SRL inregistrat TVA:**
- TVA import = TVA deductibil → recuperabil prin D300
- Documente necesare pentru deducere: DAU/MRN + certificat de import + factura
- Efect: TVA-ul de import NU este un cost real, ci un avans recuperabil

**Daca NU esti inregistrat TVA (PFA fara TVA):**
- TVA import = COST REAL, nerecuperabil
- Adauga 19-21% la costul total — reduce semnificativ profitabilitatea
- **Recomandare:** Inregistrare TVA voluntara daca importi regulat din UK

### Pasul 5 — Evalueaza viabilitatea financiara vs surse EU
**Regula de viabilitate:** Importul UK este justificat DOAR daca pretul UK este 15-20% sub echivalentul EU, pentru a acoperi:
- Taxa vamala (0-6.5%)
- Costuri suplimentare declaratie vamala (~€100)
- Timp suplimentar procesare (3-7 zile)
- Risc valutar GBP/RON

**Exemplu comparativ:**

| Element | Sursa EU (Troostwijk NL) | Sursa UK |
|---------|--------------------------|----------|
| Pret achizitie | €1,000 | £850 (~€1,000) |
| Buyer's premium | €210 | £178 (~€209) + £36 UK VAT |
| Transport 1 pallet | €120 | €250 (UK→RO) |
| Taxa vamala | €0 (intra-EU) | €31 (2.5%) |
| TVA import | €0 (reverse charge) | €279 (recuperabil) |
| Broker vamal | €0 | €100 |
| **Cost total** | **€1,330** | **€1,626** |
| TVA recuperat | N/A | -€279 |
| **Cost net** | **€1,330** | **€1,347** |

In acest exemplu, UK nu ofera avantaj. UK devine viabil cand pretul ciocanului este cu 15-20% mai mic.

### Pasul 6 — Gestioneaza expunerea valutara GBP/RON
**Strategii de hedging/protectie:**
1. **Buffer de pret** — adauga 5% la cursul curent GBP/RON in calculul de cost
2. **Plata rapida** — converteste GBP imediat dupa achizitie (nu amana)
3. **Cont multi-currency** — Wise/Revolut Business cu sold GBP (evita spread bancar)
4. **Forward contract** — pentru achizitii >€5,000, negociaza curs fix cu banca

**Curs referinta:** BNR zilnic (bnr.ro), Wise mid-market rate

### Pasul 7 — Documenteaza si arhiveaza
- Pastreaza toate documentele de import minim 10 ani (cerinta EU)
- Creaza dosar per import: factura UK + DAU + MRN + CMR + D300 cu TVA recuperat
- Fotografiaza bunurile la receptie (pentru eventuale reclamatii transport)

## Verificare
- [ ] Cod NC/TARIC verificat pentru fiecare categorie de bunuri
- [ ] Cost stack complet calculat inainte de licitare (inclusiv duty + transport + broker)
- [ ] Diferenta pret UK vs EU ≥ 15-20% pentru a justifica importul
- [ ] DAU completat si TVA import platit
- [ ] TVA import declarat ca deductibil in D300 (daca SRL TVA)
- [ ] Expunere GBP/RON gestionata (buffer sau hedging)

## Instrumente
- TARIC (ec.europa.eu/taxation_customs/dds2/taric/) — clasificare vamala
- e-Customs Romania (customs.ro) — declaratii vamale
- ANAF SPV — D300 cu TVA import deductibil
- Wise Business / Revolut Business — conturi multi-currency GBP
- BNR (bnr.ro) — curs valutar referinta

## Note
- Post-Brexit, UK = tara terta. Toate importurile necesita declaratie vamala (chiar si sub €150 — taxa fixa €4.90).
- Acordul TCA UK-EU ofera taxa vamala 0% pentru multe categorii DACA se demonstreaza originea UK — solicita certificat de origine de la vanzator.
- UK VAT (20%) platit pe buyer's premium NU este recuperabil in Romania — este cost real.
- Pentru bunuri fragile sau de mare valoare, asigurarea de transport este obligatorie (DHL declared value 1-2% din valoare).
- Conectat: ARB-001 (margin scheme la revanzare), ARB-002 (reverse charge — NU se aplica la UK), ARB-005 (structura entitate), ARB-007 (transport).
