---
type: procedure
created: 2026-03-22
status: active
slug: c-036-crypto-twitter-x-strategy
tags: [procedure, nexus]
---

# Crypto Twitter/X Marketing Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Strategie completă de marketing pe Twitter/X pentru un proiect crypto, de la optimizarea profilului până la crearea de conținut viral și building comunitate.

---

## 1. Problema

Twitter/X (Crypto Twitter — CT) este cel mai important canal de distribuție și descoperire pentru proiectele crypto. Un profil neoptimizat, o strategie de conținut absentă, sau engagement superficial duc la un cont invizibil, indiferent de calitatea proiectului. Algoritmul X penalizează inactivitatea și recompensează consistența.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Crearea și optimizarea contului de Twitter/X pentru un token nou
- Strategie de conținut pe termen lung pentru building comunitate
- Campanie de growth agresiv pre și post lansare token

---

## 2. Procedura

### Pas 1: Optimizează profilul Twitter/X
Bio trebuie să includă: ce face proiectul în max 160 caractere, adresa contractului (sau link Linktree), website, chain/DEX. Header image cu branding proiect. Profile picture — logo proiect (sau founder face pentru conturi personale). Pinned tweet: announcement principal sau thread de prezentare proiect.

### Pas 2: Definește strategia de conținut (content pillars)
Stabilește 3-4 piloni de conținut: (1) Educativ — DeFi, tokenomics explicat, how-to; (2) Community — polls, Q&A, shoutouts; (3) Updates proiect — milestones, parteneriate, roadmap; (4) Alpha/insights — analize de piață relevante pentru nișa ta. Planifică un mix săptămânal.

### Pas 3: Creează calendar editorial și programare
Planifică 1-2 postări/zi minimum. Zilele și orele optime pentru CT: 13:00-17:00 UTC. Folosește TweetDeck sau Buffer pentru programare. Creează un backlog de 20+ postări în avans. Setează reminder pentru postări manuale în timp real (lansări, market events).

### Pas 4: Execută threaduri educative (thread strategy)
Thread-urile sunt cel mai viral format pe CT. Structură: Tweet 1 = hook puternic (problemă sau stat surprinzător), Tweet 2-7 = conținut de valoare, Tweet 8 = CTA. Subiect pentru thread: "Ce este [TOKEN]?", "Cum funcționează [PROTOCOL]?", "De ce am construit [PROIECT]". Postează 2-3 threads/săptămână.

### Pas 5: Activează engagement și networking
Interacționează zilnic cu KOL-uri din nișa ta: comentarii de calitate (nu doar "great post!"). Reply la tweeturi relevante despre tehnologia sau nișa ta. Retweet cu adăugare de context (quote tweet). Follow și engage cu proiecte complementare — fac follow-back frecvent.

### Pas 6: Monitorizează analytics și ajustează
Verifică săptămânal în Twitter Analytics: impressii, engagements, profile visits, follower growth. Identifică ce tipuri de conținut performează cel mai bine (threads vs single tweets, educativ vs update). Ajustează content mix-ul în favoarea formatelor cu engagement ridicat.

### Pas 7: Documentează strategia validată și salvează în Cortex
La fiecare lună, compilează: follower growth (%), engagement rate mediu, tweeturile cu performanță cea mai bună, și next steps. Documentează ce a funcționat și ce nu. Actualizează calendarul editorial bazat pe insights. Salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Strategie Twitter/X crypto completă executată. Profil optimizat, calendar editorial creat, thread strategy activată, analytics monitorizate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-036",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "twitter", "social-media", "community", "token-launch", "content-strategy"],
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
- Profil Twitter fără bio complet și contract address → violation
- Niciun calendar editorial creat → violation
- Analytics neveificate săptămânal → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Profil optimizat cu toate elementele obligatorii?
- [ ] Calendar editorial cu minim 20 postări în backlog?

**VK-uri obligatorii:**
1. `✅ [PROC] C-036 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Crypto Twitter/X Marketing Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Twitter/X Analytics | Monitorizare performanță |
| TweetDeck / Buffer | Programare postări |
| Canva | Grafice și bannere tweet |
| LunarCrush | Monitorizare sentiment crypto CT |
| Linktree | Centralizare link-uri în bio |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Frecvență postare | Minim 1/zi |
| Engagement rate | Minim 2% |
| Follower growth lunar | Documentat și comparat |
| Thread-uri publicate/săptămână | Minim 2 |
| Analytics review | Săptămânal obligatoriu |
