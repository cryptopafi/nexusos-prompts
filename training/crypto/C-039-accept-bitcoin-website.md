---
type: procedure
created: 2026-03-22
status: active
slug: c-039-accept-bitcoin-website
tags: [procedure, nexus]
---

# Accept Bitcoin on Your Business Website — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru integrarea acceptării de Bitcoin (on-chain și Lightning) ca metodă de plată pe un website de business, folosind soluții non-custodial și procesoare de plăți.

---

## 1. Problema

Acceptarea Bitcoin pe un website de business permite accesul la o bază globală de clienți care preferă plăți crypto, elimină chargeback-urile, și reduce comisioanele față de cardurile de credit. Fără o integrare corectă, plățile pot fi pierdute, neconfirmate, sau procesate incorect din cauza variației prețului BTC.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Business cu website care vrea să accepte Bitcoin ca metodă de plată suplimentară
- Freelancer sau creator digital care vrea să factureze în Bitcoin
- Developer care integrează Bitcoin payments într-un e-commerce existent

---

## 2. Procedura

### Pas 1: Alege modelul de acceptare Bitcoin
Trei opțiuni: (1) Self-custody non-custodial — BTCPay Server (controlezi complet, zero fee, necesită server); (2) Procesor custodial simplu — OpenNode, CoinGate (ușor de integrat, fee 1%); (3) Lightning Address — primești direct în wallet (ideal pentru sume mici, creatori). Documentează alegerea în funcție de volumul estimat și resurse tehnice.

### Pas 2: Configurează BTCPay Server (recomandat pentru self-custody)
Opțiunea self-custody: deployează BTCPay Server pe VPS (DigitalOcean $6/lună, 1-click deployment) sau folosește un hosting provider BTCPay. Conectează-ți propriul wallet Bitcoin (xPub pentru on-chain, Lightning node pentru LN). BTCPay generează automat adrese noi pentru fiecare comandă — niciodată nu refolosești o adresă.

### Pas 3: Integrează cu platforma website-ului
BTCPay are plugins/integrations pentru: WooCommerce, Shopify, Wix, WordPress, și API pentru custom. Instalează plugin-ul corespunzător platformei tale. Configurează webhook-urile pentru confirmarea plăților. Testează în modul test (testnet) înainte de go-live.

### Pas 4: Configurează conversia automată (opțional, pentru evitarea volatilității)
Dacă nu vrei exposure la volatilitatea BTC, configurează conversia automată în fiat (EUR/USD) la primirea plății. OpenNode și CoinGate oferă această opțiune. BTCPay Server poate fi configurat cu un exchange API (Kraken, Bitstamp) pentru conversie automată.

### Pas 5: Adaugă Bitcoin ca opțiune de plată pe checkout
Asigură-te că checkout-ul afișează clar: QR code pentru adresa BTC, suma în BTC calculată la rata curentă, timer (de obicei 15 minute pentru confirmare la rata cotată), și instrucțiuni simple. Testează flow-ul complet de la client: selectare produs → checkout → plată BTC → confirmare comandă.

### Pas 6: Testează end-to-end cu o tranzacție reală
Execută o achiziție test cu o sumă mică (ex. $1-5). Verifică că: invoice-ul este generat corect, plata este detectată, confirmarea comenzii este trimisă automat, și fondurile au ajuns în wallet. Documentează orice erori și rezolvă-le înainte de go-live public.

### Pas 7: Comunică opțiunea de plată și documentează în Cortex
Adaugă badge-ul "Bitcoin Accepted" pe site. Anunță pe social media că accepți Bitcoin. Documentează: procesorul ales, data integrării, volumul primelor plăți. Monitorizează lunar utilizarea acestei metode de plată. Salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Integrare acceptare Bitcoin pe website business completă. BTCPay/procesor configurat, test end-to-end executat, opțiune comunicată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-039",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "bitcoin", "payments", "e-commerce", "personal-finance", "btcpay"],
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
- Integrare fără test end-to-end cu tranzacție reală → violation
- Refolosire adrese Bitcoin (nu HD wallet) → violation de securitate
- Go-live fără testarea flow-ului complet de checkout → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Test end-to-end cu tranzacție reală executat?
- [ ] Adrese Bitcoin generate per-comandă (nu refolosite)?

**VK-uri obligatorii:**
1. `✅ [PROC] C-039 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Accept Bitcoin on Your Business Website" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| BTCPay Server | Procesor self-custody (recomandat) |
| OpenNode / CoinGate | Alternativă custodial simplu |
| WooCommerce / Shopify | Platformă e-commerce de integrat |
| VPS (DigitalOcean) | Hosting BTCPay Server |
| Bitcoin HD Wallet (xPub) | Generare adrese per-comandă |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Test end-to-end | Executat pre-go-live |
| Timp procesare plată (Lightning) | < 5 secunde |
| Timp procesare plată (on-chain) | 1-3 confirmări (~10-30 min) |
| Rata de succes plăți procesate | ≥ 99% |
| Monitorizare lunară utilizare | Documentată |
