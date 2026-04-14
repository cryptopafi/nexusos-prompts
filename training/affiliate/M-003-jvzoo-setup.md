---
type: procedure
created: 2026-03-20
status: active
slug: m-003-jvzoo-setup
tags: [procedure, nexus]
---

# M-003 — JVZoo Account Setup and Product Research

**Domeniu**: affiliate | **Sub-domeniu**: jvzoo-setup
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Configurarea profesionala a unui cont JVZoo si identificarea produselor digitale profitabile pentru promovare, cu filtrare anti-scam, scoring multi-criterial, si setup de tracking integrat cu Voluum/BeMob. JVZoo ramane relevant ca sursa de comisioane 50-100% pe produse digitale — ideal pentru SMS traffic unde user-ul primeste mesajul si decide in 30 secunde.

**SMSads Context**: Produsele digitale JVZoo cu comisioane 50-100% pot fi promovate via SMS catre segmentele Technology (693K BO), Software (523K BO), si Apps (457K BO). CPA efectiv per vanzare: $15-50 pe produse IM/software. Avantaj fata de gambling: zero restrictii de licenta, zero KYC, conversie in 1 click. Dezavantaj: payout-uri mai mici, refund rate 5-15%.

**Ce se intampla fara procedura:**
- Cont JVZoo configurat fara payment method → comisioane se acumuleaza dar nu se platesc
- Produse selectate pe gravity manipulat → vanzari cu 15-30% refund rate
- Vendori noi fara track record → risc de produs abandonat sau scam
- Link-uri affiliate fara tracking → zero date de optimizare pe campanii SMS
- Materiale promotionale generice copiate verbatim → ban pe surse de trafic

---

## Pasi

### Pas 1: CREARE CONT JVZOO CU CONFIGURARE COMPLETA

Creaza cont pe jvzoo.com cu date reale (obligatoriu pentru payouts). Configuratie completa: nume legal, adresa fizica, metoda de plata (PayPal Business sau Payoneer — PayPal Personal are limite de $10K/luna), informatii fiscale (W-8BEN pentru non-US, W-9 pentru US). Activeaza 2FA cu authenticator app (nu SMS — riscuri SIM swap). Seteaza timezone corect — afecteaza raportarea zilnica a vanzarilor.

**Eroare frecventa**: Folosirea PayPal personal in loc de PayPal Business. JVZoo retine comisioanele daca PayPal marcheaza tranzactia ca suspecta pe contul personal. PayPal Business suporta volume fara problema.

### Pas 2: CONFIGURARE PROFIL AFFILIATE PROFESIONAL

Completeaza profilul de afiliat: bio profesionala (2-3 randuri despre experienta ta in marketing), website/blog URL (obligatoriu — vendorii verifica), surse de trafic (email, social, SMS — fii specific), nise de interes. Profilul complet creste rata de aprobare la produsele premium cu acces restrictionat. Upload avatar profesional — profilurile cu avatar au 2x rata de aprobare vs cele fara.

### Pas 3: NAVIGARE MARKETPLACE CU FILTRE AVANSATE

Navigheaza la marketplace si seteaza filtre strategice:
- **Commission rate**: minim 50% (sub 50% nu justifica efortul de promovare SMS)
- **Gravity**: 10-200 (sub 10 = cerere nevalidata, peste 200 = saturatie competitiva extrema)
- **Launch date**: ultimele 90 zile pentru produse fresh + all-time cu gravity stabil pentru evergreen
- **Categorii prioritare SMS**: Internet Marketing, Software, E-Business (audienta tech-savvy compatibila cu segmentele IAB Technology/Software/Apps)
- **Recurring**: DA (preferinta pentru comisioane lunare recurente — SaaS tools, membership sites)

Exporta lista de 20-30 candidati intr-un spreadsheet cu coloane: Product Name, Vendor, Gravity, Commission %, Price, Refund Rate, EPC, Category, Launch Date.

### Pas 4: SCORING MULTI-CRITERIAL PRODUSE (7 DIMENSIUNI)

Scoreaza fiecare produs candidat pe 7 criterii:

| Criteriu | Pondere | Scor 1 | Scor 5 | Scor 10 |
|---------|---------|--------|--------|---------|
| Gravity (cerere dovedita) | 20% | <10 | 30-80 | 80-200 |
| Commission % | 15% | <30% | 50-75% | 75-100% |
| Refund Rate | 20% | >15% | 5-10% | <5% |
| Vendor Reputation | 15% | Nou, 0 produse | 3-5 produse | 10+ produse, rating 4.5+ |
| EPC (Earnings Per Click) | 15% | <$0.30 | $0.50-1.00 | >$1.50 |
| Recurring Revenue | 10% | One-time only | Optional upsell | Monthly subscription |
| SMS Compatibility | 5% | Necesita demo lung | Landing page OK | 1-click buy, mobile-first |

**Scor minim pentru selectie**: 55/100. Sub 55 = SKIP.

**Eroare frecventa**: Selectie pe commission % maxim fara verificarea refund rate. Produs cu 100% comision dar 20% refund rate = 80% comision efectiv + risc de shaving vendor.

### Pas 5: VERIFICARE ANTI-SCAM (OBLIGATORIE, ZERO DEROGARI)

Pentru fiecare produs cu scor >55, verifica TOATE semnalele:
- **Promisiuni venituri**: "$10K/day guaranteed", "make money while you sleep" = RED FLAG
- **Product demo**: lipsa preview/demo = RED FLAG (vendorul ascunde calitatea)
- **Vendor track record**: profil cu 0 produse anterioare = CAUTION (posibil hit-and-run)
- **Gravity manipulation**: spike brusc (0→100 in 7 zile) urmat de drop = launch coordonat artificializat
- **Sales page red flags**: countdown fake (se reseteaza la refresh), income screenshots needitate, testimoniale generic stock photo
- **Support**: lipsa email de support real sau doar "contact form" fara raspuns = RED FLAG
- **WarriorForum/STM check**: cauta "[product name] review scam" pe Google — dacă exista reviews negative multiple = AVOID

**Regula**: Un singur RED FLAG = EXCLUDE din lista. Doua CAUTION = EXCLUDE. Zero toleranta pe scam — un produs scam promovat pe SMS arde numarul de telefon (carrier block).

### Pas 6: REQUEST APROBARE AFFILIATE CU MESAJ PROFESIONAL

Solicita aprobare pentru top 7-10 produse care au trecut screening-ul. Unele produse necesita aprobare manuala — mesaj profesional catre vendor:

```
Subject: Affiliate Application — [Product Name]

Hi [Vendor Name],

I'm an affiliate marketer with experience in [email/SMS/social] traffic.
I'm interested in promoting [Product Name] to my [audience size] subscribers
in the [niche] space.

My promotion plan:
- [Traffic source]: targeted campaigns to [audience description]
- Expected volume: [X] clicks/day from qualified traffic
- I have tracking set up with Voluum for accurate attribution

Happy to discuss further. Looking forward to working together.

Best, [Name]
```

Aplica la mai multe produse simultan — rata medie de aprobare e 40-60%, deci din 10 aplicatii primesti 4-6 aprobari.

### Pas 7: AUDIT MATERIALE PROMOTIONALE SI CREARE CUSTOM

Descarca materialele promotionale furnizate de vendori (swipe emails, banners, ad copy). Evalueaza calitatea:
- **Swipes profesionale**: adapteaza cu vocea ta, nu copia verbatim (filtele spam detecteaza copy identic)
- **Banners de calitate**: redimensioneaza pentru mobile (300x250 si 320x50 — formatele SMS landing page)
- **Swipes generice/spam**: IGNORA si creaza propriile materiale — headline custom + 3 beneficii + CTA

**Pentru SMS traffic**: creaza mesaj scurt (160 caractere) cu: hook (problema/beneficiu), brand product, CTA + link tracker. Exemplu: "Struggling with [problem]? [Product] helped 50K+ people [benefit]. Try free: [tracking link]"

### Pas 8: SETUP LINK-URI AFFILIATE CU TRACKING VOLUUM/BEMOB

Configureaza link-urile affiliate cu tracking:
1. In JVZoo: copiaza affiliate link-ul per produs (contine affiliate ID)
2. In Voluum/BeMob: creaza campanie → adauga JVZoo ca "Affiliate Network" (postback manual)
3. Seteaza SubID parameters: s1=traffic_source (sms/email/social), s2=creative_id, s3=product_id, s4=geo
4. Genereaza tracking URL din Voluum → acest URL inlocuieste link-ul JVZoo direct
5. Test click → verifica in tracker ca click-ul apare → simuleaza conversie test (cerere la vendor sau test purchase cu refund)

**Eroare frecventa**: Folosirea link-ului JVZoo direct in SMS fara tracker. Zero date de optimizare = zero scaling posibil. MEREU tracker intre SMS si oferta.

### Pas 9: MONITORIZARE PERFORMANTA SI ROTATIE PRODUSE LUNARA

Setup monitorizare:
- **Dashboard JVZoo zilnic**: vanzari, refund-uri, comisioane pending, comisioane platite
- **Voluum/BeMob**: clicks, CR per produs, CR per sursa de trafic, EPC real vs EPC estimat
- **Review lunar (prima zi a lunii)**: re-evalueaza top 5 produse — gravity trend, refund rate trend, EPC trend
- **Kill criteria**: refund rate >15% pe 30 zile → STOP promovarea. EPC real < 50% din EPC afisat → STOP.
- **Rotatie**: inlocuieste produsele killed cu urmatoarele din pipeline (scoring Pas 4)
- **Evergreen winners**: produse cu EPC stabil 3+ luni si refund <5% → muta in "always-on" rotation

---

## Verificare

- [ ] Cont JVZoo configurat complet (payment method activa, 2FA, profil complet)?
- [ ] Minim 20 produse evaluate cu scoring 7 dimensiuni?
- [ ] Anti-scam check completat pentru toate produsele selectate (zero RED FLAGS)?
- [ ] Minim 7 cereri de aprobare trimise cu mesaj profesional?
- [ ] Materiale promotionale custom create (nu copiate verbatim)?
- [ ] Link-uri affiliate configurate cu tracking Voluum/BeMob?
- [ ] Test click validat in tracker?
- [ ] Plan de monitorizare si rotatie lunara documentat?

---

## Instrumente

| Tool | Rol |
|------|-----|
| JVZoo Marketplace | Sursa produse + aprobare affiliate |
| Voluum / BeMob | Tracking clicks si conversii |
| Google Sheets | Spreadsheet scoring produse |
| PayPal Business / Payoneer | Payment method pentru comisioane |
| WarriorForum / STM Forum | Verificare reputatie produs/vendor |
| Google Search | Anti-scam check ("[product] review scam") |

---

## Note

- Depinde de M-001 (nisa validata determina categoriile de search pe JVZoo).
- JVZoo este complementar CPA networks (M-006/M-007) — produsele digitale au CPA mai mic ($15-50) dar zero restrictii de compliance.
- Re-evaluare portofoliu: lunar obligatoriu, saptamanal recomandat pe produsele noi.
- Gravity JVZoo NU e identic cu ClickBank gravity — JVZoo-ul e calculat diferit, nu compara direct.
- Comisioanele JVZoo sunt procesate instant (PayPal) sau NET30 (Payoneer/wire) — verifica per vendor.
