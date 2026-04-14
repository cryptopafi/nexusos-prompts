---
type: procedure
created: 2026-03-20
status: active
slug: aim-105-ai-social-media-bulk-post-creation
tags: [procedure, nexus]
---

# AI Social Media Bulk Post Creation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Producerea în volum a postărilor pentru social media pe multiple platforme folosind AI, păstrând brand consistency și adaptând conținutul per canal. Include prompt templates per platformă, workflow automation pentru generare + scheduling, batch QA process, și framework de repurposing sistematic.

---

## 1. Problema

Echipele de social media petrec 60-70% din timp pe crearea de conținut, lăsând insuficient timp pentru strategie, community management și analiză. O postare de calitate manuală durează 20-40 minute (research + scriere + hashtags + vizual). Cu un pipeline AI structurat, se poate genera un batch de 30-50 postări în 2-3 ore (4-6 minute/post), cu calitate comparabilă sau superioară. Fără proces, AI produce conținut repetitiv, tone-deaf, sau format greșit per platformă. Această procedură transformă producția bulk în proces predictibil și scalabil.

Situații acoperite:
- Popularea calendarului editorial lunar (30 zile, 3-5 platforme)
- Campanie tematică pe durată limitată (lansare produs, sezon, event)
- Repurposing al conținutului existent (blog, video, podcast) în social posts
- Crearea de conținut pentru multiple conturi client simultan (agenție)
- Evergreen content library pentru re-utilizare ciclică

---

## 2. Procedura

### Pas 1: Strategy — Stabilește cadrul editorial cu piloni, formate și frecvențe

Completează acest Content Strategy Grid înainte de producție:

```markdown
# Social Media Content Strategy — [Brand] — [Luna/Perioada]

## Piloni de conținut (4-6 categorii)
| # | Pilon | % din total | Exemple |
|---|-------|-------------|---------|
| 1 | Educație / How-to | 30% | Tips, tutoriale, myth-busting |
| 2 | Produs / Serviciu | 20% | Features, use cases, demo |
| 3 | Social proof | 15% | Testimoniale, results, UGC |
| 4 | Behind-the-scenes | 15% | Echipă, proces, cultură |
| 5 | Engagement / Comunitate | 10% | Polls, Q&A, this-or-that |
| 6 | Trending / Actualitate | 10% | Memes, news hijacking, trends |

## Frecvența per platformă
| Platformă | Posturi/săpt. | Formate prioritare | Best posting time |
|-----------|--------------|-------------------|-------------------|
| Instagram | 5-7 | Carousel, Reel, Story | 11-13, 18-20 |
| LinkedIn | 3-5 | Text post, Document, Poll | 8-10, 12-13 |
| Twitter/X | 7-14 | Text, Thread, Quote | 8-9, 12-13, 17-18 |
| TikTok | 3-5 | Short video, Duet | 19-22 |
| Facebook | 3-5 | Link, Image, Video | 13-15 |

## Campanii/Events luna aceasta
- [Data] — [Event/Campanie] — [nr. posturi dedicate]
- [Data] — [Zi tematică — ex: World X Day] — [nr. posturi]

## Brand Voice Reference
- Ton: [3 adjective]
- DO: [3 exemple de fraze on-brand]
- DON'T: [3 exemple de fraze off-brand]
```

### Pas 2: Input — Colectează materiale sursă structurate pentru generare

Adună toate materialele care alimentează generarea (cu cât mai mult input specific, cu atât mai puțin output generic):

- **Blog posts** publicate sau în pregătire → extrage key insights per articol
- **Statistici și date recente** din industrie → cifre concrete pentru credibilitate
- **Quote-uri și testimoniale** → social proof formatted
- **Produse/servicii** de promovat → USP-uri, prețuri, diferențiatori
- **Competitor posts** performante → nu pentru copiere, ci pentru inspirație de format
- **UGC (User Generated Content)** → screenshots, reviews, mențiuni
- **Events și date importante** → calendar editorial
- **Conținut evergreen** reutilizabil → „pillar posts" care funcționează oricând
- **Performance data** luna anterioară → ce tip de post a performat (repeat winners)

Stochează într-un document partajat (Notion page sau Google Doc) — devine „Content Source Bank".

### Pas 3: Generate — Batch generation per pilon cu prompts specializate

**Regula cheie**: Generează câte un batch per pilon de conținut, nu toate postările simultan. Calitatea scade dramatic dacă ceri „30 de postări pe orice subiect".

**TEMPLATE — Instagram Posts (ChatGPT GPT-4o):**
```
Ești social media strategist pentru [brand] — [descriere 1 propoziție].
Audiența: [persona — demographics + interests].
Ton: [3 adjective din strategy grid].

Generează 8 postări Instagram pe pilonul „[PILON]":

Cerințe per postare:
- HOOK: prima linie puternică (apare înainte de „...mai mult")
- BODY: 80-150 cuvinte, valoare reală (nu fluff)
- CTA: specific (save this / tag a friend / comment below / click link in bio)
- HASHTAGS: 8-12 (3 niche + 3 medii + 3-5 branded/context)
- FORMAT sugerat: [Carousel / Reel script / Single image + caption / Story sequence]

Variază structura: 2 educational tips, 2 storytelling, 2 data/stats-based,
1 question/poll, 1 behind-the-scenes.

NU repeta aceeași structură de hook. NU începe mai mult de 1 post cu întrebare.
NU folosi: „în lumea de azi", „hai să fim sinceri", „nu e un secret că".
```

**TEMPLATE — LinkedIn Posts (Claude Sonnet):**
```
Ești content strategist B2B pentru [brand]. Scrii pe LinkedIn pentru [ICP].
Tonul: profesional dar accesibil, autoritar fără a fi academic.

Generează 5 postări LinkedIn pe pilonul „[PILON]":

Cerințe per postare:
- HOOK: prima linie care oprește scroll-ul (controversată, surprinzătoare, sau very specific)
- FORMAT: fiecare postare are white space generos (1-2 propoziții per „paragraf")
- LUNGIME: 150-250 cuvinte (sweet spot LinkedIn engagement)
- STRUCTURE VARIETY: 1 story format, 1 listicle (3-5 puncte), 1 hot take/opinie,
  1 data-driven insight, 1 lesson learned
- CTA: engagement-focused (Agree or disagree? What's your experience? Repost if useful.)
- HASHTAGS: max 3-5, relevante (nu #success #motivation #mindset)
- NO: emoji spam, humble bragging, „I'm humbled to announce"

Fiecare post trebuie să ofere 1 insight concret pe care cititorul îl poate aplica azi.
```

**TEMPLATE — Twitter/X Threads (ChatGPT GPT-4o):**
```
Creează 3 thread-uri Twitter pe tema „[PILON]" pentru [brand].

Fiecare thread: 5-8 tweets.
- Tweet 1: HOOK + promisiune de valoare (ce va învăța cititorul)
- Tweets 2-N: câte 1 idee per tweet, standalone dar legat de fir narativ
- Tweet final: CTA (follow for more / bookmark this / retweet if useful) + link opțional

Cerințe:
- Max 250 caractere per tweet (lasa spațiu pentru engagement)
- Include 1 tweet cu o statistică sau cifră concretă
- Include 1 tweet cu „Most people think X. But actually Y."
- Ton: [casual expert / authority / provocateur]
```

**TEMPLATE — TikTok Scripts (ChatGPT GPT-4o):**
```
Generează 5 scripturi TikTok (15-60 secunde) pe pilonul „[PILON]" pentru [brand].

Fiecare script include:
- HOOK (primele 2-3 secunde): text on-screen + ce spui
- BODY: talking head sau voiceover, 1 singur insight per video
- TWIST/PAYOFF: reveal, surpriză sau concluzie puternică
- CTA: „Follow for more" / „Comment your answer" / „Save this"
- SOUND SUGGESTION: trending sound sau original audio
- ON-SCREEN TEXT: key phrases care apar peste video

Format output:
[0-3s] HOOK: [ce se vede] + [ce se spune]
[3-20s] BODY: [conținut principal]
[20-25s] CTA: [closing]
Sound: [sugestie]
Caption: [max 150 caractere]
```

**Bulk generation cu DeepSeek (cost-efficient pentru volume mari):**
Pentru volume >50 postări/sesiune, folosește DeepSeek API via Make.com:
- Input: CSV cu [pilon, platformă, topic, format] per rând
- DeepSeek generează draft-ul per rând
- Output: CSV completat cu postare generată
- Cost: ~$0.01-0.03 per postare vs. $0.05-0.15 cu GPT-4o

### Pas 4: Platform Adapt — Verificare specificuri platformă (checklist)

Parcurge fiecare post generat și verifică:

**Instagram:**
- [ ] Hook-ul captează atenția în primele 125 caractere (înainte de „...mai mult")
- [ ] Hashtags: 8-12, mix niche + medium volume (nu #love #instagood)
- [ ] CTA clar (save/share/comment/click link in bio)
- [ ] Carousel: fiecare slide standalone dar fir narativ coerent
- [ ] Reel: script ≤60s, subtitles necesare, vertical 9:16

**LinkedIn:**
- [ ] Ton profesional (nu casual TikTok copy-paste)
- [ ] White space generos (nu wall of text)
- [ ] Hashtags: max 3-5, profesionale
- [ ] NO: emoji excess, hashtag stuffing, humble brag
- [ ] Engagement question naturală (nu forțată)

**Twitter/X:**
- [ ] Max 280 caractere per tweet individual
- [ ] Thread: tweet 1 = hook, tweet final = CTA
- [ ] Fără hashtag spam (max 1-2 per tweet)

**TikTok:**
- [ ] Caption: max 150 caractere
- [ ] Script: hook în primele 2 secunde
- [ ] Trending sound sugerat (verifică TikTok Creative Center)
- [ ] Vertical format (9:16)

### Pas 5: QA Batch — Review calitate pe întreg batch-ul (quality gate)

**AI QA Pass** — Prompt pentru **Claude Sonnet**:
```
Review aceste [X] postări social media generate pentru [brand].
Brand voice: [3 adjective + DO/DON'T din strategy grid].

Marchează cu 🔴 REJECT postările care au:
1. AI-ISMS: fraze generice tipice („în lumea de azi", „hai să recunoaștem")
2. DUPLICAT: structură sau mesaj prea similar cu altă postare din batch
3. OFF-BRAND: ton sau mesaj care nu se aliniază cu brand voice
4. CLAIM NEVERIFICABIL: statistici sau afirmații fără sursă
5. WEAK HOOK: prima linie nu oprește scroll-ul

Marchează cu 🟡 EDIT postările care sunt bune dar necesită ajustare minoră.
Marchează cu 🟢 APPROVED postările gata de publicare.

Target: 70-80% approved, 15-20% edit, max 10% reject.
```

**Review uman obligatoriu** (20-30 minute per batch de 30 postări):
- Citește primele 5 postări: sună ca brandul tău sau ca orice alt brand?
- Verifică diversitatea: nu ai 5 postări consecutive care încep cu întrebare?
- Verifică claim-uri: orice cifră → sursă
- Verifică CTA-uri: variezi sau repeți „comment below" la fiecare post?
- Finalizează: aprobă, editează, sau regenerează

### Pas 6: Visual — Asociază template-uri vizuale (batch design)

**Workflow visual efficient:**

1. **Creează 5-8 templates Canva Pro** ($13/mo) per brand, aliniiate cu brand kit:
   - Quote card template (text pe background brand colors)
   - Carousel template (cover + content slides + CTA slide)
   - Stats/data template (cifră mare + context)
   - Tips list template (3-5 tips formatted)
   - Behind-the-scenes template (photo frame + text overlay)
   - Promo template (produs + ofertă + CTA)

2. **Batch design**: duplică templates, schimbă textul per postare — 2-3 minute per vizual.

3. **AI image generation** pentru postări care necesită imagini unice:
   - **Midjourney** ($30/mo) pentru creative de calitate: `/imagine [brand aesthetic] + [subiect] --ar 1:1 --style raw`
   - **DALL-E 3** (via ChatGPT) pentru iterații rapide
   - **Canva Magic Design** pentru sugestii automate din text

4. **Batch export**: Canva → Download All → formatul corect per platformă (1080x1080 IG, 1200x628 LinkedIn, etc.)

### Pas 7: Schedule — Programează cu optimal timing

**Tool recomandat per buget:**
- **Buffer** ($6/canal/mo) — cel mai simplu, AI caption assistant inclus
- **Publer** ($12/mo) — cel mai cost-efficient multi-platform
- **Hootsuite** ($99/mo) — cel mai complet (analytics + scheduling + team)
- **Later** ($25/mo) — visual-first, ideal Instagram-first brands

**Scheduling checklist:**
- [ ] Ora de postare: bazată pe analytics (nu pe intuiție) — verifică Instagram Insights / LinkedIn Analytics
- [ ] Distribuție echilibrată pe piloni (nu 3 posturi promo consecutive)
- [ ] Spațiere: minim 4-6 ore între posturi pe aceeași platformă
- [ ] Weekend: reduce frecvența sau programează evergreen content
- [ ] Preview vizual: verifică cum arată fiecare post pe platformă (thumbnail, crop, text overflow)
- [ ] Campanii speciale: postări de lansare/event programate cu 48h înainte

### Pas 8: Automate — Workflow end-to-end cu Make.com

**Make.com Social Content Pipeline ($9/mo):**
```
Scenario: Monthly Social Media Bulk Generation

Trigger: Schedule (1st of month, 8:00 AM)

→ Module 1: Google Sheets → Read „Content Source Bank" (topics, piloni, materials)
→ Module 2: Iterator → per pilon
  → Module 3: HTTP → ChatGPT API (generate Instagram posts for pilon)
  → Module 4: HTTP → ChatGPT API (generate LinkedIn posts for pilon)
  → Module 5: HTTP → DeepSeek API (generate Twitter posts for pilon — cost-efficient)
→ Module 6: Aggregator → collect all posts
→ Module 7: Google Sheets → Write all posts in „Monthly Calendar" sheet
→ Module 8: Slack → Notify: „30-day social calendar generated. Review ready."

Cost: ~$5-15 API per lună + $9 Make.com = $14-24/lună total
Manual time: 6-8 ore → 1.5-2 ore (review + scheduling only)
```

**n8n Alternative (self-hosted, free):**
Același flow cu HTTP Request nodes. Necesită VPS sau Docker local.

### Pas 9: Analyze & Iterate — Post-performance feedback loop

La finalul fiecărei luni, analizează performanța și alimentează luna următoare:

Prompt pentru **Claude Sonnet**:
```
Analizează performanța social media din [luna]:

TOP 5 POSTURI (highest engagement):
[Post 1 — text, platformă, engagement metrics]
[Post 2 — ...]
...

BOTTOM 5 POSTURI (lowest engagement):
[Post 1 — text, platformă, metrics]
...

Identifică:
1. Ce pattern au top performers (format, hook type, pilon, ton, timing)?
2. Ce pattern au bottom performers?
3. Ce pilon de conținut a performat cel mai bine overall?
4. Ce tip de hook a generat cel mai mult engagement?
5. Ce platformă a crescut cel mai mult?
6. 5 recomandări concrete pentru luna viitoare (ce să faci mai mult/mai puțin)
```

Actualizează Strategy Grid-ul (Pasul 1) cu insights-urile. Repetă ciclul.

### Pas 10: Agency Scale — Multi-client batch production

Pentru agenții cu 3-10 clienți:

1. **Template library per industrie**: creează seturi de prompts per vertical (SaaS, e-com, local business, personal brand) — reutilizabile cross-client cu brand voice swap
2. **Client onboarding kit**: Brand Voice Doc + Strategy Grid + Content Source Bank per client (setup 2-3 ore, apoi reutilizabil lunar)
3. **Batch workflow**: generează pentru toți clienții în aceeași sesiune Make.com (paralel, nu secvențial)
4. **Pricing model**: include AI tooling în retainer (costul AI: 3-5% din retainer client)
5. **ROI pentru client**: „50 postări/lună, 5 platforme, calitate consistentă, la un cost de 70% mai puțin decât un social media manager full-time ($4,000-5,000/mo)"

---

## 3. Cortex Logging

```json
{
  "text": "Social Media Bulk Creation finalizat. Platforme: [lista]. Posturi generate: [X]. Posturi aprobate post-QA: [Y] ([Z]%). Calendar acoperit: [X zile]. Tool scheduling: [Buffer/Hootsuite/etc.]. Cost AI: [$X.XX].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-105",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["social-media", "bulk-creation", "content-calendar", "scheduling", "instagram", "linkedin", "tiktok", "automation"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + producție calendar editorial lunar

### WHEN
La fiecare sesiune de producție bulk social media (minim lunar)

### HOW (violation detection)
- Generare fără Strategy Grid și piloni definiți → violation
- Toate platformele cu același text (zero adaptare) → violation
- Postări publicate fără QA batch (AI + uman) → violation critică
- Batch fără asset vizual asociat → violation
- Zero analiză post-performance la finalul lunii → violation
- Hooks repetitive (>3 posturi cu aceeași structură de hook) → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-103 → content pipeline (sursă repurposing blog → social)
- AIM-109 → video scripts (pentru Reels/TikTok)
- AIM-102 → multi-model routing (care model per volum)

### VERIFY
- [ ] Strategy Grid completat (piloni, frecvențe, formate)?
- [ ] Content Source Bank populat?
- [ ] Generare pe piloni separați (nu all-in-one)?
- [ ] Adaptare per platformă verificată (checklist)?
- [ ] QA batch: AI pass + review uman aplicat?
- [ ] Template vizuale asociate fiecărui post?
- [ ] Scheduled în tool cu timing bazat pe analytics?
- [ ] Performance analysis programat la end-of-month?

**VK-uri:**
1. `✅ [PROC] AIM-105 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Social Media Bulk Post Creation" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Instagram + TikTok posts | ChatGPT GPT-4o | Creativitate hooks, ton casual |
| LinkedIn posts | Claude Sonnet | Ton profesional, nuanță B2B |
| Twitter threads | ChatGPT GPT-4o | Concizie, punch per tweet |
| Bulk volume (>50 postări) | DeepSeek | 95% cost reduction |
| QA batch review | Claude Sonnet | Detectare AI-isms, brand voice check |
| Performance analysis | Claude Sonnet | Analiză structurată, recomandări |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT GPT-4o / Claude Sonnet | Generare text posturi |
| DeepSeek API | Bulk generation cost-efficient |
| Canva Pro ($13/mo) | Template-uri vizuale, batch design |
| Buffer ($6/canal) / Publer ($12/mo) / Hootsuite ($99/mo) | Scheduling |
| Midjourney ($30/mo) / DALL-E 3 | Imagini unice |
| Make.com ($9/mo) / n8n (free) | Workflow automation |
| Google Sheets / Notion | Content calendar, Source Bank |

---

## 6. Instrumente

- ChatGPT Plus ($20/mo) — generare posturi creative (IG, TikTok, Twitter)
- Claude Pro ($20/mo) — LinkedIn posts, QA, analiză performanță
- DeepSeek (API ~$0.01-0.03/post) — bulk volume cost-efficient
- Canva Pro ($13/mo) — templates și batch design
- Buffer ($6/canal/mo) — scheduling cu AI timing suggestions
- Publer ($12/mo) — alternativă multi-platform cost-efficient
- Make.com ($9/mo) — pipeline automation end-to-end
- Flick ($14/mo) — hashtag research și analytics
- TikTok Creative Center (free) — trending sounds și formats

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Posturi generate per sesiune (2h) | ≥ 40 |
| Rata de aprobare post-QA | ≥ 70% |
| Calendar acoperit per sesiune | ≥ 30 zile |
| Timp scheduling post-aprobare | ≤ 30 min |
| Cost AI per post (mediu) | ≤ $0.10 |
| Engagement rate Instagram | ≥ 2% |
| Engagement rate LinkedIn | ≥ 3% |
| Diversitate hooks (unice/total) | ≥ 80% |
