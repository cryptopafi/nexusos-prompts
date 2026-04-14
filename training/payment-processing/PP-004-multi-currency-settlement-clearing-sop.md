---
id: PP-004
title: Multi-Currency Settlement and Clearing SOP
domain: payment-processing
source: Payment Gateway Models and Business Strategies — Fintech (Udemy)
version: 2.0
forge_score: "3.75"
business_mapping: []
---

# PP-004: Multi-Currency Settlement and Clearing SOP

## Obiectiv
Gestiona eficient colectarea, conversia si settlement-ul platilor in multiple valute pentru a minimiza costurile de FX (foreign exchange) — spread-uri tipice 1.5-3% la banci vs. 0.4-1% la EMI-uri — a reduce riscul valutar prin hedging adecvat, si a asigura cash flow predictibil pentru business-uri cu clienti internationali, cu reconciliere completa si compliance fiscal (Romania/EU).

## Pasi

### Pasul 1 — Intelegerea completa a fluxului de fonduri si a costurilor ascunse
**Ciclul complet al unei plati multi-currency:**
```
Cardholder plateste (ex: USD) →
  Issuing Bank (banca clientului) →
  Card Network (Visa/MC — scheme fee 0.1-0.3%) →
  Acquiring Bank (banca procesatorului) →
  Payment Processor (MDR = interchange + scheme + acquirer markup) →
  Settlement Account (in transaction currency sau settlement currency) →
  FX Conversion (daca settlement ≠ transaction currency) →
  Merchant Bank Account (fonduri disponibile)
```

**Termeni cheie si unde se pierd bani:**
| Termen | Ce este | Cost | Unde se pierde |
|--------|---------|------|---------------|
| Transaction currency | Moneda in care plateste clientul | 0% | - |
| Settlement currency | Moneda in care merchantul primeste | 0% daca = transaction | FX daca diferita |
| Presentment currency | Moneda in care tranzactia e prezentata la card network | 0% | - |
| Interchange fee | Taxa platita bancii emitente | 0.2-2%+ | Fix, nenegociabil |
| Scheme fee | Taxa platita Visa/MC | 0.1-0.3% | Fix |
| Acquirer markup | Markup-ul bancii acquirer | 0.3-1.5% | Negociabil (in MDR) |
| **FX conversion fee** | **Taxa de conversie valutara** | **0.5-3.5%** | **COSTUL ASCUNS PRINCIPAL** |
| Cross-border fee | Fee suplimentar daca card si merchant in tari diferite | 0.5-1.5% | Se adauga la MDR |

**Momentele de conversie FX si costurile lor:**
| Moment | Cine converteste | Spread tipic | Control merchant |
|--------|-----------------|-------------|-----------------|
| DCC (Dynamic Currency Conversion) | Procesator la momentul tranzactiei | 3-5% | DA (dar evitati) |
| Settlement conversion | Acquirer la clearing (T+1/T+2) | 1.5-3% | Partial (negociabil) |
| Manual conversion post-settlement | Merchant (Wise/Revolut/banca) | 0.4-1% | DA (recomandat) |

**Regula**: primiti fondurile in moneda ORIGINALA a tranzactiei (fara conversie automata) si convertiti VOI cand rata este favorabila.

### Pasul 2 — Structura optima de conturi multi-currency
**Principiu**: colectati in mai multe valute FARA a converti imediat — convertiti in batch cand rata este favorabila.

**Optiunea 1 — Procesator cu multi-currency settlement (RECOMANDAT pentru high-risk):**
- Procesatori: Nuvei, Adyen, Checkout.com, Worldpay — ofera conturi multi-currency.
- Fondurile in USD raman in USD, GBP in GBP, EUR in EUR — separate.
- Convertiti la cerere sau la scheduling definit.
- Avantaj: un singur dashboard, settlement integrat.
- Dezavantaj: FX markup procesator (1-2.5%) este mai mare decat EMI.

**Optiunea 2 — EMI (Electronic Money Institution) multi-currency ca FX hub:**
| EMI | Nr. valute | FX spread | Cost cont | Best for |
|-----|-----------|----------|----------|---------|
| Wise Business | 50+ | 0.4-0.7% | Gratuit | SMB, volum < 500K/luna |
| Airwallex | 30+ | 0.3-0.6% | Gratuit | E-commerce, APAC focus |
| Revolut Business | 30+ | 0.4-1% | De la 0 EUR/luna | Quick setup, EU |
| Payoneer | 10+ | 1-2% | Gratuit | Marketplace payouts |

**Structura recomandata (cost-optima):**
```
Procesator (Nuvei/Worldpay) → Settlement multi-currency (EUR, USD, GBP separate)
                                      ↓
                              Wise Business / Airwallex (FX hub — convertiti aici)
                                      ↓
                              Cont EUR principal (cheltuieli operationale Romania)
                                      ↓ (daca necesar)
                              Cont RON (plati ANAF, furnizori locali)
```

**Optiunea 3 — Banca traditionala cu conturi nostro multi-currency:**
- Banci: ING, Raiffeisen, BCR — ofera conturi multi-currency dar spread FX 2-4%.
- Avantaj: stabilitate bancara (important pentru volume > 1M EUR/luna).
- Dezavantaj: costul FX este 3-5x mai mare decat Wise/Airwallex.
- Folositi DOAR pentru depozite/rezerve de siguranta, NU pentru conversii zilnice.

### Pasul 3 — Optimizarea costurilor FX (strategie practica)
**Costul real al conversiei — comparatie la 100K USD/luna convertit in EUR:**
| Metoda | FX markup | Cost lunar | Cost anual |
|--------|----------|-----------|-----------|
| Banca traditionala | 2-4% | 2,000-4,000 EUR | 24K-48K EUR |
| Procesator (auto-conversion) | 1.5-2.5% | 1,500-2,500 EUR | 18K-30K EUR |
| **Wise Business** | **0.4-0.7%** | **400-700 EUR** | **4.8K-8.4K EUR** |
| **Airwallex** | **0.3-0.6%** | **300-600 EUR** | **3.6K-7.2K EUR** |

**Economie anuala Wise vs. banca**: 15K-40K EUR la 100K USD/luna. Diferenta CRESTE liniar cu volumul.

**5 strategii de optimizare FX:**

1. **Natural hedging** (GRATUIT, cel mai eficient):
   - Daca aveti cheltuieli si in USD (servere, publicitate Meta/Google care factureaza USD, furnizori internationali): platiti DIRECT din USD colectat.
   - Nu convertiti si apoi reconvertiti — fiecare conversie costa.

2. **Batch conversion** (simplu, ieftin):
   - Nu convertiti zilnic (rate mai proaste per conversie, fees per tranzactie).
   - Convertiti 1-2 ori/saptamana sau o data pe luna.
   - Wise si Airwallex au batch conversion features.

3. **Rate alerts** (Wise, Revolut):
   - Setati alerta la rata dorita (ex: "Notifica-ma cand EUR/USD > 1.10").
   - Convertiti manual cand rata atinge target-ul.
   - Eficient daca nu aveti urgenta cash flow.

4. **Forward contracts** (pentru volume mari > 500K EUR/an):
   - Fixati un curs de schimb pentru 3-12 luni in avans.
   - Furnizori: Ebury, WorldFirst, IG, sau banca proprie (pentru volume mari).
   - Eliminati complet riscul de volatilitate FX.
   - Cost: 0.1-0.5% premium fata de spot rate — ieftin vs. riscul de miscare 5-10% pe an.

5. **Evitati DCC (Dynamic Currency Conversion)**:
   - DCC = procesatorul ofera clientului sa vada pretul in moneda proprie la momentul platii.
   - Spreadul DCC: 3-5% — este sursa de revenue pentru procesator, nu pentru voi.
   - Daca procesatorul ofera DCC: refuzati-l sau asigurati-va ca nu e activat default.

### Pasul 4 — Settlement si clearing — timpi si reconciliere
**Settlement cycle standard per tip procesator:**
| Tip | Authorization | Clearing | Settlement | Total |
|-----|-------------|---------|-----------|-------|
| Low-risk (Stripe) | Real-time | T+0 | T+2 | 2 zile |
| Medium-risk | Real-time | T+1 | T+3-T+5 | 3-5 zile |
| High-risk | Real-time | T+1 | T+5-T+7 | 5-7 zile |
| High-risk cu rolling reserve | Real-time | T+1 | T+7 (minus RR) | 7 zile + RR retinut 180 zile |

**Reconciliere lunara multi-currency (obligatorie):**

**Raport de reconciliere lunar (template Google Sheets):**
| Valuta | Volume brut | Refund-uri | CB-uri | Net volume | FX rate folosit | Net EUR | Net RON (curs BNR) |
|--------|-----------|-----------|--------|-----------|----------------|---------|-------------------|
| USD | | | | | | | |
| GBP | | | | | | | |
| EUR | | | | | | | |
| **Total** | | | | | | **X EUR** | **Y RON** |

**Pasi de reconciliere (executati lunar, 1-2 ore):**
1. Exportati raportul de tranzactii din dashboardul procesatorului (CSV/Excel — pe luna calendaristica)
2. Exportati extrasele de cont bancar / Wise / Airwallex (pe aceeasi luna)
3. Reconciliati: tranzactii procesate = fonduri primite + CB-uri + refund-uri + fees + rolling reserve retinut
4. Identificati discrepante: tranzactii procesate dar nesettled, CB-uri deductate, fees nerecunoscute
5. Reconciliati cu facturarea interna (facturile emise vs. platile primite)
6. **Documentati discrepantele** — oricare > 100 EUR nerezolvata trebuie escalata la procesator in 5 zile

### Pasul 5 — Gestionarea riscului valutar (FX Risk Management)
**Tipuri de risc FX:**
| Tip risc | Descriere | Exemplu | Hedging |
|----------|----------|---------|---------|
| Transaction risk | Rata se schimba intre data tranzactiei si settlement | Vindeti la $100 azi, primiti $97 worth peste 5 zile | Forward contract |
| Translation risk | Active/datorii in valuta fluctueaza la raportare | Cont USD de $500K fluctueaza zilnic in EUR | Conversie regulata |
| Economic risk | Competitivitate preturi afectata pe termen lung | EUR se intareste 10%, produsul devine scump pt USD clients | Pricing strategy |

**Praguri de hedging (ghid practic):**
| Expunere FX lunara | Strategie recomandata | Cost |
|-------------------|----------------------|------|
| Sub 50K EUR | NU hedging formal — Wise/Revolut, batch conversion | 0.4-1% spread |
| 50K-500K EUR | Forward contracts pe 3-6 luni pentru valutele principale | 0.1-0.5% premium |
| Peste 500K EUR | Broker FX dedicat (Ebury, WorldFirst) + treasury management | Fee lunar + 0.1-0.3% |

**Politica interna FX (document de 1 pagina — de creat si respectat):**
```
POLITICA FX [COMPANIE]

1. Moneda de raportare: EUR
2. Valute colectate: USD, GBP, EUR
3. Frecventa conversie: saptamanala (vineri) sau la atingerea threshold de [X] EUR echivalent
4. Metoda conversie: Wise Business (sub 500K EUR/luna) / Forward contract (peste)
5. Rata de referinta pentru facturare: curs BNR din data facturarii
6. Limita expunere nehedged: max [X] EUR per valuta
7. Responsabil FX: [Nume/Functie]
8. Review politica: semestrial
```

### Pasul 6 — Compliance fiscal multi-currency (Romania + EU)
**Cerinte ANAF Romania:**
- Toate tranzactiile in valuta trebuie convertite in RON la **cursul BNR din data tranzactiei** pentru raportare fiscala.
- Cursul BNR se publica zilnic la `bnr.ro/Cursul-de-schimb-al-pietei-valutare-702.aspx` — automat exportabil.
- Veniturile si cheltuielile in valuta se inregistreaza la curs BNR, iar diferentele de curs (favorabile sau nefavorabile) se inregistreaza separat ca venituri/cheltuieli financiare.

**TVA pe tranzactii cross-border (B2C EU):**
| Situatie | Regim TVA | Actiune |
|---------|----------|--------|
| B2C in Romania | TVA standard 19% | Normal |
| B2C in alte state UE, volum < 10K EUR/an total EU | TVA Romania (19%) | Normal |
| **B2C in alte state UE, volum > 10K EUR/an total EU** | **OSS (One Stop Shop)** | **Inregistrare OSS + declaratie trimestriala + plata TVA per stat** |
| B2C in afara UE | TVA 0% (export) | Documentatie export obligatorie |
| B2B in EU (cu VAT ID valid) | Reverse charge (0% TVA) | Verificati VIES, facturati fara TVA |

**OSS (One Stop Shop) — daca aplicabil:**
- Inregistrare la ANAF pentru regimul OSS
- Declaratie trimestriala online cu defalcarea vanzarilor pe fiecare stat UE
- Platiti TVA-ul fiecarui stat EU din Romania (nu trebuie inregistrare separata in fiecare stat)
- Cota TVA variaza per stat: 17% (Luxembourg) - 27% (Ungaria)

**Cerinte de raportare catre procesator:**
- Unii procesatori cer "settlement reconciliation report" lunar
- Certificat de rezidenta fiscala (Form 6166 echivalent romanesc) — cerut de procesatori non-EU pentru reducerea withholding tax

### Pasul 7 — Optimizari avansate si automatizari
**Automatizare reconciliere (reduceți de la 2 ore la 15 minute):**
- Exportati automat rapoartele procesator via API → Google Sheets
- Comparati automat cu extrasele Wise/Airwallex via API
- Scrieti formule de reconciliere care evidentiaza discrepantele > [threshold]
- Raport auto-generat lunar (Google Sheets + conditional formatting)

**Multi-currency pricing (optional, avansat):**
- In loc sa vindeti totul in EUR si sa lasati cardholder-ul sa plateasca in moneda proprie (cu FX la banca lor):
- Afisati pretul in moneda locala a clientului (USD pentru US, GBP pentru UK) si procesati in acea moneda.
- Avantaj: clientul vede pret clar, fara surprize FX pe extrasul lor → reduce CB-urile de tip "amount different than expected".
- Dezavantaj: trebuie sa mentineti pricing in multiple valute si sa gestionati FX risk.

**Cascade routing multi-currency:**
- Daca aveti 2 procesatori (primary + backup — recomandat in PP-001):
- Rutati tranzactii USD prin procesatorul cu cel mai bun FX rate pe USD
- Rutati tranzactii EUR direct (fara conversie)
- Reduceti costul FX global cu 0.3-0.5% prin routing inteligent

## Verificare
- [ ] Conturi separate per valuta configurate (procesator + EMI)
- [ ] Costurile FX monitorizate si optimizate (comparate cu Wise/Airwallex ca benchmark)
- [ ] Reconciliere lunara multi-currency executata si documentata
- [ ] Politica interna FX documentata (frecventa conversie, limite expunere)
- [ ] Compliance TVA OSS verificat daca volume B2C EU > 10K EUR/an
- [ ] Raportare ANAF pentru tranzactii valutare conforma (curs BNR)
- [ ] Natural hedging implementat unde posibil (cheltuieli in aceeasi valuta ca veniturile)
- [ ] Forward contracts evaluate (daca expunere > 50K EUR/luna)
- [ ] Reconciliere fara discrepante nerezolvate > 100 EUR

## Instrumente
- Wise Business sau Airwallex (FX hub cu rate competitive)
- Xe.com sau OANDA (rate FX de referinta si comparatie)
- Google Sheets sau Xero/QuickBooks (reconciliere multi-currency)
- BNR.ro (cursuri oficiale pentru raportare fiscala Romania)
- ANAF portal (raportare TVA OSS)
- Ebury / WorldFirst (forward contracts pentru volume mari)

## Note
- **Cel mai frecvent cost ascuns in plati internationale este FX markup-ul** — calculati-l EXPLICIT (cereti rata la procesator si comparati cu Wise/xe.com) si mitigati cu EMI.
- La 100K USD/luna, diferenta intre banca traditionala (3%) si Wise (0.5%) = **30K EUR/an economie**. Nu e trivial.
- Settlement mai rar (saptamanal vs zilnic) reduce numarul de conversii si costurile FX cumulate — dar afecteaza cash flow. Gasiti echilibrul.
- **Rolling reserve** (vezi PP-001, PP-005) este in settlement currency — daca settleti in USD dar reporting in EUR, reserve-ul fluctueaza in RON/EUR. Calcullati-l in moneda de referinta si actualizati lunar.
- **Cursa pentru "cele mai mici rate FX" are diminishing returns sub 0.3% markup** — prioritizati stabilitatea procesatorului si reliability-ul settlement-ului fata de 0.1% diferenta.

## Cortex Logging
```json
{
  "type": "procedure_execution",
  "collection": "payment-processing",
  "tags": ["multi-currency", "FX", "settlement", "clearing", "reconciliation", "PP-004"],
  "entry": {
    "procedure_id": "PP-004",
    "timestamp": "{{ISO_8601}}",
    "executor": "{{agent_id}}",
    "currencies_active": ["{{EUR}}", "{{USD}}", "{{GBP}}"],
    "fx_provider": "{{Wise|Airwallex|Revolut|bank}}",
    "fx_spread_avg": "{{spread_%}}",
    "monthly_fx_cost_eur": "{{EUR}}",
    "reconciliation_status": "matched | discrepancies_found | pending",
    "discrepancies_eur": "{{EUR_total_unresolved}}",
    "hedging_method": "natural | batch | forward | none",
    "outcome": "optimized | needs_review | critical",
    "notes": "{{free_text}}"
  }
}
```

## Enforcement Loop

### WHERE
- Activat pe orice business care colecteaza plati in mai mult de o valuta.
- Se aplica si la business-uri cu o singura valuta de colectare dar settlement in alta moneda.
- VK: `PP-004-WHERE-ACTIVE`

### WHEN
- La onboarding-ul initial: configurare structura de conturi multi-currency (Pasul 2).
- Lunar: reconciliere obligatorie (Pasul 4) — nu se sare peste niciodata.
- Saptamanal (sau la batch conversion): executie conversie FX (Pasul 3).
- Semestrial: review politica FX interna + evaluare forward contracts (Pasul 5).
- La orice schimbare de procesator sau valuta noua adaugata.
- VK: `PP-004-WHEN-TRIGGERED`

### HOW
- Structura de conturi se defineste INAINTE de prima tranzactie (Pasul 2).
- Reconcilierea se executa cu template-ul din Pasul 4 — nu ad-hoc.
- Discrepante > 100 EUR se escaleaza la procesator in 5 zile lucratoare.
- DCC (Dynamic Currency Conversion) se dezactiveaza MEREU (Pasul 3, strategie 5).
- Conversia FX se face prin EMI (Wise/Airwallex), NU prin banca traditionala.
- VK: `PP-004-HOW-EXECUTED`

### CONNECT
- PP-001 (Gateway Selection) — settlement currency se negociaza in contractul de gateway.
- PP-002 (Chargeback Management) — CB-urile se deduc din settlement; reconcilierea trebuie sa le includa.
- PP-005 (High-Risk Onboarding) — rolling reserve este in settlement currency; impactul FX pe RR se calculeaza.
- VK: `PP-004-CONNECT-LINKED`

### VERIFY
- [ ] Conturi separate per valuta configurate (procesator + EMI) (Pasul 2).
- [ ] FX markup monitorizat si sub 1% (comparatie cu Wise/xe.com benchmark).
- [ ] Reconciliere lunara executata cu 0 discrepante nerezolvate > 100 EUR (Pasul 4).
- [ ] Politica interna FX documentata si respectata (Pasul 5).
- [ ] Compliance TVA OSS verificat daca B2C EU > 10K EUR/an (Pasul 6).
- [ ] Natural hedging implementat unde posibil.
- VK: `PP-004-VERIFY-COMPLETE`

## Model Routing
| Faza | Model | Justificare |
|------|-------|-------------|
| Configurare structura conturi | **Sonnet** | Decizie structurala cu optiuni clare din Pasul 2 |
| Reconciliere lunara | **Haiku** | Matching numeric, verificare discrepante — task repetitiv |
| Optimizare FX strategy | **Sonnet** | Comparatie rate, evaluare forward contracts |
| Compliance fiscal multi-currency (OSS/ANAF) | **Opus** | Complexitate juridico-fiscala cross-border |
| Automatizare reconciliere API | **Sonnet** | Implementare tehnica standard |

## Metrics
| Metrica | Formula | Target | Red Flag | Frecventa |
|---------|---------|--------|----------|-----------|
| FX cost ratio | Cost FX lunar / Volum convertit x 100 | < 0.7% | > 1.5% | Lunar |
| Economie FX vs. banca | (Cost banca - Cost EMI) / Cost banca x 100 | > 60% economie | < 30% | Lunar |
| Reconciliere accuracy | Discrepante nerezolvate / Total tranzactii x 100 | 0% | > 0.1% | Lunar |
| Settlement timeliness | % settlements primite la timp | 100% | < 95% | Lunar |
| Expunere FX nehedged | Total valuta neconvertita / Limite politica x 100 | < 100% din limita | > 100% din limita | Saptamanal |
| Compliance TVA OSS | Declaratii depuse la timp DA/NU | DA | NU | Trimestrial |
