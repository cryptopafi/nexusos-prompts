---
type: procedure
created: 2026-03-20
status: active
slug: aim-104-ai-brand-strategy-naming-workflow
tags: [procedure, nexus]
---

# AI Brand Strategy and Naming Workflow — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Utilizarea AI pentru a accelera procesul de brand strategy și naming, de la research de piață și audiență la generarea de opțiuni de nume, tagline-uri, messaging frameworks și brand verbal identity. Include prompt engineering specializat pentru naming, multi-model creative generation, validation framework și delivery ca serviciu de agenție.

---

## 1. Problema

Brand strategy și naming sunt procese tradițional lente (4-8 săptămâni în agenții mari, cost $5,000-50,000+) și limitant creative (o echipă de 3-5 persoane generează 50-100 opțiuni). AI comprimă procesul la 3-5 zile, generează 200-500 opțiuni diverse, și permite explorarea creativă pe direcții pe care o echipă mică nu le-ar fi acoperit niciodată. Dar fără metodologie, AI produce nume generice, tagline-uri cliché, și zero legătură cu strategia de brand. Această procedură combină rigoarea strategică a unui brand strategist senior cu puterea generativă a AI.

Situații acoperite:
- Naming produs nou sau linie de produse (B2C sau B2B)
- Rebranding complet (inclusiv rename, repozițționare)
- Naming pentru sub-brand, vertical, sau feature
- Generarea de tagline-uri, sloganuri și mesaje de poziționare
- Brand verbal identity (voice, tone, messaging framework)
- Naming ca serviciu de agenție/consultanță (deliverable client)

---

## 2. Procedura

### Pas 1: Discovery — Definește contextul strategic al brandului (non-negociabil)

Completează acest Brand Strategy Brief înainte de orice generare. AI fără context strategic = rezultate generice:

```markdown
# Brand Strategy Brief — [Proiect]

## Business Context
- Ce face produsul/serviciul: [descriere 2-3 propoziții]
- Pentru cine (ICP concret): [demographics + psychographics + comportament]
- Problema #1 pe care o rezolvă: [1 propoziție clară]
- Diferențiator principal vs. alternativa dominantă: [ce face diferit]
- Model de business: [SaaS/e-com/servicii/DTC/marketplace]
- Stadiu: [pre-launch / early stage / rebrand / product extension]

## Brand Values & Personality
- Valori (3-5, specifice, nu generice):
  [Ex: „transparency over polish" nu „quality" — „accessible expertise" nu „innovation"]
- Personalitate de brand (dacă brandul ar fi o persoană):
  [Ex: „prieten expert care explică simplu lucruri complicate, nu profesor la catedră"]
- Ton dorit: [serios/jucăuș, simplu/sofisticat, local/global, warm/edgy]
- Branduri aspiraționale (nu competitori, ci tonul/feel-ul dorit):
  [Ex: „vrem să simțim ca Notion — clean, smart, minimal"]

## Naming Constraints
- Piețe geografice: [ce limbi trebuie să funcționeze]
- Lungime preferată: [monosilabic / max 3 silabe / flexibil]
- Tip de nume preferat: [descriptiv / abstract / inventat / eponym / no preference]
- Hard no's: [ce nu vrei — „nu inventat", „nu acronime", „nu similar cu X"]
- Domeniu necesar: [.com obligatoriu / .io acceptabil / TLD local]

## Competitori direcți (5-8)
- [Competitor 1] — [ce comunică numele lor]
- [Competitor 2] — ...
```

**Model routing**: Acest pas este **uman** — AI nu poate completa discovery-ul. Se completează prin workshop cu clientul sau fondatorul (60-90 minute). AI poate ajuta la structurarea notițelor post-workshop.

### Pas 2: Landscape — Analizează peisajul de naming al categoriei cu AI

Colectează 15-20 branduri din categorie și categorii adiacente.

Prompt pentru **Claude Sonnet** (analiză structurată):
```
Ești un brand naming strategist. Analizează aceste branduri din [categorie]:
[Lista de 15-20 branduri cu descriere scurtă]

Clasifică-le pe o matrice 2x2:
- Axă X: Descriptiv ←→ Abstract
- Axă Y: Serios ←→ Jucăuș/Friendly

Pentru fiecare brand, notează:
1. Tip de nume (descriptiv / metaforic / inventat / acronim / fondator / portmanteau)
2. Nr. silabe și structură fonetică
3. Ce comunică numele implicit (fără a cunoaște brandul)
4. Registrul de limbă (.com / inventat / real cuvânt)

La final sintetizează:
- Ce patternuri de naming sunt SUPRASATURATE în categorie (evită)
- Ce spații de naming sunt NEOCUPATE (oportunitate)
- Dacă brandul nostru e [descriere din brief], ce quadrant din matrice ar oferi maximum diferențiere?
- Recomandare: top 3 direcții de naming pentru proiectul nostru și de ce
```

**Model routing**: Claude Sonnet — superior la analiză structurată pe matrice.

### Pas 3: Generate — Rulează 7-10 runde creative cu prompts diferite (200-500 opțiuni)

**Regula de aur**: Nu cere AI-ului „dă-mi 20 de nume". Rulează runde separate, fiecare cu un unghi creativ diferit. Diversitatea vine din diversitatea prompturilor, nu din volumul cerut.

**Runda 1 — Descriptiv direct** (ChatGPT GPT-4o):
```
Generează 30 de nume de brand pentru [business — din brief].
DIRECȚIA: descriptive — numele descrie ce face produsul sau rezultatul pe care îl produce.
Exemple de referință din alte categorii: Salesforce, Shopify, Grammarly, Workday.
Constraint-uri: max 3 silabe, funcționează în [limbile din brief], ușor de pronunțat.
Format: Nume — Explicație scurtă (de ce funcționează)
```

**Runda 2 — Metafore și asocieri** (ChatGPT GPT-4o):
```
Generează 30 de nume de brand pentru [business].
DIRECȚIA: metaforică — un concept din altă domeniu (natură, mitologie, știință,
arhitectură, muzică, sport) aplicat la business-ul nostru.
Valorile brandului: [din brief]. Ce sentiment vrem: [din brief].
Exemple de referință: Nike (zeița victoriei), Amazon (cel mai mare fluviu),
Uber (super/supra în germană), Slack (spațiu/lejeritate).
Explorează asocieri din: natură, spațiu, mecanică, artă, jocuri, culori.
Format: Nume — Metafora/Asocierea — De ce funcționează
```

**Runda 3 — Inventate/Portmanteau** (ChatGPT GPT-4o):
```
Generează 30 de nume inventate (nu cuvinte existente) pentru [business].
DIRECȚIA: cuvinte noi create prin: fuziune de cuvinte (portmanteau),
modificarea ortografiei unui cuvânt existent, combinarea de prefixe/sufixe latine/greceești,
sugestii fonetice (sunetul numelui evocă un feeling).
Exemple de referință: Spotify (spot+identify), Figma (figment+magma), Zapier (zap+easier),
Canva (canvas), Klarna (clar).
Valorile brandului: [din brief]. Feeling dorit: [din brief].
Format: Nume — Originea/Construcția — Feeling fonetic
```

**Runda 4 — Cuvinte reale din alte limbi** (Claude Sonnet):
```
Generează 25 de nume de brand pentru [business] bazate pe cuvinte reale din:
latină, greacă, japoneză, sanscrită, nordică, italiană, spaniolă.
Criteriu: cuvântul original să aibă un înțeles relevant pentru [valorile din brief],
dar pronunția să fie accesibilă unui vorbitor de engleză.
Exemple de referință: Volvo (latină, „eu rulez"), Lego (daneză, „joacă bine"),
Audi (latină, „ascultă"), Sonos (latină, „sunet").
Format: Nume — Limba — Înțeles original — De ce funcționează
```

**Runda 5 — Abstracte/Emoționale** (ChatGPT GPT-4o):
```
Generează 25 de nume de brand care evocă un FEELING, nu o funcție.
Business: [din brief]. Emoția principală pe care vrem să o transmitem: [din brief].
Direcția: cuvinte scurte (1-2 silabe), ușor de pronunțat, care sună ca feeling-ul dorit.
Gândește-te la sunetul numelui: vocale deschise (a, o) = warmth, consonante tari (k, t) = power,
sibilante (s, z) = smooth/modern, vocale închise (i, u) = precision.
Exemple de referință: Calm, Notion, Stripe, Bloom, Drift, Ripple.
Format: Nume — Ce feeling evocă — Profil fonetic
```

**Runda 6 — Acronime inteligente** (Claude Sonnet):
```
Generează 15 acronime de brand pentru [business].
Acronimele trebuie să: formeze un cuvânt pronunțabil, să aibă un înțeles dual
(acronimul + cuvântul format), să fie max 5 litere.
Cuvintele sursă trebuie să reflecte valorile din [brief].
Exemple de referință: ASAP, NASA (nu acronim de brand dar funcționează fonetic).
Format: ACRONIM — Ce reprezintă — De ce funcționează
```

**Runda 7 — Provocatoare/Contrarian** (ChatGPT GPT-4o):
```
Generează 20 de nume de brand care sunt provocatoare, neconvenționale sau contrarian.
Business: [din brief]. Categoria competitivă e saturată cu nume [tipul dominant din Pasul 2].
Vrem un nume care iese din tipar, care face oamenii să întrebe „ce e asta?".
Direcția: cuvinte neașteptate în context, contradicții deliberate,
umor subtil, sau referințe pop culture.
Exemple de referință: Discord (pentru chat), Slack (pentru productivitate),
Innocent (pentru sucuri), Death Wish Coffee.
Format: Nume — De ce e provocator — De ce funcționează totuși
```

Rulează runde suplimentare dacă primele 7 nu au produs cel puțin 10 candidați buni. Colectează TOATE opțiunile (200-500) într-un singur document, nefiltrare. Filtrarea vine în pasul următor.

### Pas 4: Filter — Aplică 7 criterii de screening eliminatoriu

**Filtrare Tier 1 — Eliminatorii hard (eliminare rapidă):**
1. **Pronunțabilitate**: citește-l cu voce tare. Dacă eziti → eliminat.
2. **Scriere intuitivă**: îl poți scrie corect după ce l-ai auzit o dată? Dacă nu → eliminat.
3. **Lungime**: >12 caractere → eliminat (excepție: portmanteau elegant).
4. **Conotații negative**: testează în [limbile din brief]. Prompt: „Cuvântul [X] are conotații negative, vulgare, sau asocieri nedorite în [limbă]? Include slang și argou."

**Filtrare Tier 2 — Verificări tehnice (10-15 minute per candidat):**
5. **Domeniu disponibil**: verifică pe Namecheap/Instant Domain Search — .com sau TLD din brief
6. **Social handles**: verifică @brand pe Instagram, Twitter/X, TikTok, LinkedIn
7. **Trademark**: caută pe EUIPO (European trademark) + USPTO (US) + OSIM (România dacă relevant)

**Post-filtrare**: din 200-500 opțiuni rămân 15-25 candidați. Marchează-i „shortlist".

### Pas 5: Evaluate — Scorare structurată shortlist

Prompt pentru **Claude Sonnet**:
```
Scorează fiecare din aceste [X] candidați de brand name pe 6 criterii (1-5):

Context business: [din brief — 3 propoziții]
Valorile brandului: [din brief]
Audiența: [din brief]
Competitorii (pentru diferențiere): [din brief]

CANDIDAȚI: [lista]

Criterii:
1. MEMORABILITATE (sticky, rămâne în minte, ușor de reținut)
2. RELEVANȚĂ (conexiune cu business-ul, chiar indirectă)
3. DIFERENȚIERE (se distinge de competitorii din categorie)
4. FONETIC (sună bine spus cu voce tare, are ritm)
5. EXTENSIBILITATE (poate acoperi viitoare produse/verticale)
6. EMOȚIE (evocă sentimentul dorit al brandului)

Scor total = media ponderată (memorabilitate 25%, relevanță 20%, diferențiere 20%,
fonetic 15%, extensibilitate 10%, emoție 10%).

Format tabel: Nume | M | R | D | F | E | Em | Total | Comentariu scurt
Sortează descrescător pe total. Top 5 trec în finala.
```

### Pas 6: Test — Testează finaliștii cu audiența reală

**Metoda rapidă (2-3 zile, $0):**
- Google Forms cu 10-20 persoane din ICP sau echipă extinsă
- 5 întrebări per candidat: (1) Ce îți vine în minte când auzi [NUME]? (2) Ce crezi că face compania? (3) Poți scrie corect numele după ce l-ai auzit? (4) Pe o scară 1-5, cât de ușor e de reținut? (5) Ce emoție/feeling îți transmite?

**Metoda profesională (1-2 săptămâni, $50-500):**
- UsabilityHub / Maze — A/B test pe 100+ respondents
- Pickfu — preference test rapid ($50 per test, 50 respondents)
- Social media poll (LinkedIn/Instagram story) — gratuit, volum mare, bias potențial

**Analiză rezultate cu AI:**
```
Analizează rezultatele testului de naming (N=[X] respondents):

[paste raw data sau summary per candidat]

Identifică:
1. Top 3 cu cel mai bun scor overall — și de ce
2. Candidatul cu cea mai mare divergență de opinii — ce înseamnă asta
3. Cel mai bine înțeles corect (asociere cu business-ul real)
4. Cel mai memorabil (cel menționat din memorie, fără prompt)
5. Recomandarea finală cu argumentare
```

### Pas 7: Tagline — Generează tagline-uri și messaging framework pentru finaliști

Pentru top 3 candidați, generează tagline-uri diverse:

Prompt pentru **ChatGPT GPT-4o** (creativity-optimized):
```
Brand name: [FINALIST]
Business: [din brief]
USP: [diferențiator]
Audiența: [ICP]
Ton: [din brief]
Competitori (tagline-uri lor): [lista]

Generează 20 tagline-uri organizate pe 4 categorii:

DESCRIPTIVE (ce face): „[Nume] — [beneficiu principal]"
EMOȚIONALE (cum te face să te simți): „[Nume] — [stare dorită]"
ASPIRAȚIONALE (ce devii): „[Nume] — [transformare promisă]"
PROVOCATOARE (challenge status quo): „[Nume] — [reframe perspectivă]"

5 per categorie. Lungime: 3-7 cuvinte. Evită cliché-uri: „your partner in...",
„where X meets Y", „reimagining X". Fiecare tagline trebuie să funcționeze
standalone pe un business card sau billboard.
```

### Pas 8: Messaging Framework — Construiește messaging complet

Prompt pentru **Claude Sonnet**:
```
Brand name ales: [NUME]
Tagline: [TAGLINE]
Business: [din brief]
Audiența: [ICP]

Creează un Brand Messaging Framework cu:

1. POSITIONING STATEMENT (1 propoziție, format: „[Brand] este [categoria] pentru [ICP]
   care vrea [rezultat dorit], diferit de [alternativa] prin [diferențiator].")

2. ELEVATOR PITCH (30 secunde, 50-75 cuvinte)

3. BRAND PROMISE (1 propoziție — angajamentul emoțional)

4. VALUE PROPOSITIONS (3 piloni, fiecare cu: headline + 1 paragraf explicativ)

5. TONE OF VOICE GUIDELINES:
   - 4 adjective cu „și" / „dar nu" (ex: „friendly dar nu childish")
   - 3 exemple de propoziție „așa da" vs. „așa nu"
   - Lista de cuvinte de folosit frecvent (10)
   - Lista de cuvinte de evitat (10)

6. KEY MESSAGES per audiență:
   - Prospect (prima interacțiune)
   - Lead (consideration)
   - Client (post-purchase)
   - Investor (pitch)
   - Press (PR)
```

### Pas 9: Document — Creează Brand Verbal Identity deliverable

Compilează totul într-un document de 5-10 pagini:

1. **Naming rationale** (1 pagina): numele ales, procesul de selecție, de ce acest nume, alternative considerate
2. **Tagline și positioning** (1 pagina): tagline, positioning statement, elevator pitch
3. **Messaging framework** (2 pagini): value props, key messages per audiență
4. **Tone of voice** (2 pagini): adjective, do/don't, exemple, word lists
5. **Visual naming** (1 pagina): cum se scrie (capitalization, spațiere), cum se pronunță (fonetică), variante acceptate (abreviere, etc.)
6. **Appendix**: lista completă de candidați evaluați, scoruri, rezultate test

**Preț agenție** pentru acest deliverable: $2,000-8,000 (costul real cu AI: 2-3 zile muncă × tariful tău + $30-50 tool costs).

### Pas 10: QA Final — Verificare completă anti-hallucination și anti-overlap

Înainte de prezentare client sau decizie finală:
1. **Trademark search profesional** pentru numele ales (nu doar EUIPO/USPTO — include baze locale)
2. **Domeniu achiziționat** (nu doar verificat — cumpără imediat la decizie)
3. **Social handles secured** (înregistrează pe top 4 platforme)
4. **Google search test**: caută „[brand name]" — ce apare? Overlap cu altceva?
5. **Pronunțare multilingvă**: cere 3 native speakers din piețele target să pronunțe
6. **Conotații culturale**: double-check pe piețele target (AI poate rata nuanțe culturale)

---

## 3. Cortex Logging

```json
{
  "text": "Brand Strategy & Naming executat pentru [business/produs]. Opțiuni generate: [X]. Filtrate: [Y]. Finaliști: [3 nume]. Testate cu [Z] respondents. Decizie finală: [NUME]. Tagline ales: [text]. Messaging framework complet: [da/nu].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-104",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["brand-strategy", "naming", "brand-identity", "tagline", "positioning", "messaging-framework", "verbal-identity"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + orice proiect de naming sau rebranding

### WHEN
La fiecare decizie de brand naming sau rebranding

### HOW (violation detection)
- Generare nume fără Brand Strategy Brief completat → violation
- Mai puțin de 5 runde creative diferite → violation (insuficient diversitate)
- Nicio verificare trademark sau domeniu → violation critică
- Decizie finală fără testare cu audiența → violation
- Messaging framework absent → violation
- Brand verbal identity document incomplet → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-103 → content pipeline (brand voice input direct)
- AIM-105 → social media (brand voice aplicat pe postări)
- AIM-106 → ad copy (messaging alignment cu positioning)

### VERIFY
- [ ] Brand Strategy Brief completat integral (Pasul 1)?
- [ ] Landscape competitiv analizat (15+ branduri)?
- [ ] Minim 200 opțiuni generate prin 7+ runde diverse?
- [ ] Filtrare pe 7 criterii eliminatorii aplicată?
- [ ] Scorare structurată pe 6 criterii pentru shortlist?
- [ ] Test cu audiența realizat (minim 10 respondents)?
- [ ] Tagline-uri generate pe 4 categorii?
- [ ] Messaging framework complet?
- [ ] Brand verbal identity document creat?
- [ ] Trademark + domeniu + handles verificate/secured?

**VK-uri:**
1. `✅ [PROC] AIM-104 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Brand Strategy and Naming Workflow" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Landscape analiză | Claude Sonnet | Matrice structurată, pattern recognition |
| Generare naming (runde creative) | ChatGPT GPT-4o | Creativitate, diversitate, volume |
| Cuvinte din alte limbi | Claude Sonnet | Cunoștințe lingvistice, acuratețe |
| Scorare și evaluare | Claude Sonnet | Analiză obiectivă, structurată |
| Tagline-uri creative | ChatGPT GPT-4o | Punch, ritmicitate, persuasiune |
| Messaging framework | Claude Sonnet | Coerență, nuanță, profunzime |
| Decizie strategică finală | Claude Opus | Viziune pe termen lung, trade-offs |
| Conotații culturale check | Gemini Pro | Web access, context cultural live |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT GPT-4o / Claude Sonnet | Generare naming și taglines |
| Claude Sonnet | Analiză, messaging framework |
| Namecheap / Instant Domain Search | Verificare disponibilitate domeniu |
| EUIPO / USPTO / OSIM | Verificare trademark |
| Google Forms / Pickfu | Test cu audiență |
| Notion / Google Docs | Brand verbal identity document |
| Instagram / Twitter / LinkedIn | Verificare handle availability |

---

## 6. Instrumente

- ChatGPT Plus ($20/mo) — generare creativă naming, taglines
- Claude Pro ($20/mo) — analiză strategică, messaging, scoring
- Gemini Pro (free/$20/mo) — verificare conotații culturale live
- Namelix.com (free) — generator vizual de nume cu logo preview
- Instant Domain Search (free) — verificare domeniu în timp real
- Lean Domain Search (free) — sugestii domeniu cu keyword
- Pickfu ($50/test) — A/B preference testing rapid
- Brand24 / Mention — verificare utilizare online actuală a numelui
- Notion / Google Docs — deliverable final

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Timp discovery → decizie finală | ≤ 5 zile |
| Opțiuni generate înainte de filtrare | ≥ 200 |
| Runde creative diferite | ≥ 7 |
| Candidați care trec filtrare Tier 1+2 | 15-25 |
| Scor test audiență pentru finalistul ales | ≥ 3.5/5 |
| Domeniu .com secured | 100% |
| Messaging framework complet | 100% |
| Brand verbal identity document | ≥ 5 pagini |
