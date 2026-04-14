---
type: procedure
created: 2026-03-20
status: active
slug: aim-109-ai-video-script-writing
tags: [procedure, nexus]
---

# AI Video Script Writing (TikTok / YouTube / Reels) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Scrierea cu AI a scripturilor video optimizate per platformă — TikTok (15-60s), YouTube Shorts (60s), Instagram Reels (15-90s) și YouTube long-form (5-20 min) — cu focus pe hook engineering, structuri de retenție testate, AI avatar delivery, și batch production scalabilă. Include prompt templates per format, voice/avatar pipeline, și performance feedback loop.

---

## 1. Problema

Video content este #1 în organic reach pe toate platformele (TikTok, IG, YT, LinkedIn). Dar creatorii pierd cel mai mult timp pe scriptare — un script bun de 60 secunde durează 30-45 minute manual. Un script slab = retenție sub 30% = algoritmul nu distribuie = effort pierdut. AI poate genera un script de 60 secunde în 3-5 minute, dar fără metodologie produce texte robotice cu zero personalitate. Această procedură combină principii de retenție testate (MrBeast, Ali Abdaal, Hormozi) cu eficiența generativă AI, plus integrarea cu AI avatars (Synthesia, HeyGen) și AI voiceover (ElevenLabs) pentru producție fără filmare.

Situații acoperite:
- TikTok și Instagram Reels (short-form, 15-60 secunde)
- YouTube Shorts (60 secunde, vertical)
- YouTube long-form (5-20 minute, tutorial/educational/vlog)
- Scripturi pentru AI avatars (Synthesia, HeyGen) fără filmare
- AI voiceover narration (ElevenLabs) pentru explainers
- Video ad scripts (pentru Meta/TikTok/YouTube ads)
- Batch production: 10-20 scripturi per sesiune
- Client video scripts (agenție/freelancer deliverable)

---

## 2. Procedura

### Pas 1: Format — Definește specificul video cu parametri completi

Completează acest Video Script Brief:

```markdown
# Video Script Brief

## Platformă & Format
- Platformă: [TikTok / IG Reels / YT Shorts / YT Long-form / LinkedIn Video]
- Durată: [15s / 30s / 60s / 90s / 5min / 10min / 15min / 20min]
- Orientare: [Vertical 9:16 / Horizontal 16:9 / Square 1:1]
- Format delivery: [Talking head / Voiceover + B-roll / AI Avatar / Screen recording / Mixed]

## Conținut
- Subiect: [despre ce e video-ul — 1 propoziție]
- Mesaj cheie: [ce vrei ca viewerul să rețină — 1 propoziție]
- Obiectiv: [Awareness / Educational / Entertainment / Sales / Lead gen]
- CTA: [Follow / Subscribe / Comment / Click link / Save / Share / Buy]

## Audiență
- Persona: [cine vizionează — demographics + knowledge level]
- Cunoaștere subiect: [Zero / Beginner / Intermediate / Expert]
- Motivația de vizionare: [Învață ceva / Divertisment / Rezolvă o problemă]

## Brand
- Ton: [educativ / casual / autoritar / funny / inspirational / provocateur]
- Personalitate creator: [ce vibe ai — calm expert / energic / sarcastic / wholesome]
- Referință stil: [un creator care are tonul pe care îl vrei — ex: „Alex Hormozi dar mai soft"]

## Technical
- Word count target: [150 cuvinte/minut spoken → 75 cuvinte = 30s, 150 = 1min, 750 = 5min]
- Subtitles: [Da — auto-generate / Da — scripted / Nu]
- Music/Sound: [Trending sound / Original / Background music / No music]
```

### Pas 2: Hook Engineering — Construiește hooks pentru primele 1-3 secunde

**Principiu**: 65% din viewers decid în primele 2 secunde dacă rămân sau fac swipe. Hook-ul este 80% din succesul video-ului.

Prompt pentru **ChatGPT GPT-4o** (creativity-optimized):
```
Generează 10 hooks video pentru primele 2-3 secunde. Subiectul: [din brief].
Audiența: [din brief]. Tonul: [din brief].

CATEGORII DE HOOKS (2 per categorie):

1. PATTERN INTERRUPT (vizual + verbal):
   Ce se VEDE: [acțiune surprinzătoare / obiect neașteptat / jump cut dramatic]
   Ce se SPUNE: [statement scurt, max 8 cuvinte]
   Ex: [Se vede: aruncă telefonul pe masă] „Asta e cea mai mare greșeală pe care o faci."

2. BOLD STATEMENT / CONTRARIAN:
   Afirmație care contrazice ce crede majoritatea.
   Ex: „Nu mai posta pe Instagram la 9 dimineața. Iată de ce."

3. RESULT TEASER:
   Arată rezultatul ÎNAINTE de a explica procesul. Create desire.
   Ex: „Am crescut de la 0 la 10K followeri în 90 de zile cu această singură tactică."

4. DIRECT QUESTION:
   Întrebare specifică la care viewerul gândește „da, exact!" sau „ce?!"
   Ex: „De ce ai 1,000 de followeri dar doar 12 likes?"

5. „STOP DOING THIS":
   Challenge o practică comună. Creează anxietate constructivă.
   Ex: „Dacă încă faci [X], oprește-te acum. Uite ce ar trebui să faci în schimb."

REGULI HOOKS:
- Max 8 cuvinte vorbit (sub 3 secunde delivery)
- Include ON-SCREEN TEXT suggestion (ce apare ca text peste video)
- Trebuie să funcționeze ȘI fără audio (60% TikTok e fără sunet)
- Hook-ul vizual este la fel de important ca cel verbal
```

### Pas 3: Structure — Aplică structura de retenție specifică formatului

**SHORT-FORM (15-60s) — Structura IVS:**

```
[0-3s]  I = INTERRUPT (Hook vizual + verbal)
        → Oprește scroll-ul. Promite valoare.

[3-45s] V = VALUE (Insight principal)
        → 1 singură idee per video. Nu mai mult.
        → Fiecare propoziție câștigă dreptul la următoarea.
        → Schimbă unghiul camerei la fiecare 5-7 secunde (retenție vizuală).
        → Include 1 „mini-hook" la mijloc: „Dar asta nu e tot..."

[45-60s] S = SEED (CTA + next video tease)
         → CTA specific: „Save asta pentru mai târziu"
         → Opțional: „Partea 2 mâine" (creează serial)
```

**YOUTUBE LONG-FORM (5-20min) — Structura HELO:**

```
[0:00-0:30]  H = HOOK + RE-HOOK
             → Hook principal (0-10s): promise + curiosity gap
             → Re-hook (10-30s): „Și la final, o să-ți arăt [payoff neașteptat]"
             → Nu intro lungă, nu „Bună, sunt [Nume], astăzi..."

[0:30-2:00]  E = EXPLANATION (Setup)
             → Setează problema/contextul rapid
             → De ce contează ACUM (urgency)
             → „Dacă ai [problemă], acest video e pentru tine"

[2:00-15:00] L = LONG BODY (Valoare principală)
             → 3-7 puncte/secțiuni, fiecare cu:
               - Mini-hook: „Punctul #3 e cel mai important..."
               - Content: insight concret
               - Proof: exemplu, date, screenshot
               - Transition: „Și asta ne duce la..."
             → LOOP OPENS: la minutul 3, promite ceva care vine la minutul 12
               (menține retenția pe porțiunea de mijloc)
             → PATTERN BREAKS la fiecare 2-3 minute:
               unghi cameră diferit, B-roll, grafic, meme, zooming

[15:00-end]  O = OUTRO
             → Recapitulare 3 takeaways (30s)
             → CTA specific: subscribe + next video suggested
             → „Dacă vrei [topic related], click pe video-ul ăsta [end screen]"
             → NU: „Nu uita să dai like și subscribe" generic
```

### Pas 4: Write — Generează scriptul complet cu indicații de regie

**Prompt SHORT-FORM (ChatGPT GPT-4o):**
```
Scrie un script video de [durata] pentru [platforma].

SUBIECT: [din brief]
HOOK ALES: [din Pasul 2 — top 1]
STRUCTURA: IVS (Interrupt → Value → Seed)
TON: [din brief]
AUDIENȚA: [din brief]
WORD COUNT: [din brief — 150 cuvinte/min]

FORMAT SCRIPT:
Folosește exact acest format:

---
[TIMESTAMP] [VIZUAL] [ON-SCREEN TEXT]
VOCE: „[ce spui]"
---

Exemplu:
[0-3s] [Talking head, facial expression surprinsă] [TEXT: „Asta schimbă totul"]
VOCE: „Nimeni nu vorbește despre asta, dar e cea mai importantă schimbare din 2026."

[3-15s] [B-roll: ecran laptop cu [X]] [TEXT: bullet points apărând]
VOCE: „[conținut...]"
[PAUZĂ 1s — lasă ideea să se așeze]

[15-25s] [Talking head, leaning in, serios] [No text]
VOCE: „Și uite care e partea interesantă..."

REGULI:
- Limbaj VORBIT, nu scris. Citește-l cu voce tare — dacă sună ca un essay, rescrie.
- Propoziții scurte. Max 15 cuvinte per propoziție.
- Include [PAUZĂ] strategic (după o afirmație puternică, înainte de reveal)
- Include indicații de [ENERGIE]: [calm → building → peak → settle]
- Sugerează ON-SCREEN TEXT keywords (2-4 cuvinte max per frame)
- CTA la final: [din brief]
```

**Prompt LONG-FORM YouTube (Claude Sonnet — superior la coerență pe texte lungi):**
```
Scrie un script video YouTube de [X] minute pe subiectul „[topic]".

HOOK ALES: [din Pasul 2]
STRUCTURA: HELO (Hook → Explanation → Long Body → Outro)
TON: [din brief]
AUDIENȚA: [din brief]
WORD COUNT: ~[X × 150] cuvinte

Sectioned format:

## HOOK (0:00-0:30)
[Indicații regie] VOCE: „..."
[Re-hook] VOCE: „Și la final..."

## EXPLANATION (0:30-2:00)
[Setup problema] VOCE: „..."

## BODY
### Punctul 1: [Subtitlu] (2:00-5:00)
[Mini-hook] VOCE: „..."
[Content] VOCE: „..."
[Proof/exemplu] VOCE: „..."
[Transition] VOCE: „..."

### Punctul 2: [Subtitlu] (5:00-8:00)
[Loop open: „O să-ți arăt la punctul 5 de ce asta contează dublu"]
...

### Punctul 3-5: ...

## OUTRO (XX:00-end)
[Recap 3 takeaways]
[CTA specific]
[End screen suggestion]

INCLUDE ÎN SCRIPT:
- [B-ROLL: descriere] markers unde e nevoie de footage
- [GRAFIC: descriere] markers unde e nevoie de vizual/diagram
- [PATTERN BREAK] markers la fiecare 2-3 min
- Timestamps orientative per secțiune
- Sugestii de thumbnail concept (2-3 opțiuni)
- Title SEO-optimized (include keyword, max 60 caractere, curiosity gap)
```

### Pas 5: AI Avatar & Voiceover — Pipeline fără filmare

**Synthesia ($29/mo Starter, $89/mo Creator):**
```
Script adaptation pentru AI avatar:
1. Elimină toate indicațiile de regie fizice ([leaning in], [walking])
2. Propoziții scurte: max 12 cuvinte (avatarul pronunță mai natural)
3. Adaugă [PAUZĂ 1S] după fiecare idee importantă
4. Evită: tongue twisters, cuvinte foarte tehnice, acronime nestandard
5. Setează: avatar, background, limba, gesture style
6. Include slides/screens alăturate pentru vizual support
```

**HeyGen ($29/mo Creator):**
```
Avantaje vs. Synthesia: lip sync mai realist, dubbing multilingv,
instant avatar din selfie video (5 min setup).
Best for: multilingual content, personalized video at scale.
```

**ElevenLabs ($5/mo Starter, $22/mo Creator) — Voiceover:**
```
Pipeline voiceover:
1. Scrie scriptul optimizat pentru narration (ton: narrator, nu conversațional)
2. Selectează sau clonează vocea (voice cloning: 1 min audio sample)
3. Generează audio per secțiune (nu tot odată — permite editare)
4. Export: MP3/WAV high quality
5. Editare finală: Descript sau CapCut (sincronizare cu vizualuri)

Cost: ~$0.10-0.30 per minut de audio generat
Calitate: indistinguibil de voce umană la ElevenLabs Turbo v3
```

**Runway Gen-3 ($15/mo Standard) — AI Video Generation:**
```
Use cases marketing:
- B-roll generation: „slow motion coffee being poured, cinematic, warm lighting"
- Product shots: „[product] rotating on marble surface, studio lighting"
- Abstract visuals: „flowing particles forming [brand name], gradient colors"
- Transitions: scene-to-scene AI-generated transitions

NOT recommended for: talking heads, realistic people, text-heavy frames
```

### Pas 6: Platform Optimization — Adaptare per specificul platformei

**TikTok:**
- [ ] Sound/audio: trending sound suggestion (verifică TikTok Creative Center)
- [ ] Hook: funcționează fără audio (on-screen text essential)
- [ ] Caption: max 150 caractere, include 3-5 hashtags relevante
- [ ] Duet/Stitch invitation: opțional CTA „Duet this with your version"
- [ ] Duration sweet spot: 21-34 secunde (highest avg completion rate)

**Instagram Reels:**
- [ ] Cover frame: thumbnail atractiv (text overlay + imagine clară)
- [ ] Save-worthy: include valoare practică pe care userul vrea să o „save"
- [ ] Hashtags: 5-10 în caption, nu în video
- [ ] Audio: trending audio sau original (original = mai multă distribuție)
- [ ] Duration sweet spot: 15-30 secunde (highest reach per Reel)

**YouTube Long-form:**
- [ ] Title: SEO-optimized (include keyword) + curiosity gap + max 60 caractere
- [ ] Thumbnail concept: face expression + text (3-4 cuvinte) + contrasting colors
- [ ] Description: 200+ cuvinte, include keyword în primele 2 propoziții, timestamps, links
- [ ] Tags: 5-10 relevante (keyword + variations)
- [ ] End screen: 2 video suggestions (related + popular)
- [ ] Cards: 1-2 cards la momentele relevante (link alt video sau playlist)
- [ ] Chapters: timestamps ca H2 în description

**YouTube Shorts:**
- [ ] Vertical 9:16, max 60 secunde
- [ ] Hook în prima secundă (nu 3 — Shorts e mai rapid)
- [ ] Loop-worthy: ultimul frame = connects cu primul (auto-replay)
- [ ] „#Shorts" în description (ajută algoritmul)

**LinkedIn Video:**
- [ ] Subtitles OBLIGATORII (80%+ vizionări fără sunet pe LinkedIn)
- [ ] Ton: profesional dar nu corporate (educational, not entertaining)
- [ ] Duration sweet spot: 30-90 secunde
- [ ] Square (1:1) sau vertical (9:16) — nu horizontal

### Pas 7: Optimize — Citește cu voce tare și ajustează

**Checklist de scriptare (pre-filmare/pre-generare):**
1. **Citește cu voce tare** — dacă te împiedici, reformulează
2. **Cronometrează** — 150 cuvinte/minut spoken. Scriptul se încadrează în durata target?
3. **Hook test**: acoperă hook-ul primele 2-3 secunde? E concret, nu vag?
4. **One idea rule** (short-form): ai mai mult de 1 idee? Tăie — fă 2 video-uri.
5. **Energy curve**: scriptul crește în energie sau stagnează? Include [BUILD] markers.
6. **CTA natural**: CTA-ul se integrează organic sau e bolted-on?
7. **Conversational**: înlocuiește orice cuvânt pe care nu l-ai folosi într-o conversație reală.

**AI Optimization Pass** — Prompt pentru **Claude Sonnet**:
```
Review acest script video și optimizează pentru retenție:

[paste script]

Verifică:
1. Hook (primele 3s): opre scroll-ul? E specific, nu generic?
2. Fiecare tranziție: e fluentă? Există „dead spots" (secțiuni plictisitoare)?
3. Pattern breaks: sunt suficiente? (minim 1 la fiecare 15-20s short-form, 2-3 min long-form)
4. Engagement cues: incluzi momente unde viewerul gândește/reacționează?
5. CTA: natural, integrat, nu forțat?
6. Limbaj: vorbit, nu scris? Propoziții sub 15 cuvinte?
7. Ritm: variezi viteza? (rapid pe facts, lent pe emphasis, pauze pe reveal)

Returnează versiunea optimizată cu [MODIFICAT] markers pe ce ai schimbat și de ce.
```

### Pas 8: Batch Production — Scalează la 10-20 scripturi per sesiune

**Metoda Content Series:**
```
Creează o serie de 10 scripturi TikTok pe tema „[topic umbrella]":

Fiecare video: 30-60 secunde, aceeași structură IVS, ton consistent.
Variază: subiectul specific, tipul de hook, exemplul/datele.

Video 1: „[subtopic 1]" — Hook: Pain agitation
Video 2: „[subtopic 2]" — Hook: Bold statement
Video 3: „[subtopic 3]" — Hook: Result teaser
Video 4: „[subtopic 4]" — Hook: Question
Video 5: „[subtopic 5]" — Hook: Stop doing this
Video 6-10: [repete hooks pe subtopice noi]

Format per video: [timestamp][vizual][text] + VOCE: „..."
Include la final: serie connection — Video 5 teasează Video 6.
```

**Metoda Repurposing (1 long-form → 5-8 shorts):**
```
Din acest script YouTube de [X] minute, extrage 6 scripturi short-form (30-60s):

[paste long-form script]

Fiecare short trebuie să:
1. Fie standalone (funcționează fără contextul video-ului original)
2. Aibă un hook nou (nu copy-paste intro)
3. Se concentreze pe UN singur insight/punct din video
4. Includă CTA: „Full video in bio" sau „Link in description"

Sugerează timestamp-ul exact din video-ul original de unde se extrage fiecare clip
(pentru editare rapidă cu Opus Clip sau Descript).
```

### Pas 9: Performance Feedback Loop — Retenție analytics → prompt improvement

**La 7 zile post-publicare, analizează:**

Prompt pentru **Claude Sonnet**:
```
Analizează retenția acestor [X] video-uri publicate săptămâna aceasta:

| Video | Platformă | Hook type | Avg retention | Views | Saves | Shares |
[paste data din TikTok Analytics / YouTube Studio / IG Insights]

Benchmark-uri:
- TikTok avg retention: 50-60% = bun, 70%+ = viral
- YouTube avg retention la 30s: 70%+ = bun
- Instagram Reels avg retention: 40-50% = bun

Identifică:
1. TOP 3 performeri: ce hook type a câștigat? Ce au în comun?
2. BOTTOM 3: unde scade retenția? (la ce secundă/minut e drop-off-ul?)
3. Pattern de save/share: ce tip de conținut e save-worthy?
4. Recomandări: ce hook types să repet, ce să abandonez
5. Sugestie: 3 subiecte noi bazate pe ce a performat cel mai bine
```

### Pas 10: Agency Delivery — Video scripts ca serviciu

**Pachet „Video Script Factory"** (preț sugerat):
- **Basic** ($500/mo): 10 short-form scripts/lună + 1 round revisions
- **Pro** ($1,200/mo): 20 short-form + 4 long-form scripts/lună + series planning + hooks library
- **Scale** ($2,500/mo): 40 short-form + 8 long-form + AI avatar delivery (Synthesia) + voiceover (ElevenLabs)

**Cost intern cu AI:**
- 10 short-form scripts: ~1.5 ore + $1-2 API = $75-100 (la $50/h)
- 20 short-form + 4 long-form: ~4 ore + $3-5 API = $200-250
- Margin: 60-80%

**Deliverable per client:**
1. Google Doc cu scripturi formatate (timestamp + vizual + voce)
2. Hooks library (top performers tracked per month)
3. Monthly performance report cu recomandări AI-generated
4. Thumbnail concepts pentru YouTube (3 opțiuni per video)

---

## 3. Cortex Logging

```json
{
  "text": "Video Script Writing finalizat. Platformă: [TikTok/YouTube/Reels]. Nr. scripturi: [X]. Durată totală: [Y min]. Hook type dominant: [Z]. Format delivery: [talking head/avatar/voiceover]. Batch production: [da/nu]. Avg retention post-publish: [%].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-109",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["video-script", "tiktok", "youtube", "reels", "short-form", "long-form", "ai-avatar", "voiceover", "retention"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + orice producție de conținut video

### WHEN
La scrierea oricărui script video destinat publicării

### HOW (violation detection)
- Script fără hook definit în primele 2-3 secunde → violation
- Același script pe TikTok și YouTube fără adaptare → violation
- Script nu a fost citit cu voce tare / cronometrat → violation
- Script pentru AI avatar fără indicații de pauze → violation
- Short-form cu mai mult de 1 idee principală → violation
- Long-form fără loop opens sau pattern breaks → violation
- Zero performance analysis post-publish → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-105 → social media posts (repurposing script în text)
- AIM-106 → paid social (video ad scripts)
- AIM-103 → content pipeline (video din blog repurposing)

### VERIFY
- [ ] Video Script Brief completat?
- [ ] Minim 8 hooks generate pe 5 categorii?
- [ ] Structura de retenție aplicată (IVS/HELO)?
- [ ] Indicații de regie incluse [VIZUAL][TEXT][VOCE]?
- [ ] Citit cu voce tare și cronometrat?
- [ ] Adaptat pentru specificul platformei?
- [ ] AI avatar/voiceover: script adaptat (propoziții scurte, pauze)?
- [ ] Performance analysis programat la 7 zile?

**VK-uri:**
1. `✅ [PROC] AIM-109 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Video Script Writing" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Hook generation | ChatGPT GPT-4o | Creativitate, punch, diversitate |
| Short-form scripts | ChatGPT GPT-4o | Ton conversațional, energie |
| Long-form scripts | Claude Sonnet | Coerență pe text lung, structurare |
| Script optimization | Claude Sonnet | Analiză retenție, detectare dead spots |
| Batch generation series | ChatGPT GPT-4o / DeepSeek | Volume, cost-efficient |
| Performance analysis | Claude Sonnet | Structured analysis, patterns |
| AI voiceover | ElevenLabs | Calitate vocală superioară |
| AI avatar delivery | HeyGen / Synthesia | Lip sync realist, multilingv |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT GPT-4o / Claude Sonnet | Generare script |
| Synthesia ($29/mo) / HeyGen ($29/mo) | AI avatar video delivery |
| ElevenLabs ($5-22/mo) | AI voiceover narration |
| Runway Gen-3 ($15/mo) | AI B-roll generation |
| CapCut Pro ($10/mo) / Descript ($24/mo) | Editare, auto-captions |
| Opus Clip ($19/mo) | Repurposing long-form → shorts |
| TikTok Creator Center (free) | Analytics, trending sounds |
| YouTube Studio (free) | Retention curve, analytics |

---

## 6. Instrumente

- ChatGPT Plus ($20/mo) — scriptare creativă, hooks, short-form
- Claude Pro ($20/mo) — long-form scripts, optimization, analysis
- ElevenLabs ($5-22/mo) — voice cloning, narration AI
- Synthesia ($29/mo) / HeyGen ($29/mo) — AI avatar video
- Runway Gen-3 ($15/mo) — AI B-roll, visual generation
- CapCut Pro ($10/mo) — editare AI, auto-captions, trending templates
- Descript ($24/mo) — editare audio/video cu transcript, filler removal
- Opus Clip ($19/mo) — repurposing long-form → short clips automat
- vidIQ ($9/mo) / TubeBuddy ($5/mo) — YouTube SEO, title/tag optimization
- TikTok Creative Center (free) — trending sounds, formats, benchmarks

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Retenție medie TikTok | ≥ 55% |
| Retenție YouTube la 30s | ≥ 70% |
| Retenție YouTube avg (long-form) | ≥ 45% |
| Timp scriere script short-form | ≤ 10 min |
| Timp scriere script long-form | ≤ 45 min |
| Batch production (10 shorts) | ≤ 2 ore |
| Cost per script (AI) | ≤ $0.50 |
| Save rate (IG/TikTok) | ≥ 2% |
| Hooks unice/total | ≥ 80% (nu repete) |
