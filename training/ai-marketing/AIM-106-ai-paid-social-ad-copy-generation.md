---
type: procedure
created: 2026-03-20
status: active
slug: aim-106-ai-paid-social-ad-copy-generation
tags: [procedure, nexus]
---

# AI Paid Social Ad Copy Generation (Meta + TikTok + Google) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Generarea sistematică de copy pentru reclame plătite pe Meta (Facebook/Instagram), TikTok Ads și Google Ads folosind AI, cu focus pe variante de A/B testing, hooks psihologice, mesaje per segment de audiență și compliance per platformă. Include prompt templates per framework persuasiv, matrice de testare, automation pipeline și ROI tracking.

---

## 1. Problema

Ad copy de calitate slabă este cauza #1 de ROAS slab — nu targeting-ul, nu bugetul. Diferența între un hook bun și unul mediocru = 2-5x CTR. Generarea manuală a 20+ variante pentru A/B testing durează 3-5 ore și este limitată creativ de biasul individual. AI poate genera 50-100 variante în 30-60 minute, acoperind unghiuri psihologice pe care un copywriter singur nu le-ar fi explorat. Dar AI fără framework produce copy generic cu CTR sub benchmark. Această procedură îmbină psihologia persuasiunii (Cialdini, Ogilvy, Schwartz) cu eficiența generativă AI.

Situații acoperite:
- Lansarea unei campanii noi (cold audience TOF)
- Retargeting cu mesaje personalizate per funnel stage (MOF/BOF)
- Testarea sistematică de hooks, angles și CTA-uri
- Adaptarea copy pentru audiență nouă sau ofertă nouă
- Scale testing (20-50 variante per ad set)
- Client delivery pentru agenție de performance marketing

---

## 2. Procedura

### Pas 1: Brief — Completează Campaign Copy Brief (template obligatoriu)

```markdown
# Campaign Copy Brief — [Campanie]

## Produs / Ofertă
- Produs/serviciu: [ce vinzi]
- Preț: [preț normal + preț ofertă dacă există]
- Ofertă specială: [discount, bonus, free trial, free shipping]
- USP-uri (max 3): 1. [USP1] 2. [USP2] 3. [USP3]
- Diferențiator #1 vs. competitor principal: [ce faci diferit]

## Campanie
- Obiectiv: [Purchase / Lead / Traffic / Awareness]
- Buget zilnic: [€/zi] — influențează agresivitatea CTA
- Platforme: [Meta / TikTok / Google / LinkedIn]
- Formate: [Single image / Carousel / Video / Story / Collection]
- Funnel stage: [TOF cold / MOF retarget / BOF convert]

## Audiență
- Persona: [demographics + psychographics]
- Pain point #1: [cea mai mare frustrare legată de problemă]
- Dorința #1: [cel mai dorit rezultat/transformare]
- Obiecție #1: [cea mai mare barieră de cumpărare]
- Nivel de awareness (Schwartz): [Unaware / Problem-aware / Solution-aware / Product-aware / Most aware]
- Triggere emoționale: [frică de a pierde / dorință de status / comoditate / urgență / FOMO]

## Brand Voice
- Ton: [direct / empatic / autoritar / jucăuș / premium]
- Referință: [un brand al cărui ton ad-urilor îl admiri]
```

**Nivel de awareness (Eugene Schwartz)** determină fundamentul copy-ului:
- **Unaware**: Nu știu că au problema → educă, nu vinde
- **Problem-aware**: Știu problema, nu soluția → validează durerea, prezintă categorie
- **Solution-aware**: Știu soluția, nu produsul tău → diferențiază
- **Product-aware**: Știu produsul, nu sunt convinși → social proof, obiecții, ofertă
- **Most aware**: Sunt convinși, au nevoie de trigger → urgency, deal, reminder

### Pas 2: Hook Generation — 15-20 hooks organizate pe categorii psihologice

Hook-ul (prima linie text sau primele 2-3 secunde video) determină 80% din performanța ad-ului.

Prompt pentru **ChatGPT GPT-4o** (persuasion-optimized):
```
Ești un direct response copywriter cu 15 ani experiență.
Produs: [din brief]
Audiența: [din brief]
Pain point #1: [din brief]
Dorința #1: [din brief]
Awareness level: [din brief]

Generează 20 hooks pentru ad copy, organizate pe 5 categorii psihologice:

CATEGORY 1 — PAIN AGITATION (4 hooks):
Atacă direct durerea. Fii specific, nu generic.
Pattern: „[Problemă specifică] face [consecință concretă]"
Ex: „Your $200/mo gym membership is wasting your time if you're still doing these 3 exercises"

CATEGORY 2 — CURIOSITY GAP (4 hooks):
Creează un gap informațional pe care cititorul TREBUIE să-l închidă.
Pattern: „[Fapt surprinzător] și iată de ce contează"
Ex: „I stopped using [alternativa populară] 6 months ago. Here's what happened."

CATEGORY 3 — SOCIAL PROOF (4 hooks):
Folosește autoritatea socială sau numere.
Pattern: „[Număr] [persoane ca audiența] au descoperit că [beneficiu]"
Ex: „12,000 marketers switched to [produs] last quarter. The reason? [beneficiu]"

CATEGORY 4 — CONTRARIAN / PROVOCATIVE (4 hooks):
Challenge o credință populară sau un status quo.
Pattern: „Stop doing [ce face majoritatea]. Here's why."
Ex: „Stop spending 4 hours on content that gets 12 likes. There's a better way."

CATEGORY 5 — RESULT / TRANSFORMATION (4 hooks):
Arată rezultatul final, nu procesul.
Pattern: „[Rezultat concret] în [Timeframe] fără [Obiecție/sacrificiu]"
Ex: „I grew from 0 to 10K followers in 90 days without posting daily"

REGULI: Max 15 cuvinte per hook. Specifice, nu generice. Zero superlative goale.
Fiecare hook trebuie să funcționeze standalone ca primă linie de ad.
```

### Pas 3: Body Copy — Generează copy principal pe framework-uri de persuasiune

Pentru fiecare din top 5 hooks selectate, generează 2-3 variante de body copy:

**Framework PAS (Problem → Agitate → Solution):**
```
Hook ales: „[hook din Pasul 2]"
Framework: PAS
Produs: [din brief]
Audiența: [din brief]
Lungime: [50-120 cuvinte Meta / 30-50 cuvinte TikTok]

PROBLEM: Descrie problema specific, cu empatie (1-2 propoziții)
AGITATE: Amplifică impactul — ce se întâmplă dacă nu rezolvă (1-2 propoziții)
SOLUTION: Prezintă produsul ca soluția naturală + 1 USP + social proof (2-3 propoziții)
CTA: [din brief]

Ton: [din brief]. Scrie la persoana a 2-a. Fii conversațional, nu academic.
```

**Framework BAB (Before → After → Bridge):**
```
Hook ales: „[hook]"
Framework: BAB

BEFORE: Descrie viața audienței FĂRĂ produsul tău (1-2 propoziții, specific)
AFTER: Descrie viața cu produsul — transformarea concretă (1-2 propoziții)
BRIDGE: Cum ajungi din Before în After — produsul tău (2-3 propoziții + USP)
CTA: [din brief]
```

**Framework AIDA (Attention → Interest → Desire → Action):**
```
Hook ales: „[hook]"
Framework: AIDA

ATTENTION: Hook-ul (deja selectat)
INTEREST: Fapt sau insight care menține atenția (1-2 propoziții)
DESIRE: Beneficii concrete + social proof + urgency (2-3 propoziții)
ACTION: CTA clar, specific, low-friction (1 propoziție)
```

**Framework 4Ps (Promise → Picture → Proof → Push):**
```
Hook ales: „[hook]"
Framework: 4Ps (ideal pentru high-ticket sau B2B)

PROMISE: Ce vei obține concret (1 propoziție)
PICTURE: Vizualizarea rezultatului — „Imaginează-te..." (1-2 propoziții)
PROOF: Social proof, statistică, testimonial (1-2 propoziții)
PUSH: CTA + urgency sau scarcity (1 propoziție)
```

### Pas 4: CTA Variants — Generează și testează 5 tipuri de CTA

```
Generează 5 variante de CTA pentru ad-ul nostru:

1. DIRECT: „Cumpără acum" / „Înscrie-te azi" / „Începe free trial"
2. LOW-COMMITMENT: „Descoperă cum" / „Află mai mult" / „Vezi demo"
3. URGENCY: „Oferta expiră [data]" / „Doar [X] locuri rămase"
4. SOCIAL: „Alătură-te celor [X] care deja [beneficiu]"
5. BENEFIT-LED: „Obține [rezultat] în [timp]" / „Începe să [beneficiu] azi"

Adaptează la: obiectiv [conversii/lead/traffic], awareness level [din brief],
preț [low-ticket = direct CTA, high-ticket = low-commitment CTA].
```

### Pas 5: Platform Adaptation — Copy specificuri per platformă

**META ADS (Facebook/Instagram):**
```markdown
## Character Limits & Best Practices:
- Primary Text: 125 caractere VIZIBILE (restul „See more") — HOOK must fit here
- Headline: 27 caractere (sub imagine)
- Description: 27 caractere
- Link description: 30 caractere

## Ad Policies Red Flags:
- ❌ Health claims absolute: „vindecă", „garantat"
- ❌ Before/After images implying unrealistic results
- ❌ Personal attributes: „Ești supraponderal?" (nu poți presupune)
- ❌ Profanity sau content sexual/violent
- ❌ Clickbait exagerat
- ✅ Ok: „Mulți oameni se confruntă cu [X]" (depersonalizat)
- ✅ Ok: „Rezultate tipice includ [X]" (disclaimer)
```

**TIKTOK ADS:**
```markdown
## Specificuri:
- Caption: max 150 caractere — ULTRA concis
- Ad text: informal, nativ (nu trebuie să „sune ca reclamă")
- Language: trend language, slang acceptat
- Hook vizual: primele 1-2 secunde = tot ce contează
- Sound: trending sound = boost organic distribution
- Format: vertical 9:16, full-screen
- UGC style performează mai bine decât polished ads

## Copy adaptation:
- Meta copy formal → TikTok: rescrie ca și cum vorbești cu un prieten
- Eliminare: jargon, corporate speak, „click here"
- Adaugă: emojis strategic (1-3), casual language
```

**GOOGLE ADS (Search + Performance Max):**
```markdown
## RSA (Responsive Search Ads):
- Headlines: 15 headlines × max 30 caractere fiecare
- Descriptions: 4 descriptions × max 90 caractere fiecare
- Pin headlines 1-3 pentru control
- Include keyword în minim 5 headlines
- Include CTA în minim 3 headlines
- Variații: beneficiu, feature, preț, urgency, social proof, question

## Prompt pentru Google RSA:
Generează 15 headlines (max 30 char) și 4 descriptions (max 90 char) pentru:
Keyword: [keyword]
Produs: [din brief]
USP: [din brief]
Variații: 3 cu keyword exact, 3 cu beneficiu, 3 cu CTA, 2 cu preț/ofertă,
2 cu social proof, 2 cu urgency.
```

### Pas 6: Testing Matrix — Construiește matrice A/B sistematică

```
TESTING MATRIX:

| Ad # | Hook (H) | Body Framework | CTA | Platformă | Segment |
|------|----------|---------------|-----|-----------|---------|
| Ad 1 | H1 (Pain) | PAS | Direct | Meta | Cold |
| Ad 2 | H2 (Curiosity) | PAS | Low-commit | Meta | Cold |
| Ad 3 | H3 (Social Proof) | BAB | Social | Meta | Cold |
| Ad 4 | H1 (Pain) | AIDA | Urgency | Meta | Retarget |
| Ad 5 | H4 (Contrarian) | 4Ps | Benefit | Meta | Retarget |
| Ad 6 | H5 (Result) | BAB | Direct | TikTok | Cold |
| Ad 7 | H2 (Curiosity) | PAS | Low-commit | TikTok | Cold |
| Ad 8 | H1 (Pain) | RSA Headlines | Direct | Google | Search |
```

**Testing protocol:**
1. Lansează 6-10 variante simultan per ad set
2. Budget egal per variantă ($5-10/zi per ad minim)
3. Statistică minimă: 1,000 impressions per variantă înainte de decizie
4. Kill loser la 48-72 ore (CTR sub 50% din best performer)
5. Scale winner: crește budget 20-30% per zi (nu dubla overnight)
6. Documentează: hook câștigător, framework câștigător, CTA câștigător → aplică pe campanii viitoare

### Pas 7: Compliance — Pre-flight check obligatoriu

Prompt pentru **Claude Sonnet** (compliance analysis):
```
Review aceste [X] ad copies pentru conformitate cu:

1. META ADVERTISING POLICIES (ads.meta.com/policies):
   - Health claims: nicio promisiune de vindecare sau rezultat garantat
   - Personal attributes: nu presupune caracteristici personale
   - Discriminare: nu exclude pe bază de rasă, religie, sex, etc.
   - Financial claims: nicio promisiune de câștig garantat
   - Deceptive content: nu exagerezi, nu induci în eroare

2. TIKTOK COMMUNITY GUIDELINES & AD POLICIES:
   - Autenticitate: nu fake engagement, nu misleading claims
   - Safety: nu promovezi produse periculoase
   - Intellectual property: nu folosești muzică/content fără drept

3. GOOGLE ADS POLICIES:
   - Trademark: nu folosești marca competitorului în headline fără aprobare
   - Landing page alignment: ad copy = promisiunea de pe landing page
   - Prohibited content: gambling, adult, pharma restrictions per piață

4. LEGISLAȚIE LOCALĂ (dacă relevant):
   - ANPC (România): prețuri complete cu TVA, ofertă limitată cu dată end
   - GDPR: nu colecta PII fără consimțământ
   - Consumer protection: disclaimers pe rezultate tipice

Marchează cu 🔴 ce trebuie modificat obligatoriu (risc de respingere ad).
Marchează cu 🟡 ce trebuie revizuit (risc moderat).
Marchează cu 🟢 ce e compliant.
```

### Pas 8: Automate — Pipeline generare ad copy cu Make.com

**Make.com Ad Copy Pipeline:**
```
Scenario: Ad Copy Generation Batch

Trigger: Google Sheet new row (Campaign Copy Brief completed)

→ Module 1: HTTP → ChatGPT API → Generate 20 hooks (Pasul 2)
→ Module 2: Iterator → Top 5 hooks
  → Module 3: HTTP → ChatGPT API → Generate body copy per hook + 3 frameworks
  → Module 4: HTTP → ChatGPT API → Generate 5 CTA variants
→ Module 5: Aggregator → Compile testing matrix
→ Module 6: HTTP → Claude API → Compliance check (Pasul 7)
→ Module 7: Google Sheets → Write approved variants in „Ad Copy Matrix"
→ Module 8: Slack → Notify: „[X] ad variants ready for review. [Y] flagged compliance."

Estimated time: 5-10 minutes automated vs. 3-5 hours manual
API cost per campaign: $1-3
```

### Pas 9: Performance Loop — Post-launch optimization cu AI

La 72 ore post-lansare:

Prompt pentru **Claude Sonnet**:
```
Analizează performanța acestor [X] ad variants:

| Ad # | Hook type | Framework | CTR | CPC | ROAS | Conversions |
[paste data]

Benchmark industrie: CTR [X]%, CPC [€X], ROAS [X].

Identifică:
1. TOP 3 performers — ce au în comun? (hook type, framework, CTA, lungime)
2. BOTTOM 3 — ce au în comun? De ce au eșuat?
3. Ce hook category a câștigat overall?
4. Ce framework (PAS/BAB/AIDA/4Ps) a generat cel mai bun ROAS?
5. Ce CTA type a avut cel mai bun click rate?
6. Recomandări: 3 noi variante de testat bazate pe patternurile winners
7. Estimare: dacă realocam bugetul pe top 3, ce ROAS estimăm?
```

### Pas 10: ROI Calculation — Framework de calcul rentabilitate

```markdown
# Ad Copy AI ROI Calculator

## Cost traditional (fără AI):
- Copywriter freelance: $50-150 per ad copy
- 20 variante per campanie: $1,000-3,000
- Timp intern: 5-8 ore × $50/h = $250-400
- Total per campanie: $1,250-3,400

## Cost cu AI pipeline:
- API costs: $1-3 per campanie
- Tool subscriptions (pro-rated): $5-10 per campanie
- Timp intern (review + launch): 1-2 ore × $50/h = $50-100
- Total per campanie: $56-113

## Savings: $1,194-3,287 per campanie (95-97% reducere)
## La 4 campanii/lună: $4,776-13,148/lună savings

## Performance improvement:
- CTR improvement cu A/B testing sistematic: +15-40%
- ROAS improvement cu winning hooks scaled: +20-50%
- Speed to market: 3-5 ore → 30-60 minute (5-10x faster)
```

---

## 3. Cortex Logging

```json
{
  "text": "Paid Social Ad Copy generat pentru [produs/campanie]. Platforme: [Meta/TikTok/Google]. Hooks generate: [X]. Variante copy: [Y]. Testing matrix: [Z] combinații. Compliance: [passed/flagged]. Top performer post-launch: [hook type + framework].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-106",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["paid-social", "ad-copy", "meta-ads", "tiktok-ads", "google-ads", "ab-testing", "hooks", "persuasion", "roas"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + orice campanie paid social nouă sau refresh

### WHEN
La fiecare set nou de ad creatives sau A/B test

### HOW (violation detection)
- Brief incomplet (lipsește awareness level sau pain point) → violation
- Mai puțin de 5 hooks categorii diferite testate → violation
- Compliance check sărit → violation critică
- Aceleași texte pe Meta/TikTok/Google fără adaptare → violation
- Zero A/B testing (single copy per ad set) → violation
- Performance analysis post-launch sărită → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-109 → video scripts (pentru video ad creatives)
- AIM-107 → email (continuitate funnel paid → email nurture)
- AIM-102 → multi-model routing (ChatGPT primar pentru persuasion)

### VERIFY
- [ ] Campaign Copy Brief complet (awareness level identificat)?
- [ ] Minim 15 hooks generate pe 5 categorii psihologice?
- [ ] Body copy pe minim 2 framework-uri diferite (PAS, BAB, AIDA, 4Ps)?
- [ ] CTA-uri variate (minim 3 tipuri)?
- [ ] Platform adaptation aplicată per specificuri?
- [ ] Testing matrix construită cu minim 6 variante?
- [ ] Compliance check completat (Meta + TikTok + Google policies)?
- [ ] Performance analysis planificată la 72 ore post-launch?

**VK-uri:**
1. `✅ [PROC] AIM-106 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Paid Social Ad Copy Generation" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Hook generation | ChatGPT GPT-4o | Superior la persuasion, creative hooks |
| Body copy frameworks | ChatGPT GPT-4o | Ton conversațional, AIDA/PAS fluent |
| Compliance review | Claude Sonnet | Analiză policy riguroasă, nu omite |
| Performance analysis | Claude Sonnet | Structured analysis, recomandări |
| Google RSA headlines | ChatGPT GPT-4o | Concizie, keyword integration |
| Bulk variants volume | DeepSeek | Cost-efficient pentru 50+ variante |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT GPT-4o | Generare ad copy persuasiv, hooks |
| Claude Sonnet | Compliance review, performance analysis |
| Meta Ads Manager | Implementare și A/B test Meta |
| TikTok Ads Manager | Implementare campanie TikTok |
| Google Ads | RSA, Performance Max |
| Make.com ($9/mo) / n8n | Pipeline automation |
| Google Sheets | Testing matrix, performance tracking |

---

## 6. Instrumente

- ChatGPT Plus ($20/mo) — copy persuasiv, hooks, frameworks
- Claude Pro ($20/mo) — compliance, analysis, QA
- AdCreative.ai ($29/mo) — AI ad creative generation (complementar)
- Foreplay.co ($49/mo) — competitor ad library, creative inspiration
- Meta Ad Library (free) — research ads competitori
- Smartly.io (enterprise) — automation și optimization la scară
- VidTao (free) — YouTube ad spy, video ad inspiration

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Timp generare 20+ variante | ≤ 45 min |
| Variante per campanie A/B | ≥ 6 |
| Hook categories testate | ≥ 4 din 5 |
| CTR improvement vs. benchmark | +15-40% |
| Ads respinse post-compliance check | 0 |
| Cost AI per campanie | ≤ $3 |
| ROAS improvement cu systematic testing | +20-50% |
| Time to launch (brief → live) | ≤ 2 ore |
