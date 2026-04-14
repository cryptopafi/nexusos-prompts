---
type: procedure
created: 2026-03-22
status: active
slug: c-035-reddit-token-marketing
tags: [procedure, nexus]
---

# Reddit Marketing for Your Token — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Strategie structurată pentru marketing organic și plătit pe Reddit pentru promovarea unui token crypto, cu respectarea regulilor subredditurilor.

---

## 1. Problema

Reddit este una dintre cele mai influente comunități crypto, dar și una dintre cele mai sensibile la spam și shilling. Postările care nu respectă regulile subredditurilor duc la ban permanent al contului, iar tentativele de manipulare sunt rapid identificate și amplifică negativ reputația proiectului.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Post de lansare/anunț pe subreddituri relevante crypto
- Strategie de engagement organic pe termen lung în comunitate
- Gestionarea unui subreddit propriu al proiectului

---

## 2. Procedura

### Pas 1: Identifică și studiază subredditurile țintă
Lista principală: r/CryptoCurrency, r/DeFi, r/ethereum, r/solana, r/altcoin, și subredditurile specifice chain-ului tokenului. Citește cu atenție regulile fiecărui subreddit (sidebar + pinned posts). Identifică zilele și orele cu engagement maxim. Documentează regulile relevante și restricțiile de self-promotion.

### Pas 2: Creează sau maturizează contul Reddit
Un cont nou fără karma va fi filtrat automat de subredditurile mari. Dacă contul este nou, construiește karma timp de 2-4 săptămâni prin contribuții genuine înainte de a posta despre proiect. Alternativ, folosește un cont cu karma existentă. Nu crea conturi false multiple — este ban permanent.

### Pas 3: Creează conținut de valoare, nu shill pur
Formatele care funcționează pe Reddit: "I built X — here's what I learned" (educativ), comparații obiective, tutoriale tehnice, analize DeFi. Evită: "Buy [TOKEN] — it will 100x", posts fără substanță, spam de link-uri. Conținutul trebuie să ofere valoare chiar dacă ignori tokenul.

### Pas 4: Planifică și scrie postul de lansare/anunț
Postul de anunț trebuie să includă: ce problemă rezolvă proiectul, cum funcționează tehnic, tokenomics transparente, echipa (chiar și anon, cu istoric), audit de securitate dacă există, și link-uri verificabile. Evită limbajul de marketing agresiv. Scrie în stil Reddit — conversațional, direct.

### Pas 5: Postează la momentul optim și monitorizează
Postează între 12:00-16:00 UTC în zilele de marți-joi (engagement maxim Reddit). Rămâi activ în comentarii primele 2-3 ore — răspunde la fiecare întrebare, inclusiv la cele sceptice. Nu șterge comentariile negative — răspunde-le civilizat. Upvote-urile organice contează.

### Pas 6: Execută strategie de engagement pe termen lung
Contribuie regulat la subredditurile țintă — nu doar când promovezi. Răspunde la întrebări despre DeFi/crypto în general. Participă la discuții relevante fără a menționa tokenul. Această prezență organică construiește credibilitate pentru postările viitoare.

### Pas 7: Monitorizează și raportează performanța
Trackuiește: upvote ratio, număr comentarii, karma câștigată, trafic pe website din Reddit (Google Analytics/UTM). Documentează ce tipuri de posturi performează cel mai bine. Salvează în Cortex strategia validată pentru utilizare ulterioară.

---

## 3. Cortex Logging

```json
{
  "text": "Campanie Reddit marketing executată. Regulile subredditurilor respectate, conținut de valoare creat, engagement monitorizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-035",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "reddit", "marketing", "community", "token-launch", "organic"],
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
- Post Reddit fără respectarea regulilor subreddit (no shill) → violation (ban risk)
- Postare cu cont nou (karma insuficientă) pe subredditure mari → violation
- Conținut fără valoare reală (shill pur fără substanță) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Regulile fiecărui subreddit citite și respectate?
- [ ] Postul conține valoare reală dincolo de self-promotion?

**VK-uri obligatorii:**
1. `✅ [PROC] C-035 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Reddit Marketing for Your Token" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Reddit Account (karma ≥ 100) | Cont pentru postare |
| Google Analytics + UTM | Tracking trafic din Reddit |
| Reddit Ads (opțional) | Campanii plătite dacă organic insuficient |
| Subreddit Rules Documentation | Referință pentru compliance |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Upvote ratio | Minim 75% |
| Subredditure postate | Minim 3 relevante |
| Comentarii răspunse (primele 3h) | 100% |
| Trafic website din Reddit (UTM) | Documentat |
| Engagement organic pe termen lung | Minim 2 contribuții/săptămână |
