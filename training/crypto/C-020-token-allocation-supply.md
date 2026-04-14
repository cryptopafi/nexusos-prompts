---
type: procedure
created: 2026-03-22
status: active
slug: c-020-token-allocation-supply
tags: [procedure, nexus]
---

# Design Token Allocation and Initial Supply — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Metodologie pentru proiectarea distribuției inițiale a tokenurilor și a supply-ului total, cu respectarea standardelor de echitate și sustenabilitate recunoscute în industrie.

---

## 1. Problema

Distribuția greșită a tokenurilor este unul dintre cei mai frecvenți factori de eșec și pierdere de încredere în proiecte crypto. O alocare cu prea mult la echipă și investitori fără vesting adecvat semnalează risc de "rug pull", în timp ce un supply prea mare sau prea concentrat afectează prețul și distribuția puterii de governance. Investitorii și comunitatea evaluează tokenomics-ul înainte de a investi.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

> **Safety Note**: Tokenomics-ul influențează direct valoarea tokenului. Distribuții inechitabile sau supply excesiv sunt red flags pentru investitori și pot fi considerate manipulare de piață.

Situații acoperite:
- Definirea supply-ului total și a mecanismelor de emisie (inflation/deflation)
- Proiectarea categoriilor de alocare: echipă, investitori, comunitate, treasury, ecosistem
- Configurarea schedule-ului de vesting pentru toate categoriile

---

## 2. Procedura

### Pas 1: Stabiliți supply-ul total și modelul de emisie
Decideți total supply (ex: 1 miliard tokeni — număr rotund recomandat pentru UX). Alegeți modelul: Fixed Supply (deflationist, ex: BTC, 21M), Inflationist cu rată fixă (ex: ETH post-merge, ~0.5%/an), sau Inflationist cu rată variabilă (staking rewards). Documentați justificarea alegerii. Definiți dacă tokenul va fi deflationist (burn mechanism) și rata de burn țintă.

### Pas 2: Definiți categoriile de alocare și procentajele
Alocați supply-ul pe categorii standard, respectând benchmark-urile industriei. Limite recomandate: Echipă & Fondatori: maximum 20% (ideal 15%); Investitori (seed, private, VC): maximum 20%; Comunitate & Airdrop: minimum 20%; Ecosistem & Grants: 15-20%; Treasury/DAO: 15-20%; Lichiditate inițială: 5-10%; Public sale (dacă există): 5-10%. Total = 100%. Documentați fiecare categorie cu justificare.

### Pas 3: Proiectați vesting schedule pentru echipă și investitori
Aplicați vesting obligatoriu pentru TOATE categoriile de insideri (echipă, fondatori, investitori): Cliff minim 6 luni (recomandat 12 luni) — niciun token eliberat în această perioadă. Vesting liniar pe 24-48 luni după cliff. Documentați graficul de unlock-uri în tabel și grafic vizual. Adăugați clauze de accelerare (termination, M&A) dacă este cazul. Publicați vesting schedule în whitepaper.

### Pas 4: Calculați circulant supply la lansare și schedule-ul de 36 luni
Calculați exact câți tokeni vor fi în circulație la TGE (Token Generation Event): tokeni publici vânduți + lichiditate inițială + airdrop imediat. Target recomandat: 5-15% din total supply la TGE. Construiți un model Excel cu eliberările lunare pentru 36 luni, categorii separate. Identificați "cliff walls" — luni cu unlock-uri masive care pot crea presiune de vânzare.

### Pas 5: Analizați distribuția puterii de governance
Calculați concentrația voturilor post-vesting: nicio entitate singulară nu ar trebui să dețină >33% din governance power (risc de governance takeover). Evaluați dacă comunitatea are putere reală de vot sau dacă echipa + investitorii pot trece orice propunere. Proiectați mecanism anti-whale dacă este relevant (quadratic voting, delegation). Documentați governance power distribution.

### Pas 6: Validați alocarea față de benchmark-urile industriei
Comparați alocarea finală cu proiecte de succes similare (Uniswap, Compound, Aave, Solana) — verificați pe Messari sau Token Terminal tokenomics-ul lor. Identificați orice abatere semnificativă de la benchmark-uri și justificați. Solicitați feedback extern: advisori, potențiali investitori, membri cheie ai comunității crypto. Ajustați alocarea pe baza feedback-ului primit.

### Pas 7: Documentați Token Allocation Document final
Compilați documentul final cu: Total Supply, Model de emisie, Tabel detaliat de alocare (categorie, %, tokens, $value la FDV țintă, vesting), Grafic vizual de distribuție (pie chart), Schedule de unlock lunare pe 36 luni, Governance power analysis. Publicați în whitepaper și website. Înregistrați în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Token allocation design completat: supply total definit, categorii de alocare cu vesting proiectate, schedule 36 luni calculat, Token Allocation Document finalizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-020",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "token-launch", "tokenomics", "allocation", "vesting", "supply", "governance"],
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
- Token allocation cu >50% la fondatori → violation (red flag pentru investitori)
- Vesting schedule absent pentru echipă sau investitori → violation critică
- Circulant supply la TGE >30% fără justificare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Echipă + investitori sub 40% total supply?
- [ ] Vesting cu cliff minimum 6 luni aplicat tuturor insider-ilor?

**VK-uri obligatorii:**
1. `✅ [PROC] C-020 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Design Token Allocation and Initial Supply" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Excel / Google Sheets | Modelare token allocation și vesting schedule |
| Messari / Token Terminal | Benchmark tokenomics proiecte similare |
| Token Allocation Document | Output principal al procedurii |
| Whitepaper | Publicare distribuție și vesting public |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Echipă + investitori | Sub 40% din total supply |
| Vesting cliff minim | 6 luni pentru toți insider-ii |
