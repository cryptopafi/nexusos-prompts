---
type: procedure
created: 2026-03-22
status: active
slug: m-041-twitter-x-marketing-strategy
tags: [procedure, nexus]
---

# Twitter/X Marketing Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Dezvoltarea și implementarea unei strategii de marketing pe Twitter/X pentru construirea autorității, creșterea audienței și generarea de trafic și leads.

---

## 1. Problema

Twitter/X are un algoritm și o cultură distincte față de alte platforme sociale — conținut care funcționează pe Instagram sau LinkedIn eșuează pe X. Fără o strategie adaptată specificului platformei (viteza conversației, formatul thread-urilor, importanța engagement-ului timpuriu), prezența pe X nu generează reach sau oportunități de business.

Situații acoperite:
- Lansare strategie X pentru brand sau personal brand de la zero
- Repositionare și reactivare cont X inactiv sau cu engagement scăzut
- Scalare prezență X de la audiență mică la poziție de thought leadership în nișă

---

## 2. Procedura

### Pas 1: Optimizare profil X pentru conversie și credibilitate
Setează o fotografie de profil profesională și consistentă cu celelalte platforme. Scrie un bio de 160 caractere cu: ce faci, pentru cine, dovadă socială sau număr relevant (ex: "10K+ clienți", "5 ani în marketing digital"), și un CTA cu link (X Premium permite un link aparte în profil). Creează un Header Image (1500x500px) care comunică vizual nixa ta. Activează X Premium (fostul Twitter Blue) dacă audiența ta este în principal pe X — deblochează editing, thread-uri mai lungi și reach prioritar.

### Pas 2: Definire nișă, voce și piloni de conținut
X recompensează conturi cu nișă clară și voce distinctă. Definește: nișa exactă (nu "marketing general", ci "Facebook Ads pentru ecommerce"), vocea (expert direct și opinat, sau educativ și prietenos), 3-4 piloni de conținut (ex: tips tactici, opinii despre industrie, behind-the-scenes, case studies). Evită să postezi despre orice — focusul pe nișă atrage followeri calificați care devin ulterior clienți.

### Pas 3: Calendar de postare și mix de formate
Postează consistent: minim 1-3 tweet-uri pe zi pentru a rămâne în algoritmul de recomandare "For You". Mix de formate: Tweet simplu cu hook puternic (sub 280 caractere, primul rând este hook), Thread (10-25 tweet-uri pentru conținut educativ aprofundat), Reply-uri la conturi mari din nișă (cel mai subestimat canal de creștere pe X), Quote Tweet cu opinie sau comentariu adăugat. Postează dimineața (6-9 AM) sau seara (6-9 PM) pentru engagement maxim.

### Pas 4: Strategia thread-urilor pentru reach viral
Thread-urile sunt cel mai puternic format de conținut pe X pentru reach și followeri noi. Structura unui thread cu performanță ridicată: Tweet 1 (hook) — promisiune specifică ("Iată cum am generat 50K EUR fără reclame plătite. Thread:"), Tweet 2-8 — conținut de valoare numerotate (1/, 2/, etc.), Tweet final — CTA (follow, link, reply). Adaugă o imagine sau grafic la tweet-ul cu hook pentru a crește CTR-ul în feed. Publică thread-ul ca un reply la tine însuți pentru a păstra structura.

### Pas 5: Strategie de creștere prin engagement și networking
Creșterea pe X în 2025-2026 vine în principal din engagement-ul cu conturi mai mari. Implementează "Power Hour": 30-60 minute zilnic de reply-uri de calitate la tweet-urile din top 20 conturi din nișa ta (replies educative, nu "Great post!"). Participă activ la Spaces (audio live) din nișa ta ca speaker sau participant activ. Colaborează cu conturi de dimensiune similară pentru Quote Tweet mutual sau thread-uri colaborative.

### Pas 6: Utilizare X Analytics și optimizare conținut
Activează X Analytics (analytics.twitter.com) și urmărește săptămânal: Impressions, Engagements, Profile Visits, New Followers, Top Tweet-uri. Identifică pattern-urile tweet-urilor cu engagement ridicat: ce format, ce subiect, ce oră de postare. Testează variante de hook-uri pentru același conținut — hook-ul este 80% din performanța unui tweet. Republica sau "pin-ează" cele mai performante tweet-uri sau thread-uri la intervalul de 3-6 luni.

### Pas 7: Conversie audiență X în leads și clienți
X este o platformă de top-of-funnel — conversia se face prin: link în bio cu Landing Page de captare emailuri (oferă un Lead Magnet gratuit relevant pentru nișă), CTA în thread-urile de valoare ("Dacă vrei versiunea extinsă ca PDF, reply cu 'DA' și ți-o trimit"), X Premium Subscriptions sau Super Follows pentru conținut premium. Configurează UTM links pentru toate linkurile din X pentru a urmări traficul în Google Analytics. Target: 1-3% din profilul vizitat → subscribe la lista de email.

---

## 3. Cortex Logging

```json
{
  "text": "Strategie Twitter/X implementată — profil optimizat, nișă și voce definite, calendar de postare activ, format thread-uri configurat, strategie de engagement cu conturi mari, conversie audiență în leads.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-041",
    "domain": "social-media-marketing",
    "rule_id": "META-H-002",
    "tags": ["twitter", "X", "twitter-marketing", "threads", "thought-leadership", "organic-growth"],
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
- Conținut fără nișă clară definită (postare despre subiecte disparate fără legătură) → violation
- Thread-uri publicate fără hook distinct în primul tweet → violation
- X Analytics ignorat (fără analiză lunară a performanței) → violation
- Strategie de engagement (replies la conturi mari) absentă → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum social-media-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Nișă și 3-4 piloni de conținut definiți explicit?
- [ ] Calendar de postare cu minim 1 thread pe săptămână planificat?

**VK-uri obligatorii:**
1. `✅ [PROC] M-041 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Twitter/X Marketing Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Twitter/X Account (Business sau Personal) | Contul principal de marketing |
| X Analytics (analytics.twitter.com) | Tracking performanță tweet-uri și audiență |
| X Premium (opțional) | Editing, reach prioritar, monetizare |
| Landing Page cu Lead Magnet | Conversie follower X → subscriber email |
| Thread scheduling tools (Typefully, Hypefury) | Planificare și publicare thread-uri |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Impressions medii per tweet | > 500 pentru conturi sub 5K followeri |
| Engagement Rate | > 2% |
| Creștere followeri lunară | > 5% net |
| Profile Visits → Link Clicks | > 3% rată conversie |
| Thread-uri publicate lunar | Minim 4 (1 pe săptămână) |
