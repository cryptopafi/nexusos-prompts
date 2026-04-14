---
id: ARB-009
title: Freight Platform Integration (Cargopedia API)
domain: commercial-law
source: Arbitrage Pro Research — Transport Report 2026-03-21
version: "2.0"
forge_score: "3.65"
business_mapping: ["arbitrage-pro"]
---

# ARB-009: Freight Platform Integration (Cargopedia API)

## Obiectiv
Integreaza Cargopedia REST API in workflow-ul Arbitrage Pro pentru publicare automata de cereri de transport (loads), refresh listing-uri, monitorizare oferte si gasire return loads pe coridorul EU→RO, utilizand endpoint-urile disponibile (POST /api/v1/loads/publish, /delete, /up) si selectand tier-ul de abonament optim (Free → Standard €90/yr → Premium €115/yr) in functie de volumul de transporturi.

## Pasi

### Pasul 1 — Creaza cont si selecteaza tier-ul Cargopedia
**Cargopedia (cargopedia.net) — statistici:**
- Companie romaneasca
- 151,000+ membri inregistrati
- 26,000 activi saptamanal
- Cea mai mare bursa de marfuri din Romania

**Tiers disponibile:**

| Tier | Pret | Functionalitati | Recomandat pentru |
|------|------|-----------------|-------------------|
| Free | €0 | Vizualizare loads, cautare de baza | Testare, <2 transporturi/luna |
| Standard | €90/an | Publicare loads, contact transportatori, API acces | 2-10 transporturi/luna |
| Premium | €115/an | Toate Standard + prioritate, statistici avansate, API full | >10 transporturi/luna |

**Recomandare Arbitrage Pro:**
- Start cu Free (primele 2-3 luni, testeaza platforma manual)
- Upgrade la Standard cand ai 3+ transporturi/luna (API acces necesar)
- Premium daca ajungi la 10+ transporturi/luna

### Pasul 2 — Obtine credentiale API
**Procedura:**
1. Logheaza-te pe cargopedia.net cu contul Standard/Premium
2. Acceseaza setari cont → sectiunea "API / Integrari"
3. Genereaza API key (token de autentificare)
4. Stocheaza API key securizat (env variable, NU in cod)
5. Noteaza base URL: `https://cargopedia.net/api/v1/`

**Autentificare:** Fiecare request necesita header:
```
Authorization: Bearer {API_KEY}
Content-Type: application/json
```

### Pasul 3 — Publica un load (cerere transport)
**Endpoint:** `POST /api/v1/loads/publish`

**Parametri request:**

| Parametru | Tip | Obligatoriu | Descriere |
|-----------|-----|-------------|-----------|
| origin_country | string | DA | Tara origine (NL, DE, BE) |
| origin_city | string | DA | Oras origine |
| destination_country | string | DA | Tara destinatie (RO) |
| destination_city | string | DA | Oras destinatie |
| palletized | boolean | DA | true/false |
| pallets | integer | Daca palletized=true | Nr. paleti |
| pallet_type | string | Daca palletized=true | "EUR" (120×80) |
| weight_kg | integer | DA | Greutate totala kg |
| groupage | string | DA | "FTL" sau "LTL" |
| truck_type | string | NU | "tilt", "box", "flatbed", "reefer" |
| pickup_date | date | DA | Data ridicare dorita |
| notes | string | NU | Detalii suplimentare |

**Exemplu request publicare load:**
```json
{
  "origin_country": "NL",
  "origin_city": "Rotterdam",
  "destination_country": "RO",
  "destination_city": "Bucharest",
  "palletized": true,
  "pallets": 3,
  "pallet_type": "EUR",
  "weight_kg": 500,
  "groupage": "LTL",
  "truck_type": "tilt",
  "pickup_date": "2026-04-01",
  "notes": "Second-hand equipment from auction. Declare value: €2,500."
}
```

**Response:** Load ID + status "published" + URL listing

### Pasul 4 — Refresh (up) un listing existent
**Endpoint:** `POST /api/v1/loads/up`

**Scop:** Readuce listing-ul in top (ca pe OLX "promoveaza") — listing-urile vechi coboara in lista.

**Parametri:**
```json
{
  "load_id": "12345678"
}
```

**Strategie refresh:**
- Refresh la fiecare 12-24h pentru vizibilitate maxima
- Automatizeaza cu cron job (ex: zilnic la 8:00 AM)
- NU refresh mai des de 6h — poate fi considerat spam

### Pasul 5 — Sterge un load completat
**Endpoint:** `POST /api/v1/loads/delete`

**Parametri:**
```json
{
  "load_id": "12345678"
}
```

**Cand stergi:**
- Dupa ce ai gasit transportator si confirmat booking
- Daca anulezi transportul
- Daca load-ul a expirat (data pickup trecuta)

### Pasul 6 — Integreaza in workflow-ul Arbitrage Pro
**Workflow automatizat propus:**

```
1. Castigi licitatie EU
   ↓
2. Script calculeaza: nr. paleti, greutate, origine, destinatie
   ↓
3. API call → POST /api/v1/loads/publish (Cargopedia)
   + Paralel: email Paneuropa + Eurosender (pentru comparatie)
   ↓
4. Cron job → POST /api/v1/loads/up (refresh zilnic)
   ↓
5. Primesti oferte de la transportatori pe Cargopedia
   ↓
6. Compara cu ofertele Paneuropa/Eurosender
   ↓
7. Accepta cea mai buna oferta
   ↓
8. API call → POST /api/v1/loads/delete (cleanup)
```

**Integrare cu Arbitrage Pro pipeline:**
- Dupa phase "lot won" → trigger automat publicare load
- Date preluate din lot metadata: locatie licitatie, nr. bunuri, greutate estimata
- Pret target: €120/pallet (benchmark Paneuropa)

### Pasul 7 — Cauta return loads pentru reducere cost
**Return loads = camioane care se intorc goale din EU in RO**
- Cost redus 20-40% fata de tariful normal
- Disponibilitate fluctuanta (depinde de sezon)
- Cel mai frecvent pe rutele NL→RO, DE→RO

**Cautare return loads pe Cargopedia:**
1. Filtreaza loads cu origine RO, destinatie NL/DE/BE
2. Contacteaza transportatorii acestor loads
3. Intreaba: "Aveti loc pe returul gol NL/DE→RO?"
4. Negociaza pret redus (50-70% din tariful normal)

**Alternative platforme return loads:**
- Trans.eu — puternic pe coridorul DE/PL/RO
- Eurosender — agregator, include return loads automat

### Pasul 8 — Monitorizare si optimizare
**Metrici de urmarit:**
- Cost mediu per pallet per ruta (target: ≤€120 NL/BE→RO, ≤€100 DE→RO)
- Timp mediu gasire transportator (target: <48h)
- Rata oferte primite per load publicat
- Rata return loads vs tarif normal

**Error handling API:**
- Rate limiting: respecta limitele Cargopedia (undocumented — start conservativ, 1 req/10s)
- Exponential backoff pe 429/503: 1s→2s→4s→8s, max 4 retries
- Log toate API calls pentru audit si optimizare
- Fallback: publicare manuala pe site daca API down

**Automatizare avansata (faza viitoare):**
- Webhook/polling pentru notificare oferte noi (daca API suporta)
- Auto-accept oferte sub threshold (ex: <€130/pallet)
- Dashboard cu statistici transport per luna

## Verificare
- [ ] Cont Cargopedia creat si tier selectat (Standard minim pentru API)
- [ ] API key generat si stocat securizat (env variable)
- [ ] Test publicare load reusit (POST /api/v1/loads/publish)
- [ ] Test refresh load reusit (POST /api/v1/loads/up)
- [ ] Test stergere load reusit (POST /api/v1/loads/delete)
- [ ] Workflow integrat in pipeline Arbitrage Pro
- [ ] Exponential backoff implementat pe toate API calls
- [ ] Return loads cautate activ pe fiecare transport

## Instrumente
- Cargopedia API (cargopedia.net/api/v1/) — REST endpoints
- Trans.eu (trans.eu) — alternativa freight exchange
- Paneuropa Logistics (paneuropa-logistics.ro) — benchmark preturi
- Eurosender (eurosender.com) — cotatie rapida comparativa
- Pamyra (pamyra.de) — instant freight DE→RO

## Note
- **Cargopedia API = REST, nu GraphQL.** Toate call-urile sunt POST (nu GET pentru publicare).
- Abonamentul €90-115/an este extrem de ieftin raportat la economiile posibile (un singur return load economiseste €50-100).
- Trans.eu are si API pentru TMS integration — evaluare viitoare daca Cargopedia nu acopera suficient coridorul DE→RO.
- Clicktrans (clicktrans.eu) = marketplace vehicule, nu groupage. Pentru vehicule → ARB-008.
- Pamyra (pamyra.de) = comparison tool german, instant booking — bun pentru DE→RO one-off.
- Conectat: ARB-007 (procedura manuala groupage — aceasta procedura o automatizeaza), ARB-008 (vehicule — Clicktrans nu Cargopedia).
