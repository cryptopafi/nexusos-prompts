---
type: procedure
created: 2026-03-22
status: active
slug: c-030-token-launch-day-marketing
tags: [procedure, nexus]
---

# Launch Day Marketing and Post-Launch Promotion — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Planificarea și execuția campaniei de marketing pentru ziua lansării tokenului și primele 72 de ore post-lansare.

---

## 1. Problema

Ziua lansării unui token este cel mai critic moment pentru adoptarea inițială și price discovery. Fără o strategie coordonată de marketing pe multiple canale, lansările eșuează în obscuritate chiar și cu produse solide. Primele 72 de ore determină dacă tokenul câștigă tracțiune sau se scufundă în trading sideways.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Lansare token nou pe DEX (prima listare publică)
- Re-lansare token după rebranding sau migrare pe alt chain
- Campanie coordonată de awareness pentru un token existent cu listing nou

---

## 2. Procedura

### Pas 1: Pregătește pachetul de launch assets (cu 24h înainte)
Creează: announcement post (Twitter/X), banner grafic cu adresa contractului și link pool, thread Twitter pre-scris (10+ tweet-uri), mesaj Telegram/Discord pentru comunitate, și press release scurt pentru crypto media. Toate asset-urile trebuie gata înainte de ora lansării.

### Pas 2: Coordonează embargoul și timing-ul
Stabilește ora exactă de lansare (ex. 15:00 UTC pentru acoperire maximă globală). Informează KOL-urile și partenerii cu 2 ore înainte sub embargo. Programează post-urile automate (Buffer/Hootsuite) și verifică că pool-ul de lichiditate este activ cu 15 minute înainte de anunț.

### Pas 3: Execută anunțul simultan pe toate canalele
La ora stabilită, publică simultan pe: Twitter/X (thread principal), Telegram (grup comunitate + canale partenere), Discord, Reddit (r/CryptoCurrency, r/DeFi, subreddituri relevante), și trimite email la lista de waitlist. Asigură-te că adresa contractului verificat este inclusă în toate postările.

### Pas 4: Activează partenerii și KOL-urile
Trimite semnalul convenite partenerilor și influencerilor că embargo-ul s-a ridicat. Monitorizează că postează conform înțelegerii. Repostează și engaged cu postările lor (like, RT, reply). Documentează care parteneri au livrat și care nu.

### Pas 5: Monitorizează și răspunde la comunitate (primele 6h)
Monitorizează toate canalele în timp real. Răspunde la întrebări tehnice (cum cumpăr, cum adaug la MetaMask). Fixează mesajele esențiale în Telegram/Discord. Combate dezinformarea cu fapte și link-uri verificate. Postează update-uri la fiecare oră cu volume și milestone-uri.

### Pas 6: Execută campania post-launch (24h-72h)
Postează conținut regulat: volume milestones (ex. "1000 holders!"), mulțumiri comunității, FAQ thread, AMA (Ask Me Anything) live. Continuă engagement cu KOL-uri pentru al doilea val de postări. Trimite PR la site-uri de crypto news (CoinTelegraph, Decrypt, BeInCrypto).

### Pas 7: Compilează raportul de launch și salvează în Cortex
La 72h post-lansare, documentează: volum tranzacționat, număr holderi, peak price vs launch price, engagement social (impressions, shares), parteneri care au livrat, ce a funcționat și ce nu. Salvează raportul și Cortex log.

---

## 3. Cortex Logging

```json
{
  "text": "Campanie launch day executată. Anunț simultan pe toate canalele, parteneri activați, monitorizare 72h completă, raport de lansare documentat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-030",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "marketing", "token-launch", "community", "social-media"],
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
- Launch assets nepregatite cu 24h înainte de lansare → violation
- Anunț fără adresa contractului verificat → violation
- Nicio monitorizare a canalelor în primele 6 ore → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Launch assets pregătite pre-lansare?
- [ ] Raport 72h compilat și salvat?

**VK-uri obligatorii:**
1. `✅ [PROC] C-030 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Launch Day Marketing and Post-Launch Promotion" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Twitter/X | Canal principal anunț |
| Telegram / Discord | Comunitate directă |
| Buffer / Hootsuite | Programare postări automate |
| CoinGecko / DexScreener | Monitorizare volum și preț |
| Crypto PR Sites | Distribuție press release |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Canale activate la lansare | Minim 5 |
| Timp răspuns comunitate (primele 6h) | < 15 minute per întrebare |
| Engagement Twitter (primele 24h) | Minim 500 impressii |
| Holderi la 72h | Minim 100 |
| Volum tranzacționat 24h | Documentat și comparat cu target |
