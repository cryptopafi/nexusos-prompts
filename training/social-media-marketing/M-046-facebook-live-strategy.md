---
type: procedure
created: 2026-03-22
status: active
slug: m-046-facebook-live-strategy
tags: [procedure, nexus]
---

# Facebook Live and Live Video Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește procesul complet de planificare, execuție și valorificare a sesiunilor Facebook Live și live video pentru creșterea reach-ului organic, engagement-ului și conversiilor.

---

## 1. Problema

Facebook Live primește reach organic de 6x mai mare decât videoclipurile pre-înregistrate conform datelor Meta, dar majoritatea brandurilor evită live-urile din cauza lipsei unui framework structurat. O sesiune live nepregătită poate dăuna mai mult decât ajuta, iar fără o strategie de repurposing, efortul depus este pierdut după transmisie.

Situații acoperite:
- Brand sau creator care nu folosește live video din lipsa unui protocol clar
- Live-uri realizate fără promovare prealabilă cu audiență mică la transmisie
- Conținut live fără strategie de repurposing post-transmisie

---

## 2. Procedura

### Pas 1: Planificare topic și format live (cu minimum 72 ore înainte)
Alege un topic specific și limitat (nu general) bazat pe: întrebări frecvente din comentarii/DM-uri, anunțuri de produs/serviciu, tutoriale how-to, Q&A sesiuni, interviews cu experți invitați. Definește formatul: durată (20-45 minute optim), structura (introducere 3 min, conținut principal 30-35 min, Q&A 5-7 min), CTA final (1 singur call-to-action clar). Pregătește un script sau bullet outline — nu citit mot-a-mot, dar cu punctele cheie notate.

### Pas 2: Promovare pre-live pe multiple canale
Cu 72 ore înainte: post announcement pe pagina Facebook cu data/ora și teaser topic. Cu 24 ore: reminder în Stories (Facebook + Instagram dacă conturile sunt legate). Cu 1 oră: reminder final + "Dați remind ca să nu pierdeți". Opțional: anunț în newsletter sau grup Facebook dedicat. Creează un eveniment Facebook pentru live-ul planificat și invită urmăritorii relevanți.

### Pas 3: Setup tehnic și test pre-live (30 minute înainte)
Verifică: conexiune internet stabilă (minim 10 Mbps upload, preferabil cablu nu WiFi), iluminare adecvată (lumina din față, nu din spate), sunet clar (microfon extern recomandat față de built-in), fundal curat sau branded. Testează o transmisie privată de 2 minute pentru a confirma calitatea audio-video. Pregătește: notele, link-urile de partajat în comentarii, apa, telefonul în modul avion dacă transmiți de pe alt device.

### Pas 4: Execuție live cu tehnici de engagement activ
În primele 2 minute: salută cei care se alătură pe nume, explică ce urmează și cere un semn (comentariu cu un emoji sau număr) pentru a activa algoritmul. Pe parcurs: pune întrebări audienței la fiecare 5-7 minute, citește și răspunde la comentarii în direct, repeta punctele cheie periodic (oamenii intră în orice moment). Finalizează cu CTA clar: link în comentarii, formular, produs, sau next live.

### Pas 5: Engagement post-live în primele 2 ore
Imediat după live: răspunde la toate comentariile rămase fără răspuns, postează link-urile promise în comentariile live-ului, partajează clipul pe Stories cu teaser pentru replay. Mulțumește top comentatori individual. Activitatea post-live în primele 2 ore semnalizează algoritmului că videoclipul merită distribuire extinsă.

### Pas 6: Repurposing video live pentru multiple formate
Descarcă videoclipul live și editează: extrage clip-uri de 30-90 secunde pentru Reels/TikTok (momentele cele mai valoroase sau funny), creează audiogram pentru podcast/audio content, transformă punctele principale în carousel post sau articol de blog, extrage quotes pentru postări grafice. Un live de 45 minute poate genera 10-15 piese de conținut repurposed.

### Pas 7: Analiză metrici și planificare next live
Revizuiește în Facebook Insights (la 48 ore după live): peak viewers, average watch time, total reach, engagement (reactions + comments + shares), clicks pe link-ul din comentarii. Identifică momentul cu cel mai mare drop în vizualizatori — acesta indică unde ai pierdut audiența. Target: average watch time > 50% din durata totală, engagement rate > 5% din reach. Programează următorul live în max 2 săptămâni pentru consistență.

---

## 3. Cortex Logging

```json
{
  "text": "Facebook Live Strategy executată: live planificat, promovat, executat cu engagement activ, repurposing conținut realizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-046",
    "domain": "social-media-marketing",
    "rule_id": "META-H-002",
    "tags": ["facebook-live", "live-video", "streaming", "engagement", "repurposing"],
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
- Live realizat fără promovare prealabilă minimum 24 ore → violation
- Lipsă test tehnic pre-live (probleme audio/video evitabile) → violation
- Conținut live fără plan de repurposing post-transmisie → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum social-media-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Promovare pre-live realizată pe minimum 2 canale?
- [ ] Plan de repurposing definit înainte de live?

**VK-uri obligatorii:**
1. `✅ [PROC] M-046 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook Live and Live Video Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Page / Creator Studio | Platformă transmisie + analytics |
| Streamlabs / OBS (opțional) | Setup avansat live streaming |
| Microfon extern | Calitate audio profesională |
| Descript / CapCut | Editare și repurposing video post-live |
| Meta Business Suite | Programare anunțuri și Events pentru live |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Average watch time | > 50% din durata totală |
| Engagement rate live | > 5% din reach |
| Piese conținut repurposed per live | Minimum 5 |
