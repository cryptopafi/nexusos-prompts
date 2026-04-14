---
id: PAS-109
title: Cross-Platform Ad Performance Comparison Framework
domain: paid-advertising-social
source: TikTok Ads Mastery — Multi-Platform Analytics
version: 2.0
forge_score: "estimated M/3.5"
---

# Cross-Platform Ad Performance Comparison Framework

## Obiectiv
Framework standardizat pentru compararea performanței între Meta, TikTok și alte platforme, cu normalizare KPIs prin GA4 hub central, CPM benchmarks per platformă, attribution window uniformizare, raport lunar template și decision framework alocare buget.

## Pași

### 1. De ce NU poți compara direct KPIs brute
- Meta vs TikTok: ferestre de atribuire diferite, modele de conversie diferite
- CPM diferă enorm: Meta €8-15 vs TikTok €5-10 vs Snapchat €3-7
- Audiențe diferite demografic: TikTok 18-35 | Meta 25-55
- "Conversion" = lucruri diferite per platformă

**Soluția**: normalizare prin metrici de BUSINESS (CPA, ROAS, CAC) măsurate în GA4, nu în platforme.

### 2. GA4 ca hub central measurement
- Instalează GA4 pe site (via Google Tag Manager)
- UTM parameters OBLIGATORII pe fiecare campanie:
  - `utm_source=tiktok&utm_medium=paid&utm_campaign=Spring2026`
  - `utm_source=facebook&utm_medium=paid&utm_campaign=Spring2026`
- Goals/Conversions identice în GA4 (Purchase, Lead, etc.)
- Raport: Acquisition → Traffic Acquisition → filtrează pe utm_source
- **Sursa de adevăr**: GA4/CRM, NU dashboardurile Meta/TikTok

### 3. Attribution windows uniformizate
- **Meta**: setează 7-day click, 1-day view (Account Settings → Attribution)
- **TikTok**: setează 7-day click, 1-day view (identic cu Meta)
- **Snapchat**: 7-day click, 1-day view
- Astfel toate platformele raportează pe aceeași fereastră → comparație echitabilă
- Discrepanță GA4 vs platformă: 20-50% diferență e normală — GA4 = adevărul

### 4. KPI Matrix comparativă normalizată (2026)
| KPI | Meta | TikTok | Snapchat | Interpretare |
|-----|------|--------|----------|-------------|
| CPM | €8-15 | €5-10 | €3-7 | TikTok/Snap mai ieftine |
| CTR | 0.8-2% | 0.5-1.5% | 0.3-1% | Meta mai bun CTR |
| CPC | €0.30-1.50 | €0.20-1.00 | €0.15-0.80 | Snap cel mai ieftin |
| CVR (site) | 1-5% | 0.5-3% | 0.3-2% | Meta convertește mai bine |
| CPA | Variabil | Variabil | Variabil | **PRINCIPAL KPI** |
| ROAS | 2-10x | 1.5-6x | 1-4x | Meta ROAS mai stabil |
| LTV/CAC | >3:1 ideal | >3:1 ideal | >3:1 ideal | Business KPI identic |

### 5. Template raport lunar cross-platform
```
SHEET 1: Executive Summary
Platform | Budget | Revenue | ROAS | Orders | CPA | CAC

SHEET 2: Meta Details
Campaign | AdSet | Budget | Impressions | CTR | CPC | Conv | CPA | ROAS

SHEET 3: TikTok Details
(identic format cu Meta)

SHEET 4: GA4 Cross-Reference
Source (UTM) | Sessions | Bounce Rate | Conversions | Revenue | CPA (GA4)

SHEET 5: Insights & Decisions
- Winner platform luna asta: ___
- Creative câștigător per platformă: ___
- Audiența cu cel mai mic CPA per platformă: ___
- Decizie buget luna viitoare: ___
```

### 6. Budget allocation decision framework
**Meta ROAS > TikTok ROAS cu >30%**:
→ 60% Meta / 30% TikTok / 10% Testing

**TikTok CPA < Meta CPA cu >20%**:
→ 40% Meta / 50% TikTok / 10% Testing

**Performanță similară ambele**:
→ 45% Meta / 45% TikTok / 10% Testing

**Formula avansată (Marginal ROAS)**:
- Crește bugetul pe platforma unde ROAS-ul marginal (al ultimului euro cheltuit) e cel mai mare
- Diminishing returns: la un punct, ROAS-ul marginal scade — atunci redistribui

### 7. Factori non-ROAS de luat în calcul
- **Scalabilitate**: Meta audiențe mai mari → mai ușor de scalat
- **Brand building**: TikTok awareness mai ieftin (CPM $5-10 vs Meta $8-15)
- **Nișa ta**: audiența predominant pe care platformă?
- **Sezonalitate**: Q4 Meta +30-50% CPM | TikTok +20-30%
- **Creative fatigue speed**: TikTok obosește mai rapid → mai multă producție necesară
- **Attribution accuracy**: Meta pixel mai matur decât TikTok pixel

### 8. Cross-platform creative testing
- Testează ACELAȘI creative pe ambele platforme simultan
- Comparație echitabilă: identic creative + identic buget + identic timing
- Rezultat: înțelegi pe care platformă performează mai bine fiecare tip de creative
- UGC tinde să performeze mai bine pe TikTok; polished content mai bine pe Meta

## Verificare
- [ ] UTM parameters configurate pe TOATE campaniile active
- [ ] GA4 Goals identice create și funcționale (test conversion verificat)
- [ ] Attribution windows uniformizate (7-day click / 1-day view) pe toate platformele
- [ ] Raport lunar template creat în Google Sheets
- [ ] Decizia alocare buget documentată și revizuită lunar
- [ ] Cross-platform creative test executat cel puțin o dată
- [ ] GA4 vs platform discrepanță verificată și documentată
- [ ] Looker Studio dashboard configurat (opțional dar recomandat)

## Instrumente
- Google Analytics 4 — hub central cross-platform
- Google Tag Manager — UTM + event tracking
- Google Looker Studio — dashboard vizual GA4 + Ads
- Northbeam / Triple Whale — multi-touch attribution (buget >€5K/lună)
- Google Sheets — raport manual lunar

## Note
- "Platform truth" vs "Real truth": discrepanță 20-50% e normală — GA4 = ground truth
- Testează același creative cross-platform pentru comparație corectă
- TikTok performează mai bine pentru produse noi/virale; Meta pentru awareness construit
- CPM benchmarks: Meta $8-15 | TikTok $5-10 | Snapchat $3-7
