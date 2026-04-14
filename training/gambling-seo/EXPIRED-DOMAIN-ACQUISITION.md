---
type: procedure
created: 2026-03-22
status: active
slug: expired-domain-acquisition
tags: [procedure, nexus]
---

# TRAIN-SEO-002: EXPIRED-DOMAIN-ACQUISITION — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Spamzilla Methodology + Auction Strategy + PBN Domain Sourcing -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Domain Acquisition / PBN Sourcing / Metrics Screening -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-002
**Scope**: Identificarea, evaluarea, achiziția și activarea domeniilor expirate cu autoritate reală pentru construirea unui PBN (Private Blog Network) în nișa gambling/casino.

---

## 1. Problema

Construirea unui PBN eficient pentru gambling/casino SEO necesită domenii expirate cu autoritate autentică, nu sintetică. Fără o metodologie de screening riguroasă, operatorul riscă să cumpere domenii penalizate de Google, cu backlink-uri spam (GSA SER history), cu schimbări de limbă sau proprietar care indică activitate SEO anterioară, sau cu arhivă lipsă — toți factori care anulează investiția și pot transfera penalități către money site. Această procedură acoperă întregul workflow: sourcing → screening SZ Score → verificare metrici → decizie auction → post-win onboarding.

**Context nișă**: Domeniile pentru gambling PBN trebuie să aibă istoric credibil (fostă nișă non-spam), TF ≥ 15, autoritate reală demonstrată prin BL/MDP ratio ≈ 2, și zero indicatori de activitate SEO artificială anterioară.

---

## 2. Procedura

### Pas 1: SETUP TOOL STACK

Configurează toolset-ul minim necesar înainte de orice sourcing:

**1.1 — Spamzilla ($37/mo) — tool PRIMAR**
- Creează cont pe spamzilla.io
- Conectează Majestic API (pentru TF/CF/TR live)
- Conectează Ahrefs API (pentru DR și backlink history)
- Configurează filtrul implicit: TF ≥ 15, BL ≥ 100, SZ Score fără iconiță roșie (cross)

**1.2 — expireddomains.net (FREE) — tool secundar**
Configurează Column Manager cu exact aceste coloane:
- **General tab**: LE (Listing Extension), BL (Backlinks), WBY (Wayback Year)
- **SEOkicks tab**: DP (Domain Pop)
- **Status TLD tab**: C (Creation date), N (Nameservers), O (Open), B (Bing index), I (Google Index), D (DMOZ)
- **Related tab**: RDT (Related Domains)
- **Majestic General tab**: CF (Citation Flow), TF (Trust Flow), TR (Topical Trust Flow), MDP (Majestic Referring Domains)
- **Majestic RDs tab**: MRDL (Majestic Referring Domains Live)

**1.3 — Auction platforms (configurare conturi)**
- GoDaddy Auctions: cont activ, credit card validat
- Namejet: cont activ, $69 backorder preîncărcat
- DropCatch: cont activ
- Namesilo + CatchClub: opțional, pentru domenii .com ieftine
- Dynadot: cont activ pentru overlap coverage

---

### Pas 2: SOURCING DOMENII CANDIDATE

**2.1 — Sourcing via Spamzilla**
Aplică filtrul de bază în Spamzilla:
```
TF ≥ 15
BL ≥ 100
SZ Score: NO cross icon (iconiță roșie = eliminat automat)
Language: EN (sau limba țintă a PBN-ului)
Category: ANY — dar evită Adult, Pharma, Gambling (risc footprint)
```
Exportă lista brută. Target: minim 50 candidați pentru fiecare batch de screening.

**2.2 — Sourcing via expireddomains.net**
Aplică filtrul cu coloanele configurate la Pas 1.2:
```
TF ≥ 15
BL ≥ 100
I = "Yes" (indexat în Google — confirmare că nu e penalizat complet)
Exclude domenii cu WBY lipsă (fără Wayback history = risc mare)
```
Combină lista cu cea din Spamzilla — elimină duplicatele.

---

### Pas 3: SCREENING SZ SCORE (SPAMZILLA — OBLIGATORIU)

Acesta este filtrul critic. Fiecare domeniu trece prin 7 verificări SZ Score. **Un singur FAIL = eliminare imediată.**

**Check 3.1 — Archive.org History**
- Deschide arhiva domeniului pe web.archive.org
- Întreabă: Ce era site-ul? Nișă legitimă (health, travel, tech, news, local biz) = PASS
- IF site-ul era: adult, pharma, gambling, casino, crypto spam, link directory → **ELIMINARE**
- IF arhiva lipsă complet (0 snapshots) → **ELIMINARE** (nu există istoric credibil)
- IF arhiva existentă dar extrem de rară (1-2 snapshots vechi) → FLAG, verificare manuală suplimentară

**Check 3.2 — Backlinks și Anchor Texts**
Descarcă lista de backlink-uri din Spamzilla (sau Ahrefs):
- Scanează anchor texts pentru: simboluri speciale (@, #, %, |, &), caractere non-latine, casino/viagra/bitcoin/porn keywords
- IF anchors conțin simboluri sau special characters → **ELIMINARE** (semnătură GSA SER sau similar bulk tool)
- IF anchors sunt în proporție > 30% exact-match keywords comerciale → **FLAG**, risc penalty
- PASS: anchors predominant branded, URL, generic (click here, website, etc.)

**Check 3.3 — Redirects History**
- Verifică în Spamzilla secțiunea "Redirects"
- IF domeniu a funcționat ca redirect (301/302) spre alt domeniu → **ELIMINARE** (indiciu de link juice farming)
- PASS: zero redirects în istoric

**Check 3.4 — Number of Drops**
- Verifică câte ori a expirat și a fost re-înregistrat domeniul ("drops count")
- IF drops ≥ 3 → **ELIMINARE** (domeniu reciclat repetat = suspectat de SEO serial)
- IF drops = 2 → FLAG, verificare manuală mai atentă a fiecărei perioade de posesie
- PASS: drops ≤ 1

**Check 3.5 — Google Index Status**
- Verifică în Spamzilla (sau manual: `site:domain.com` în Google)
- IF domeniu complet deindexat (0 rezultate `site:`) → **ELIMINARE** (posibilă penalizare manuală sau algoritmică)
- IF domeniu indexat partial (câteva pagini) → PASS, dar verificare manuală penalties (Pas 4)
- PASS: indexat cu ≥ 1 pagini

**Check 3.6 — Language History**
- Verifică secțiunea "Language History" în Spamzilla sau snapshots Wayback
- IF site-ul a schimbat limba (ex: EN → RO → EN, sau EN → ZH) → **ELIMINARE** (pattern de re-utilizare SEO)
- PASS: limbă consistentă în tot istoricul

**Check 3.7 — WHOIS History (Owner Changes)**
- Verifică în Spamzilla secțiunea WHOIS History
- IF au existat > 2 schimbări de proprietar în ultimii 5 ani → **FLAG**, verificare dacă coincid cu perioade de activitate SEO
- IF schimbarea de proprietar e recentă (< 6 luni) + site-ul era activ → **ELIMINARE** (posibil SEO anterior recent)
- PASS: proprietar stabil sau schimbare veche (> 3 ani) fără activitate SEO corelată

---

### Pas 4: VERIFICARE METRICI MINIME

Domeniile care au trecut toate Check-urile SZ Score intră în verificarea numerică. **Toate pragurile sunt HARD — niciun criteriu nu e negociabil.**

**Metrici obligatorii (toate trebuie PASS simultan):**

| Metrică | Prag minim | Tool |
|---------|-----------|------|
| Trust Flow (TF) | ≥ 15 | Majestic |
| Backlinks (BL) | ≥ 100 | Majestic / Ahrefs |
| Domain Authority (DA) | ≥ 30 (preferred) | MOZ |
| Spam Score (MOZ) | < 10% | MOZ |
| BL/MDP Ratio | ≈ 2 (interval 1.5 – 3.0) | Majestic |
| Google penalizare manuală | ZERO | Google Search Console* |

*Notă GSC: verificarea penalizării manuale se face DUPĂ achiziție (Pas 7). Pre-achiziție: folosește `site:domain.com` + Google Transparency Report ca proxy.

**Decision tree metrici:**
```
TF < 15 → ELIMINARE
TF ≥ 15 → verifică BL
  BL < 100 → ELIMINARE
  BL ≥ 100 → verifică DA
    DA < 20 → ELIMINARE (sub prag minim absolut)
    DA 20-29 → FLAG (acceptabil doar dacă TF ≥ 20 și BL ≥ 200)
    DA ≥ 30 → verifică Spam Score
      Spam Score ≥ 10% → ELIMINARE
      Spam Score < 10% → verifică BL/MDP ratio
        Ratio < 1.0 sau > 5.0 → ELIMINARE (distribuție anormală)
        Ratio 1.0-1.4 sau 3.1-5.0 → FLAG, verificare manuală backlink diversity
        Ratio 1.5-3.0 → PASS → intră în shortlist
```

**BL/MDP Ratio — explicație:**
- Formula: Total Backlinks ÷ Total Referring Domains = Ratio
- Ratio ≈ 2 înseamnă că fiecare domeniu referrer trimite în medie 2 linkuri = natural
- Ratio > 5 = un număr mic de domenii trimit zeci de linkuri = posibil link scheme
- Ratio < 1 (imposibil matematic dar poate indica date inconsistente) → re-verifică sursele

---

### Pas 5: SHORTLIST ȘI PRIORITIZARE

**5.1 — Construiește shortlist-ul**
Domeniile care au trecut Pas 3 (SZ Score) + Pas 4 (metrici) intră în shortlist. Sortează descrescător după:
1. TF (prioritate maximă)
2. DA
3. BL (cu ratio sănătos)
4. Relevanța nișei istorice față de gambling/casino adiacent (sports, entertainment, finance = mai bine decât tech B2B)

**5.2 — Estimare cost și buget**
- Domenii la auction GoDaddy: estimează prețul din istoricul de licitații similare (tool: NameBio)
- Domenii Namejet: $69 backorder + auction dacă > 1 backorder
- Domenii DropCatch: prețuri competitive, verifică overlap cu alte platforme
- Buget recomandat per domeniu de calitate: $50 – $500 (excepțional: până la $1000 pentru TF ≥ 25)

---

### Pas 6: TACTICI AUCTION (FĂRĂ BIDDING WAR)

**6.1 — GoDaddy Auctions**
- **NU licita direct vizibil** — folosește funcția **Wishlist** pentru a ascunde interesul față de competitori
- Monitorizează prețul din Wishlist până în ultimele 2-4 ore ale licitației
- Plasează bid-ul maxim o singură dată în ultimele 30 de minute (bid sniping)
- Stabilește un **hard maximum bid** înainte de auction. **Nu depăși niciodată maximul stabilit.**
- IF alt bidder escaladează prețul peste maximul tău → **ABANDON** (nu intra în bidding war)
- Licitațiile durează 3-6 zile — monitorizare zilnică

**6.2 — Namejet**
- Plasează backorder ($69) pentru a intra în lista de interes
- IF un singur backorder → domeniu cade la tine la prețul de backorder
- IF multiple backorders → intrare automată în mini-auction între cei care au backorder-uit
- Tactică: backordering cu 5-7 zile înainte de drop date (verifică drop date pe domaintools.com)

**6.3 — DropCatch / Namesilo + CatchClub**
- DropCatch: folosit pentru domenii .com la prețuri competitive, dar overlap cu GoDaddy frecvent
- CatchClub pe Namesilo: pentru domenii sub $20, cost-effective pentru PBN tier 2
- Dynadot: util pentru extensii non-.com (.net, .org, .info)

**6.4 — Anti-patterns (interzise)**
- Nu licita pe același domeniu pe mai multe platforme simultan fără a seta maxim per platformă
- Nu discuta domeniile vizate în forumuri sau grupuri publice (ridică competiția)
- Nu cumpăra domeniu dacă prețul depășește 3x valoarea estimată a autorității (calcul: TF x $10 = valoare maximă rezonabilă)

---

### Pas 7: POST-WIN ONBOARDING (OBLIGATORIU — în această ordine exactă)

**Această ordine nu poate fi schimbată. Fiecare pas este prerequisite pentru următorul.**

**7.1 — Hosting și WordPress**
- Înregistrează domeniu câștigat dacă nu e auto-transferat
- Host pe IP dedicat sau C-class block separat față de alte PBN sites (footprint prevention)
- Instalează WordPress cu temă simplă, fără plugin-uri inutile
- Configurează un email de contact generic

**7.2 — Instalare SEO Plugin**
- Instalează Yoast SEO sau Rank Math
- Configurează: sitemap XML activat, robots.txt standard, meta tags activat

**7.3 — Conectare Google Search Console**
- Adaugă domeniu în GSC folosind metoda **meta tag** (nu DNS — footprint mai mic)
- Verificare proprietate: copiază meta tag → paste în `<head>` al temei WordPress
- Confirmă că GSC arată domeniu verificat

**7.4 — VERIFICARE PENALIZARE MANUALĂ (CRITICAL)**
Imediat după verificare GSC:
- Navighează în GSC: Security & Manual Actions → Manual Actions
- **IF există orice manual action (Partial sau Sitewide)** → **ABANDONEAZĂ DOMENIU COMPLET**
  - Nu investi timp în recuperare
  - Nu construi content
  - Marchează domeniu ca DISCARD în tracking sheet
- **IF zero manual actions** → PASS → continuă la 7.5

**7.5 — Activare conținut (doar după 7.4 PASS)**
Abia după confirmarea zero penalizări:
1. Identifică nișa și keyword-urile target (gambling-adjacent sau direct)
2. Publică conținut (minim 3 articole la lansare, 500-1000 cuvinte fiecare)
3. Optimizare SEO internă: title tags, meta descriptions, internal links
4. Instalare affiliate links sau CTA toward money site
5. CRO de bază: above-fold CTA, clear navigation

---

### Pas 8: TRACKING ȘI DOCUMENTARE

Menține un **Domain Acquisition Tracker** (Google Sheets sau Airtable) cu coloanele:

| Coloană | Descriere |
|---------|-----------|
| Domain | Numele domeniului |
| TF | Trust Flow la achiziție |
| DA | Domain Authority la achiziție |
| BL | Total backlinks |
| MDP | Total referring domains |
| BL/MDP Ratio | Calculat |
| Spam Score | % MOZ |
| SZ Score | PASS/FAIL per check |
| Former Niche | Ce era site-ul în arhivă |
| Auction Platform | GoDaddy / Namejet / etc. |
| Price Paid | USD |
| GSC Manual Penalty | CLEAR / FOUND (→ DISCARD) |
| Status | Active PBN / Discard / Pending |
| Date Acquired | YYYY-MM-DD |
| IP / Host | Pentru footprint tracking |

Actualizează tracker-ul în maxim 24h după fiecare achiziție.

---

## 3. Cortex Logging

```json
{
  "text": "Expired domain acquisition batch completed: [N] candidates screened, [N] passed SZ Score, [N] passed metrics, [N] acquired. Domains: [list]. Avg TF: [x]. Avg price: $[x]. Manual penalties found post-win: [N] (discarded).",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "EXPIRED-DOMAIN-ACQUISITION",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-002",
    "tags": ["expired-domains", "pbn", "gambling-seo", "casino-seo", "domain-authority", "spamzilla", "trust-flow"],
    "has_enforcement_loop": true,
    "version": "2.0",
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție + Gambling SEO project execution

### WHEN
La fiecare batch de sourcing sau achiziție individuală de domeniu expirat

### HOW (violation detection)
- Domeniu achiziționat fără SZ Score complet (toate 7 checks) → violation HARD
- Domeniu activat înainte de verificarea GSC manual penalties → violation HARD
- Metrici minime nechecked înainte de bid → violation
- Bidding war intrat (preț depășit față de maximul stabilit) → violation
- Tracker neactualizat în 24h post-achiziție → violation
- Ordine Pas 7 (post-win) schimbată → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:**
- Activarea unui domeniu cu manual penalty GSC (indiferent de calitatea metricsilor)
- Cumpărarea unui domeniu cu SZ Score cross icon (iconiță roșie) — nicio excepție
- Discutarea domeniilor vizate în spații publice înainte de achiziție
- Depășirea hard maximum bid pentru un domeniu

### CONNECT
- TRAIN-SEO-002 → enforcement gambling SEO curriculum
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum Gambling SEO / PBN Building
- Prerequisite pentru: PBN-SITE-SETUP.md, CONTENT-DEPLOYMENT-PBN.md
- Depinde de: GAMBLING-SEO-STRATEGY.md (pentru target nișe și piețe)

### VERIFY
La finalul fiecărui batch de achiziție, verifică:
- [ ] Toți pașii din §2 executați în ordine?
- [ ] Toate 7 SZ Score checks documentate per domeniu?
- [ ] Toate 6 metrici minime verificate per domeniu?
- [ ] Domeniile câștigate au primit GSC manual penalty check ÎNAINTE de orice activare?
- [ ] Tracker actualizat cu toate domeniile (inclusiv DISCARD-urile)?
- [ ] Niciun bidding war — maximul pre-stabilit respectat?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] EXPIRED-DOMAIN-ACQUISITION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Expired Domain Acquisition for Gambling/Casino PBN" | FORGE ✓ | rule: TRAIN-SEO-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură / sourcing | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | AUDIT-PRO |
| Bulk SZ Score batch analysis | Haiku (clasificare) |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Spamzilla ($37/mo) | Tool primar: SZ Score, WHOIS history, archive.org integrat, anchor analysis |
| expireddomains.net (FREE) | Sourcing secundar cu column setup specific |
| Majestic (API sau manual) | Trust Flow (TF), Citation Flow (CF), Referring Domains (MDP) |
| MOZ (API sau manual) | Domain Authority (DA), Spam Score |
| Ahrefs (optional) | Backlink history cross-check, DR verification |
| GoDaddy Auctions | Platform primară de licitație domenii expirate |
| Namejet | Backorder $69 pentru domenii cu interes crescut |
| DropCatch / Dynadot / Namesilo+CatchClub | Platforme secundare pentru coverage și cost-efficiency |
| Google Search Console | Verificare penalizare manuală post-achiziție (MANDATORIU) |
| web.archive.org | Verificare manuală istoric site când Spamzilla nu acoperă |
| NameBio | Estimare prețuri istorice pentru auctions similare |
| Domain Acquisition Tracker (Sheets/Airtable) | Documentare completă per domeniu |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate (toți pașii) | 100% |
| VK emisie | 2/2 per execuție |
| SZ Score pass rate (din candidați bruti) | Variabil — tipic 10-20% din lista brută |
| Metrics pass rate (din SZ Score PASS) | Variabil — tipic 30-50% |
| Manual penalty found post-win | 0 (target ideal) — max tolerabil: < 10% din achiziții |
| Tracker actualizat | 100% în 24h |
| Bidding war incidents | 0 |
| Avg TF al domeniilor achiziționate | ≥ 17 |
| Cost per domeniu (avg) | < $200 per domeniu cu TF ≥ 15 |
| Timp per screening complet (per domeniu) | 15-25 minute |
