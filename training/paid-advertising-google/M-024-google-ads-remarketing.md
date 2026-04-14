---
type: procedure
created: 2026-03-22
status: active
slug: m-024-google-ads-remarketing
tags: [procedure, nexus]
---

# Google Ads Remarketing Campaign — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea și lansarea campaniilor de remarketing Google Ads pentru reactivarea vizitatorilor existenți și creșterea ratei de conversie.

---

## 1. Problema

Peste 95% din vizitatorii unui site pleacă fără să convertească. Fără campanii de remarketing, acel trafic — deja plătit — este pierdut definitiv. Campaniile de remarketing targetează utilizatorii care au interacționat deja cu business-ul, generând conversii la un CPA semnificativ mai mic față de campaniile de prospecting. Această procedură standardizează setup-ul corect al audientelor și campaniilor de remarketing.

Situații acoperite:
- Setup prima campanie de remarketing pentru un cont fără audiențe configurate
- Segmentarea audientelor de remarketing pe baza comportamentului pe site
- Remarketing dinamic pentru site-uri e-commerce cu catalog de produse

---

## 2. Procedura

### Pas 1: Verifică și configurează Google Ads Remarketing Tag
Confirmă că Google Ads Remarketing Tag (sau Google Tag cu remarketing activat) este instalat pe toate paginile site-ului. Dacă folosești GTM, verifică că tag-ul de tip "Google Ads Remarketing" este activ și publicate schimbările. Audiențele încep să se populeze doar după ce tag-ul este activ — nu poți crea audiențe retroactiv.

### Pas 2: Creează segmentele de audiențe (minimum 5 segmente)
Construiește audiențe bazate pe comportament: (1) Toți vizitatorii site — ultimele 30 zile, (2) Vizitatori pagini de produs/serviciu, (3) Utilizatori care au adăugat în coș dar nu au cumpărat (pentru e-commerce), (4) Vizitatori pagini cheie (pricing, contact), (5) Converteri existenți (pentru upsell sau excludere). Setează membership duration la 30 zile pentru cold audiences și 90 zile pentru high-intent.

### Pas 3: Combină audiențe în segmente mai precise (Audience Combinations)
Creează segmente combinate pentru precizie: "Cart Abandoners" = a vizitat /cart SAU /checkout DAR NU a vizitat /thank-you. "High Intent Non-Converters" = a vizitat pagina de produs de minimum 2 ori. Exclude întotdeauna convertorii existenți din campaniile de remarketing pentru produse deja achiziționate.

### Pas 4: Creează campania de remarketing și alege rețeaua
Pentru remarketing Search (RLSA): adaugă audiențele la campaniile Search existente cu bid adjustments +20-50% și lărgește match types. Pentru remarketing Display: creează o campanie separată de tip Display cu targetare pe audiențe. Pentru remarketing YouTube: campanie Video separată. Nu mixa RLSA cu Display Remarketing în aceeași campanie.

### Pas 5: Creează anunțuri specifice de remarketing (mesaj personalizat)
Anunțurile de remarketing trebuie să fie diferite față de anunțurile de prospecting. Include: referință la vizita anterioară ("Ai văzut [produs]?"), urgență/incentiv ("Ofertă specială pentru tine"), social proof sau testimonial. Pentru Display Remarketing, creează minimum 3 dimensiuni de banner: 300x250, 728x90, 160x600. Activează Dynamic Remarketing dacă ai Google Merchant Center conectat.

### Pas 6: Configurează frequency capping și bid strategy
Setează frequency cap la maximum 3-5 impresii/utilizator/zi pentru Display Remarketing (fără cap, campaniile devin deranjante și dăunează brandului). Pentru RLSA, aplică bid adjustment pozitiv pe audiențele cu intenție ridicată. Bid strategy recomandată: Target CPA pentru audiențe cu minimum 30 de conversii/lună sau Maximize Conversions altfel.

### Pas 7: Activează și setează monitoring la 24h, 7 zile și 30 zile
Verifică după 24h că audiențele au minimum 100 de utilizatori activi (altfel campania nu va rula). Monitorizează la 7 zile: impresii, CTR, CPA versus campania de prospecting. Obiectiv: CPA remarketing = maximum 60% din CPA prospecting. Documentează în Cortex: audiențele create, dimensiunile, performanța la 30 zile.

---

## 3. Cortex Logging

```json
{
  "text": "M-024 executat: Campanie Remarketing Google Ads configurată. Audiențe create: [N] - [tipuri]. Tip campanie: [RLSA/Display/Video]. Frequency cap: [N/zi]. Dynamic remarketing: DA/NU. Audiențe populate (100+ users): DA/NU. CPA remarketing vs prospecting: [%].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-024",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "remarketing", "rlsa", "display-remarketing", "audience-segments", "dynamic-remarketing", "cart-abandoners"],
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
- Campanie de remarketing lansată cu audiențe sub 100 utilizatori → violation
- Frequency cap nesetat pentru campaniile Display Remarketing → violation
- Convertorii existenți neexcluși din campanii de remarketing pentru produse deja achiziționate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 5 segmente de audiențe create?
- [ ] Convertorii excluși din campaniile de remarketing relevante?

**VK-uri obligatorii:**
1. `✅ [PROC] M-024 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Ads Remarketing Campaign" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Ads Account | Platformă principală |
| Google Remarketing Tag / GTM | Popularea audiențelor |
| Google Merchant Center | Dynamic remarketing pentru e-commerce |
| Conversion Tracking (M-022) | Excluderea converterilor și optimizare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CPA remarketing vs. prospecting | Maximum 60% din CPA prospecting |
| Audience size la lansare | Minimum 100 utilizatori per segment |
| Frequency cap Display | Maximum 5 impresii/utilizator/zi |
