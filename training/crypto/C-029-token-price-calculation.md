---
type: procedure
created: 2026-03-22
status: active
slug: c-029-token-price-calculation
tags: [procedure, nexus]
---

# Calculate and Project Token Price — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Metodologie structurată pentru calcularea prețului curent al tokenului și proiectarea scenariilor de prețuri bazate pe market cap și supply.

---

## 1. Problema

Fondatorii și investitorii fac frecvent erori grave în calculul și proiecția prețului tokenului — fie ignorând supply-ul circulant, fie bazând scenariile pe market cap-uri nerealiste fără fundament comparativ. Aceste erori pot duce la decizii financiare greșite și la pierderi semnificative.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Calcularea prețului inițial la lansarea tokenului bazat pe lichiditate și supply
- Proiectarea scenariilor de preț la diferite niveluri de market cap
- Analiza comparativă cu tokeni similari pentru validarea realismului proiecțiilor

---

## 2. Procedura

### Pas 1: Determină parametrii de bază ai tokenului
Colectează: total supply, circulating supply la lansare (după unlock schedule), procentul destinat lichidității inițiale, și prețul perechii (ex. ETH/SOL/USDC). Documentează acești parametri — sunt inputs pentru toate calculele.

### Pas 2: Calculează prețul inițial din lichiditate
Formula: Preț inițial = Valoarea activului de pereche depus / Numărul de tokeni în pool. Ex: dacă depui 1 ETH ($3,000) la 3,000,000 tokeni, prețul = $0.001/token. Verifică că raportul este consistent cu tokenomics-ul declarat.

### Pas 3: Calculează market cap-urile de referință
Calculează: Market Cap = Preț × Circulating Supply. Calculează pentru 3 scenarii: bear (×2 față de lansare), base (×10), bull (×50). Documentează market cap-ul în USD pentru fiecare scenariu. Verifică că aceste cifre sunt plauzibile față de proiectul concret.

### Pas 4: Analizează tokeni comparabili (comps)
Identifică 3-5 tokeni cu utilitate similară și market cap similar la lansare. Verifică market cap-ul lor curent pe CoinGecko. Folosește aceste date ca benchmark realist pentru scenariile tale. Un proiect similar cu MC de $5M la 6 luni validează scenariul tău base.

### Pas 5: Identifică factorii care influențează prețul
Listează catalizatorii potențiali: listări pe CEX, parteneriate, update-uri de protocol, campanii marketing. Estimează impactul fiecăruia pe preț/volum. Identifică și riscurile: unlock-uri de tokens, competiție, bear market general.

### Pas 6: Construiește tabela de proiecție
Creează un tabel cu coloanele: Scenariu | MC (USD) | Preț/Token | Multiplier față de lansare | Probabilitate estimată | Catalizator principal. Completează pentru scenariile bear/base/bull. Salvează ca document de referință al proiectului.

### Pas 7: Documentează asumpțiile și limitele
Adaugă obligatoriu o secțiune "Asumpții și Limitări": precizează că proiecțiile nu sunt garanții, că supply-ul circulant se poate modifica prin unlock-uri, că condițiile de piață sunt volatile. Finalizează și salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Calcul și proiecție preț token completat. Scenarii bear/base/bull documentate cu market cap, comparabili identificați, asumpții declarate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-029",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "tokenomics", "price-projection", "market-cap", "token-launch"],
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
- Proiecție preț bazată pe market cap speculativ fără fundament → violation
- Nicio analiză comparativă cu tokeni similari → violation
- Asumpții și limitări nedocumentate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Proiecțiile au fundament comparativ (comps)?
- [ ] Secțiunea de asumpții și limitări prezentă?

**VK-uri obligatorii:**
1. `✅ [PROC] C-029 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Calculate and Project Token Price" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CoinGecko API | Date market cap tokeni comparabili |
| DEX Pool Data | Preț curent din lichiditate |
| Tokenomics document | Supply total și schedule unlock |
| Spreadsheet (Google Sheets/Excel) | Calcule și tabel proiecție |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Scenarii documentate | 3 (bear/base/bull) |
| Tokeni comparabili analizați | Minim 3 |
| Acuratețe calcul preț inițial | Verificat față de pool real |
| Secțiune limitări prezentă | 100% obligatoriu |
