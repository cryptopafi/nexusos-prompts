---
type: procedure
created: 2026-03-22
status: active
slug: m-062-podcast-launch-strategy
tags: [procedure, nexus]
---

# Podcast Launch and Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Strategie completă de lansare și creștere a unui podcast — de la concept și setup tehnic la distribuție, creșterea audienței și monetizare.

---

## 1. Problema

Majoritatea podcasturilor lansate dispar după primele 7 episoade ("podcast graveyard") din cauza lipsei unei strategii clare, a workflow-ului de producție inconsistent și a așteptărilor nerealiste de creștere. Lansarea fără un plan de distribuție și o nișă bine definită face imposibilă construirea unei audiențe loiale.

Situații acoperite:
- Lansarea unui podcast nou de la zero
- Relansarea unui podcast existent cu strategie nouă
- Integrarea unui podcast în ecosistemul de content marketing al unui brand

---

## 2. Procedura

### Pas 1: Definirea conceptului și nișei de podcast
Definește cu precizie: nișa (specific, nu generic — "marketing digital pentru antreprenori solopreneurs" vs. "business"), formatul (interviu, solo, co-host, panel, storytelling), frecvența de publicare sustenabilă (săptămânal sau biweekly, nu zilnic la start), durata episoadelor (15-30 min pentru educațional, 45-90 min pentru interviu), și audiența țintă. Validează conceptul — verifică că există podcast-uri similare cu audiență activă (validare cerere) dar spațiu pentru diferențiere.

### Pas 2: Setup tehnic și echipament
Configurează setup-ul de înregistrare: microfon USB sau XLR (minimum Samson Q2U sau Blue Yeti), pop filter și shock mount pentru calitate audio profesională, software de înregistrare (Audacity gratuit, Adobe Audition, Descript), și acoustic treatment de bază (înregistrează în cameră cu mobilier texturat, nu ecouri). Alege hosting platform: Buzzsprout, Anchor (Spotify for Podcasters), Libsyn, sau Transistor — platformele generează automat RSS feed pentru distribuție pe toate platformele.

### Pas 3: Crearea identității vizuale și brandingului de podcast
Creează artwork-ul de podcast: cover art 3000x3000px, format JPEG sau PNG, text lizibil la 300x300px (dimensiunea de afișare în Apple Podcasts). Cover art-ul este thumbnail-ul podcastului — investiție în design profesional. Creează numele episoadelor cu template consistent, jingle de intro (5-15 sec), și template de show notes. Toate elementele trebuie să fie recognoscibile și consistente cu brand identity (M-055) dacă este podcast de brand.

### Pas 4: Producția episoadelor de lansare (batch)
Înregistrează minimum 3-5 episoade înainte de lansarea publică. Batch production pentru lansare creează momentum inițial și evită presiunea producției week-by-week în prima perioadă. Structura unui episod standard: intro hook (30 sec), introducere presenter (60 sec), body conținut (bulk), recap și takeaways, și outro cu CTA și abonare. Editează consistent: elimină uhm-uri excesive, pauze lungi, și probleme de audio. Exportă MP3 la 128kbps (mono) sau 192kbps (stereo).

### Pas 5: Distribuirea pe toate platformele majore
Submitează RSS feed-ul pe toate platformele principale: Apple Podcasts (cel mai important pentru ranking), Spotify, Google Podcasts, Amazon Music/Audible, Stitcher, Pocket Casts, și Overcast. Procesul de aprobare durează 1-7 zile. Creează show notes optimizate SEO pentru fiecare episod pe website-ul propriu (transcriptul parțial + timestamps + link-uri menționate). Show notes cu episoadele vechi continuă să aducă trafic organic pe termen lung.

### Pas 6: Strategia de creștere și promovare
Lansează cu o "pod-fade prevention" strategie: (1) promovare cross-canal — newsletter, social media, colaborări; (2) invitați cu audiențe proprii care vor promova episodul; (3) apariții ca guest pe alte podcast-uri relevante (podcast swaps); (4) short clips din episoade pentru Reels/TikTok/YouTube Shorts; (5) listare în directoare de podcast-uri; (6) campanie de review-uri la lansare — solicită subscribers să lase review în primele 7 zile.

### Pas 7: Monitorizare, iterare și monetizare
Monitorizează trimestrial: downloads per episod, ascultători unici, completion rate, top episoade și geografii. Apple Podcasts Analytics și Spotify for Podcasters oferă date detaliate. Iterează bazat pe date: formatul episoadelor cu completion rate ridicat repetă mai des; topicurile cu downloads mari indică ce vrea audiența. Monetizare: host-read ads (Podcorn, AdvertiseCast), abonamente premium (Supercast, Supporting Cast), vânzare produse proprii prin podast, și live events.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-062 executată: Podcast launch strategy completă — concept, setup tehnic, identitate vizuală, producție batch, distribuție și strategie de creștere configurate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-062",
    "domain": "video-marketing",
    "rule_id": "META-H-002",
    "tags": ["podcast", "podcast-launch", "audio-marketing", "content-strategy", "audience-growth"],
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
- Podcast lansat fără minimum 3 episoade pregătite → violation
- Episod publicat fără show notes optimizate SEO → violation
- Audio sub standard acceptabil publicat (distorsiuni, ecouri majore) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum video-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Podcast distribuit pe minimum 5 platforme majore?
- [ ] Minimum 3 episoade batch-produse înainte de lansare?

**VK-uri obligatorii:**
1. `✅ [PROC] M-062 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Podcast Launch and Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Buzzsprout / Transistor / Libsyn | Hosting podcast și distribuție RSS |
| Audacity / Descript / Adobe Audition | Editare și producție audio |
| Apple Podcasts / Spotify for Podcasters | Platforme principale distribuție și analytics |
| Brand Identity Design (M-055) | Cover art și branding podcast |
| Content Repurposing (M-052/M-083) | Repurposing episoade în video clips și articole |
| YouTube Channel (M-057) | Cross-distribuție episoade ca video pe YouTube |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Downloads per episod (primele 30 zile) | > 100 (nou), > 500 (etablit) |
| Completion rate episod | > 60% |
| Platforme de distribuție | Minimum 5 |
| Episoade produse înainte de lansare | Minimum 3 |
