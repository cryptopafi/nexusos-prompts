---
type: procedure
created: 2026-03-22
status: active
slug: c-022-token-prelaunch-marketing
tags: [procedure, nexus]
---

# Create a Pre-Launch Marketing Plan for Your Token — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru construirea unui plan de marketing pre-lansare pentru un token crypto, incluzând community building, content strategy, KOL outreach și campanii de awareness.

---

## 1. Problema

Proiectele crypto care lansează fără o comunitate pre-construită și un plan de marketing clar au rate de eșec mult mai mari — fără interes organic la lansare, tokenul nu atinge lichiditate minimă și prețul colapsează rapid. Marketingul crypto are specificități unice: importanța comunității Discord/Telegram, rolul KOL (Key Opinion Leaders) și a influencerilor crypto, și riscurile legale ale promovării unui token (potențiale acuzații de pump).

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Construirea comunității pre-lansare pe Discord, Telegram și Twitter/X
- Planificarea campaniilor de awareness și airdrop pentru atragerea utilizatorilor
- Strategia de content și outreach KOL în perioada pre-TGE

---

## 2. Procedura

### Pas 1: Definiți audiența țintă și canalele prioritare
Identificați profilul utilizatorului ideal: crypto natives cu experiență DeFi, retail investitori noi în crypto, sau utilizatori specifici nișei proiectului. Determinați unde se află această audiență: Twitter/X Crypto Twitter, Discord servere specifice, subreddit-uri relevante, Telegram grupuri. Stabiliți 3-5 canale prioritare bazat pe audience overlap și potențial de reach. Documentați Ideal User Profile (IUP).

### Pas 2: Creați identitatea vizuală și messaging-ul core
Definiți brand identity: logo, culori, font, ton de comunicare (profesional vs. degen-friendly). Articulați 3 key messages: ce este proiectul în 1 propoziție, de ce este diferit față de competiție, și de ce acum este momentul potrivit. Creați template-uri pentru Twitter, Discord announcements, și blog posts. Asigurați-vă că messaging-ul nu conține promisiuni de profit — legal risc ridicat.

### Pas 3: Construiți platformele de comunitate
Creați și configurați: Twitter/X (conta oficială + branding), Discord server (canale: #announcements, #general, #tokenomics, #roadmap, #support), Telegram canal de announcements și grup de discuții, website landing page cu waitlist/email capture. Implementați moderare: hire community managers sau volunteers, reguli clare de comunitate, bot anti-spam. Setați obiective pentru pre-lansare: 5.000 Discord members, 10.000 Twitter followers.

### Pas 4: Planificați campania de airdrop și early adopters
Proiectați un airdrop de pre-lansare care să recompenseze acțiuni valoroase: follow + RT pe Twitter, join Discord + role completion, testnet participation (dacă există), referral program. Definiți bugetul de airdrop (% din alocare community). Stabiliți criterii clare de eligibilitate și mecanisme anti-sybil (wallet age, on-chain activity requirements). Documentați regulile în FAQ public.

### Pas 5: Dezvoltați strategia de content marketing
Creați un content calendar pentru 8 săptămâni pre-lansare: Săptămânile 1-2: Team introduction + Project vision; Săptămânile 3-4: Tokenomics deep dives și FAQ; Săptămânile 5-6: Technical updates, testnet, partnerships; Săptămânile 7-8: Launch countdown, final airdrop snapshot, exchange listings. Tipuri de content: Twitter threads, Medium/Mirror articles, Discord AMAs, YouTube explainers.

### Pas 6: Implementați KOL și media outreach
Identificați 20-30 KOL relevanți în nișa proiectului cu audiență autentică (nu paid followers). Abordați KOL cu propunere clară: compensație (tokeni din alocare marketing), timeline, deliverables (tweet, thread, video). Obțineți minim 2-3 articole pe crypto media: CoinDesk, CoinTelegraph, Decrypt sau publicații mai mici specializate. Fiți transparent față de KOL că este conținut promovat — evitați riscurile legale.

### Pas 7: Documentați Marketing Plan și stabiliți KPIs de tracking
Compilați planul final de marketing: Audience Definition, Brand Identity, Channel Strategy, Community Targets, Airdrop Design, Content Calendar, KOL List și Budget, Media Outreach Plan, Budget Total. Definiți KPIs de măsurat săptămânal: Twitter followers, Discord members, Website visitors, Waitlist emails, Airdrop participants. Configurați dashboard de tracking (Google Analytics, Dune pentru on-chain metrics).

---

## 3. Cortex Logging

```json
{
  "text": "Pre-launch marketing plan completat: audiență definită, platforme comunitate create, airdrop proiectat, content calendar 8 săptămâni, KOL outreach planificat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-022",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "token-launch", "marketing", "community", "airdrop", "kol", "content-strategy"],
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
- Marketing materials care conțin promisiuni de profit sau ROI → violation legală
- KPIs de tracking neconfigurate înainte de campanie → violation
- Airdrop fără mecanisme anti-sybil → violation (duce la distribuție inechitabilă)
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Content calendar de 8 săptămâni completat?
- [ ] KPIs de tracking configurate cu valori target concrete?

**VK-uri obligatorii:**
1. `✅ [PROC] C-022 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Create a Pre-Launch Marketing Plan for Your Token" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Discord / Telegram | Platforme principale de comunitate |
| Twitter/X | Canal principal de comunicare publică |
| Google Analytics | Tracking website și conversii |
| Dune Analytics | Tracking on-chain airdrop participanți |
| Mirror / Medium | Publicare content tehnic și tokenomics |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Discord members pre-launch | 5.000 minimum |
| Twitter followers pre-launch | 10.000 minimum |
