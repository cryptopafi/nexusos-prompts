---
type: procedure
created: 2026-03-20
status: active
slug: m-112-commission-negotiation
tags: [procedure, nexus]
---

# M-112 — Affiliate Commission Negotiation and Backend Optimization

**Domeniu**: affiliate | **Sub-domeniu**: commission-negotiation
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Negocierea sistematica a comisioanelor mai mari cu affiliate managers dupa dovada conversiilor, plus optimizarea veniturilor din backend commissions, oferte exclusive, si structuri hibride CPA+RevShare. Majoritatea afiliatilor accepta comisioanele standard — pierzand 20-50% din revenue-ul potential. Aceasta procedura transforma relatia cu AM din tranzactionala in parteneriat strategic cu payout-uri premium.

**SMSads Context**: Pe MarathonBet Bolivia (3SNET), RevShare standard = 30% brut → 15% net dupa VOX split. Cu negociere bazata pe dovada calitatii traficului SMS (LTV jucatori > media), payout-ul poate creste la 35-40% brut → 17.5-20% net. Diferenta de 5% pe 1000 FTDs/an = $15,000+ revenue suplimentar fara cost aditional. Similar, AvaTrade pe MaxBounty: CPA standard $250 → bump la $300-350 la 50+ conversii/luna.

**Ce se intampla fara procedura:**
- Comisioane standard acceptate permanent → pierdere 20-50% din revenue potential
- Backend commissions neactivate → pierdere 30-60% din revenue total posibil
- Zero leverage in negociere → AM-ul nu are motiv sa ofere termeni premium
- Acorduri verbale nescriere → termeni disputabili, pierdere financiara

---

## Pasi

### Pas 1: ACUMULARE DATE PERFORMANTA 30+ ZILE

Trackeza metricile pe minim 30 zile consecutive:
- **Daily conversions**: trend crescator demonstrabil
- **Weekly revenue**: total si per oferta
- **EPC real** (din Voluum/Binom, nu din network dashboard)
- **CR real**: per sursa de trafic, per GEO
- **Refund/chargeback rate**: sub 5% = argument puternic
- **Traffic quality indicators**: session duration, bounce rate, FTD ratio (gambling)

Creaza dashboard in Google Sheets cu: data, conversii, revenue, EPC, CR, refund rate. Grafice trend pe 30 zile.

**Eroare frecventa**: Negociere dupa 7 zile de date. AM-ul nu ia in serios volume pe 1 saptamana — poate fi spike temporar. 30+ zile = consistenta dovedita.

### Pas 2: IDENTIFICARE LEVERAGE POINTS

Analizeaza unde performezi PESTE medie:

| Metrica Ta | vs Network Average | Leverage |
|-----------|-------------------|----------|
| EPC $1.20 | Network avg $0.80 | "+50% peste media voastra" |
| CR 0.5% | Network avg 0.3% | "Trafic de calitate superioara" |
| Refund rate 2% | Network avg 8% | "Zero issues de calitate" |
| Volume 50+ conv/zi | Top 20% afiliati | "Volume consistent, scalabil" |
| LTV jucatori (gambling) | Peste media operator | "Jucatori de calitate, depun recurent" |

**Regula**: daca performezi +20% peste media retelei pe ORICE metrica, ai leverage solid.

### Pas 3: SCHEDULE CALL CU AFFILIATE MANAGER

Programeaza call (NU email pentru negociere):
- **Timing**: dupa o luna record sau milestone (100, 500, 1000 conversii)
- **Framing**: "Performance review" meeting, nu "I want more money"
- **Durata**: 15-20 minute (respecta timpul AM-ului)
- **Pregatire**: negotiation brief (Pas 4) printat/pe ecran

**Eroare frecventa**: Cerere de bump payout prin email. Email-ul e ignorat sau primit un raspuns generic "not possible at this time". Call-ul permite negociere dinamica si demonstreaza profesionalism.

### Pas 4: PREGATIRE NEGOTIATION BRIEF (DOCUMENT INTERN)

Structura briefului:

```
NEGOTIATION BRIEF — [Oferta] pe [Network]

CURRENT STATUS:
- Comision curent: [X CPA / Y% RevShare]
- Volume meu: [Z conversii/zi, W revenue/luna]
- Durata relatie: [N luni]

MY PERFORMANCE:
- EPC: $X (vs network avg $Y → +Z%)
- CR: X% (vs network avg Y%)
- Refund rate: X% (vs network avg Y%)
- Quality score: [daca disponibil]

ASK:
- Payout bump: de la $X la $Y (justificare: +Z%)
- SAU: RevShare de la X% la Y%
- SAU: Hybrid CPA + RevShare

BACKUP ASK (daca primary refuzat):
- Performance bonus lunar la threshold
- Acces la oferte exclusive
- Payout accelerat (weekly in loc de NET30)
- Cap de volum ridicat

PROJECTED IMPACT:
- La volumul meu curent, bump-ul = +$X/luna revenue
- Cu scaling planificat: +$Y/luna in 90 zile
```

### Pas 5: EXECUTIE CALL — FRAMEWORK DE NEGOCIERE

Structura call-ului:
1. **Small talk** (2 min): "How's the network doing? Any new offers for [GEO]?"
2. **Value presentation** (5 min): "Here's what I've been doing..." — prezinta datele din brief
3. **Ask** (2 min): "Based on these numbers, I'd like to discuss a bump to $[X]"
4. **Handle objections** (5 min): raspunsuri pregatite (vezi mai jos)
5. **Close** (2 min): "Great, can you confirm this by email?"

**Obiectii frecvente si raspunsuri:**

| Obiectie AM | Raspuns |
|------------|---------|
| "I need to check with the advertiser" | "Of course. When can I expect an answer? I'm planning my Q2 budget." |
| "We can't go above $X" | "I understand. What about a performance bonus at [threshold]? Or exclusive offers?" |
| "Your volume isn't high enough" | "I'm at X now and scaling. Can we set a tiered structure? $X at current volume, $Y at [double]?" |
| "Other affiliates get the same rate" | "I appreciate that. My refund rate is X% vs average Y%. Quality traffic should be rewarded." |

### Pas 6: NEGOCIERE ALTERNATIVE DACA REFUZ DIRECT

Daca bump-ul direct e refuzat, negociaza alternative cu valoare echivalenta:

| Alternativa | Valoare Estimata |
|------------|-----------------|
| Performance bonus lunar ($X la 100+ conv) | +10-20% revenue |
| Payout accelerat (weekly vs NET30) | Cash flow improvement |
| Acces oferte exclusive (nu in dashboard public) | Higher EPC pe oferte premium |
| Priority support (AM dedicat, raspuns <4h) | Operational value |
| Tiered payout ($X la 50/zi, $Y la 100/zi) | Scaling incentive |
| Cap de volum ridicat | Revenue potential growth |

### Pas 7: INVESTIGARE SI ACTIVARE BACKEND COMMISSIONS

Backend = revenue din actiunile ulterioare ale user-ului convertit:

**Per vertical:**
| Vertical | Backend Type | Revenue Potential |
|----------|-------------|------------------|
| Gambling | RevShare pe deposits ulterioare | 2-6x front-end CPA |
| Nutra | Auto-ship/subscription recurring | +30-50% per customer |
| SaaS | Monthly recurring commission | Indefinit (cat timp plateste) |
| Finance | Tiered commission (loan amount) | Variable, poate fi >CPA |
| Trading | RevShare pe trading volume | Long-term, scalabil |

**Intrebari obligatorii pentru AM:**
1. "Do you offer backend/rebill commissions on this offer?"
2. "What's the average LTV of a converted user?"
3. "Can I get RevShare on top of CPA? (hybrid structure)"
4. "Are there upsell funnels I get credited for?"
5. "What tracking is needed for backend attribution?"

### Pas 8: NEGOCIERE OFERTE EXCLUSIVE

Oferte exclusive = avantaj competitiv maxim:
- **Landing pages unice**: pre-approved, optimizate pentru traficul tau
- **Creative-uri custom**: designate de vendor special pentru audienta ta
- **Payout custom**: mai mare decat standard, exclusiv pentru tine
- **Cap exclusion**: oferta nu e limitata la cap-ul standard

**Cum obtii exclusivitate**: dupa 3+ luni de volume consistent + relatie buna cu AM + calitate trafic dovedita. Propune: "I'd like to explore an exclusive deal. I can guarantee [X] conversions/month in exchange for [better terms]."

### Pas 9: DOCUMENTARE IN SCRIS (ZERO ACORDURI VERBALE)

Post-call, trimite email de confirmare:

```
Subject: Confirming our payout discussion — [Offer Name]

Hi [AM Name],

Per our call today, here's what we agreed:

1. Payout increase from $X to $Y, effective [date]
2. [Alternative terms agreed, if any]
3. [Backend commission activation, if discussed]

Please confirm these terms by reply.

Thanks for the partnership!
[Name]
```

**Regula**: ZERO acorduri raman doar verbale. Emailul = dovada executabila. Pastreaza TOATE email-urile de confirmare in folder dedicat.

### Pas 10: REVIEW SI RENEGOCIERE LA 90 ZILE

Ciclu continuu:
- **Calendar reminder**: 90 zile de la ultima negociere
- **Update brief**: date noi, volume noi, performance actualizata
- **Pattern de crestere**: fiecare renegociere → +10-15% increment
- **Cap payout**: fiecare oferta are un payout maxim — cand atingi cap-ul, negotieaza pe alte dimensiuni (backend, exclusive, payment terms)
- **Documentare**: fiecare negociere → entry in `commission-negotiation-log.md`

---

## Verificare

- [ ] 30+ zile de date de performanta documentate cu trend?
- [ ] Negotiation brief pregatit si complet?
- [ ] Call cu AM realizat (nu doar email)?
- [ ] Acorduri confirmate in scris via email?
- [ ] Backend commissions investigate si activat (unde disponibil)?
- [ ] Alternative negociate (daca bump direct refuzat)?
- [ ] Oferte exclusive discutate?
- [ ] Review la 90 zile programat in calendar?
- [ ] Negotiation log actualizat?

---

## Instrumente

| Tool | Rol |
|------|-----|
| Google Sheets | Dashboard performanta + negotiation brief |
| Voluum / Binom | Date EPC/CR reale |
| Email client | Documentare acorduri in scris |
| Calendar | Reminder renegociere 90 zile |
| Network dashboards | Verificare payout actualizat post-negociere |

---

## Note

- Depinde de M-007 (oferte CPA active) + M-008 (tracking data pe 30+ zile).
- Nu negocia inainte de a avea date solide — risc de pierdere credibilitate cu AM.
- M-128 (LTV Cohort Analysis) furnizeaza date de calitate jucator pentru negociere gambling RevShare.
- Negocierea e un skill — primele 2-3 tentative pot fi awkward. Se imbunatateste cu practica.
- Revenue impact anual al negocierii: +15-30% din total revenue fara cost aditional.
