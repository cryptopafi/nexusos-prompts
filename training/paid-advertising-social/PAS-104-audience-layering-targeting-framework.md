---
id: PAS-104
title: Audience Layering and Targeting Framework
domain: paid-advertising-social
source: TikTok Ads Mastery — Audience Strategy
version: 2.0
forge_score: "estimated M/3.5"
---

# Audience Layering and Targeting Framework

## Obiectiv
Construirea unui sistem de audiențe 3-tier (rece/caldă/fierbinte) cu layering interese, comportamente, Lookalike și Custom Audiences, cu budget allocation per tier, exclusions management și specificități TikTok vs Meta.

## Pași

### 1. Modelul 3-tier de audiențe (Cold → Warm → Hot)
**Tier 1 — Cold** (nu te cunoaște):
- Sursă: Interese, comportamente, demographics, Broad
- Budget: 40-50% din total | ROAS așteptat: 1-2x
- Mesaj: educație, introducere brand, problem-awareness
- CPM Meta: €8-15 | CPM TikTok: €5-10

**Tier 2 — Warm** (a interacționat):
- Sursă: Video viewers 25%+, IG/TikTok engagers, website visitors 60+ sec
- Budget: 30-40% din total | ROAS așteptat: 2-4x
- Mesaj: social proof, demo detaliat, ofertă soft

**Tier 3 — Hot** (gata să cumpere):
- Sursă: Website visitors 30d, Add-to-Cart no purchase, email list, past purchasers LAL
- Budget: 20-30% din total | ROAS așteptat: 4-10x
- Mesaj: urgency, discount, reminder, testimoniale

### 2. Cold audiences — Meta vs TikTok specifics
**Meta Interest Layering**:
- Max 3 interese per AdSet (mai multe = pierzi precizie)
- Test interese separate: 1 AdSet = 1 interes (date curate)
- Interese volum mic (<500K): combină cu behavior overlay
- Budget: €10-20/zi per AdSet interest test

**TikTok Cold Targeting**:
- Interest & Behavior: categorii largi (Cooking, Travel, Food)
- Video Interaction: viewers conținut similar/competitori
- Creator Interaction: followers creatorilor din nișă
- **Broad Audience** (fără targeting): funcționează surprinzător de bine pe TikTok — algoritmul e superior la discovery

### 3. Custom Audiences (Warm + Hot) — setup complet
**Meta**: Ads Manager → Audiences → Custom Audience:
- Video Viewers: IG/FB video → 25% sau 50% watched → 30/60 zile
- Website Visitors: Pixel necesar → All / Product Page / Checkout / Purchase
- Customer List: CSV upload (GDPR: consent explicit obligatoriu!)
- Instagram Engagers: profile visitors, post engagers, story viewers

**TikTok**: Ads Manager → Audience Management:
- Pixel Events: View Content, Add to Cart, Purchase
- Engagement: Video Viewers, Profile Visitors, Followers
- App Activity: in-app events (dacă ai app)

### 4. Lookalike Audiences — strategie optimă
**Surse LAL (în ordine calitate)**:
1. Purchasers (minim 100 persoane — cel mai valoros)
2. High-Value Customers (top 25% valoare comandă)
3. Lead Form Completers
4. Video Viewers 75%+
5. Website Visitors All (cel mai puțin precis)

**Procente LAL**:
- 1%: cel mai similar, audiență mică, cost mai mare
- 2-5%: balance similitudine/volum (sweet spot)
- 10%+: broad, replacement interest targeting

**Stacking LAL**: 3 AdSets — LAL 1%, LAL 2-5%, LAL 5-10% → testează care performează

### 5. Exclusions management (igiena audiențelor)
- **EXCLUD MEREU**: purchasers recenți (30d) din conversie campaigns
- **EXCLUD**: audiența hot din cold campaigns (evită overlap)
- **Overlap Tool**: Meta Audiences → Overlap → dacă 2 AdSets >30% overlap → consolidează
- **TikTok**: exclusions la nivel Ad Group → Audiences → Excluded
- **Frequency cap**: monitor frequency per AdSet — >3 (cold) sau >5 (warm) = saturație

### 6. Budget allocation per tier (formule)
```
Budget zilnic €100:
├── Cold (40-50%): €40-50/zi → 2-3 AdSets interest/broad
├── Warm (30-40%): €30-40/zi → retargeting video viewers, engagers
└── Hot (20-30%): €20-30/zi → website visitors, cart abandoners

Scaling la €500/zi:
├── Cold: €200-250/zi → expand LAL, noi interese
├── Warm: €150-200/zi → expand retargeting windows
└── Hot: €100-150/zi → email list, purchase LAL
```

### 7. TikTok "Broad" strategy (algorithm-first approach)
Pe TikTok, "No Targeting" (Broad) cu creative bun bate adesea targeting precis:
- Algoritmul TikTok e mai avansat decât Meta la discovery
- Broad funcționează când: creative-ul e puternic + pixel are 50+ conversii
- Avantaj: zero audience exhaustion, scalabil infinit
- Dezavantaj: necesită creative excelent (hook rate 40%+ obligatoriu)

## Verificare
- [ ] Custom Audiences create pentru toate 3 tiers (pixel instalat corect)
- [ ] Lookalike din sursa cea mai valoroasă (purchasers/leads minim 100)
- [ ] Purchasers recenți excluși din prospecting campaigns
- [ ] Overlap check făcut între AdSets principale (<30%)
- [ ] Budget alocat 40/30/20 sau 50/30/20 conform tier model
- [ ] TikTok Broad audience testat (dacă pixel matur)
- [ ] GDPR compliance verificat pentru customer list upload
- [ ] Frequency monitoring setat per AdSet

## Instrumente
- Meta Audience Insights — research demografic și interese
- Meta Ads Manager → Audiences → Overlap Tool
- TikTok Ads Manager → Audience Management
- Pixel Helper (Chrome extension) — verifică pixel events
- Google Sheets — audience mapping și budget allocation

## Note
- TikTok Broad cu creative bun > targeting precis — algoritmul TikTok superior la discovery
- 50+ conversii/săptămână per AdSet = learning phase stabilă (sub: mergi Broad)
- Local business (Albastru): Geo (50km) + Interese (travel/food) + LAL clienți existenți
- CPM Meta €8-15 vs TikTok €5-10 vs Snapchat €3-7
