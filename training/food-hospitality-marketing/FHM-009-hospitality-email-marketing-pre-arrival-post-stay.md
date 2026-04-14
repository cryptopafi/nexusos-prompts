---
id: FHM-009
title: Hospitality Email Marketing (Pre-arrival to Post-stay)
domain: food-hospitality-marketing
source: Hospitality Marketing Expert (Udemy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# FHM-009: Hospitality Email Marketing (Pre-arrival to Post-stay)

## Obiectiv
Implementa un flux complet de email marketing automatizat cu Mailchimp sau ConvertKit pentru Albastru / Sapte Focuri, acoperind intregul ciclu al oaspetelui: secventa de pre-arrival (3 emailuri), mid-stay (conditional), post-stay cu solicitare recenzie si re-booking, plus newsletter trimestrial pentru lista de oaspeti anteriori. Target: open rate > 40%, minimum 1 recenzie generata per 5 emailuri post-stay.

## Pasi

### Pasul 1 — Configurare platforma de email marketing si lista de contacte
**Platforme recomandate (comparatie):**
| Platforma | Plan gratuit | Automatizari | Integrare OTA | Cost paid |
|-----------|-------------|-------------|--------------|----------|
| Mailchimp | 500 contacte, 1000 email/luna | Da (basic) | Zapier | De la $13/luna |
| ConvertKit | 1000 subscribers | Da (avansate) | Zapier | De la $29/luna |
| Brevo (ex-Sendinblue) | 300 email/zi | Da | Direct API | De la $25/luna |

**Recomandare pentru Albastru**: Mailchimp (gratuit pana la 500 contacte — suficient pentru primii 1-2 ani de operare). Migrati la ConvertKit cand depasiti 500 si vreti automatizari mai complexe.

**Setup initial:**
1. Creati cont cu adresa de email a proprietatii (`info@albastru.ro` sau domeniu propriu — NU @gmail.com).
2. **Verificati domeniul de trimitere (CRITIC pentru livrabilitate)**:
   - Configurati SPF record in DNS: `v=spf1 include:servers.mcsv.net ~all`
   - Configurati DKIM record in DNS (Mailchimp furnizeaza recordul exact)
   - Configurati DMARC: `v=DMARC1; p=none; rua=mailto:dmarc@albastru.ro`
   - Fara SPF+DKIM, emailurile ajung in spam la 30-50% din destinatari.
3. Importati lista de oaspeti existenti (cu acordul lor GDPR — doar cei care au bifat consimtamantul).
4. Creati template-ul de email cu brandul Albastru: logo, culori (Amber #C8860A + Carbune #2C2C2C — vezi FHM-012), font (Playfair Display heading + Lato body).
5. Configurati "From name": "Albastru — Sapte Focuri" + "From email": info@albastru.ro

**Colectarea emailurilor noi (3 surse principale):**
- La rezervare directa: cerut explicit la finalizarea rezervarii (obligatoriu pentru email-uri operationale)
- La check-in: pe fisa de inregistrare, cu bifa GDPR: "Doresc sa primesc noutati de la Albastru" (optional, pentru newsletter)
- Pe website / Instagram bio: formular simplu "Ramaneti la curent" cu promovare periodica pe Stories

### Pasul 2 — Email 1: Confirmare rezervare (automat, imediat dupa rezervare)
Primul email primit de oaspete seteaza asteptarile si tonul relatiei. Trebuie sa fie cald, nu tranzactional.

**Subiect**: Rezervare confirmata — [Datele sejurului] la Sapte Focuri

**Continut:**
```
Buna [Prenume]!

Rezervarea ta pentru [datele] este confirmata. Abia asteptam sa te avem la noi!

DETALII SEJUR:
- Sosire: [data], ora [X]
- Plecare: [data], ora [X]
- Numarul de oaspeti: [X]
- Tipul de camera: [X]
- Total: [X] RON

CUM AJUNGI:
[Adresa completa + link Google Maps]
Din Bucuresti: [X] km, ~[X]h pe [ruta]. Din Pitesti: [X] km, ~[X] min.
Nota: GPS-ul poate da uneori o adresa usor diferita — urmariti indicatoarele catre [reper local] dupa [sat/intersectie].

CE TE ASTEAPTA:
La noi, fiecare masa se gateste la foc deschis, in tuciuri de fonta, exact ca acum 100 de ani. Daca ai preferinte alimentare sau alergii, spune-ne din timp si ne adaptam.

INTREBARI?
Suntem la [telefon/WhatsApp] sau [email] — raspundem in cateva ore.

Cu drag,
[Nume] & Echipa Sapte Focuri
```

**Nota tehnica**: daca rezervarea vine prin OTA (Booking.com, Airbnb), acest email NU se trimite — OTA trimite propria confirmare. Trimiteti doar pentru rezervari directe.

### Pasul 3 — Email 2: Pre-arrival (automat, 5-7 zile inainte de sejur)
Scopul: entuziasmul pre-sejur + informatii practice + upsell subtil.

**Subiect**: Weekendul tau la Sapte Focuri se apropie — tot ce trebuie sa stii

**Continut:**
```
Buna [Prenume]!

Doar [X] zile pana la Sapte Focuri! Focul e gata sa arda pentru voi.

CE TE ASTEAPTA:
Dimineata incepe cu mirosul de lemn de fag si cafeaua pe terasa. Pranzul se pregateste in tuciuri pe foc — usturoiul se caleste, fasolea clocoteste, painea se coace in cuptor. Seara, cina la lumina focului si a lumanarilor. Intre timp, linistea.

CE SA ADUCI:
- Haine comode si incaltaminte de teren (daca planificati drumetie)
- Camera foto / telefon incarcat (veti vrea sa fotografiati!)
- Pofta de mancare (serios)

MENIUL SAPTAMANII:
Pregatim [2-3 preparate speciale — ex: "tocanita de ciuperci de padure in tuci", "paine cu cartofi din cuptor", "placinta cu branza si marar"].

DORESTI SA ADAUGI CEVA SPECIAL?
- Workshop "Gatit la Foc Deschis" (2h, 150 RON/persoana) — gatesti tu in tuci, cu reteta de luat acasa
- Cina speciala de [zi] (meniu extins cu 5 feluri, 120 RON/persoana)
- [Alta activitate sezoniera]
Anunta-ne pana pe [data — 2 zile inainte] si aranjam.

DETALII SOSIRE:
- Sosire: [data], de la ora [X]
- Parcare: gratuita, in curtea proprietatii
- Contact la sosire: [telefon/WhatsApp]

Ne vedem curand!
[Semnatura]
```

### Pasul 4 — Email 3: Pre-arrival final (automat, 1 zi inainte de sosire)
Scopul: confirmare finala + anticipatie + detalii de ultim moment.

**Subiect**: Maine te asteptam la Sapte Focuri!

**Continut (SCURT — max 100 cuvinte):**
```
[Prenume], maine e ziua!

Focul o sa arda de dimineata. [Detaliu personal daca disponibil — "Am pus deoparte camera cu privelistea spre padure, cum ai cerut" sau "Am pregatit meniu fara gluten cum ai mentionat"].

Daca ai nevoie de ceva pe drum sau te pierzi, suna-ne la [telefon] — raspundem imediat.

Drum bun!
[Nume]
```

**Nota**: acest email face diferenta intre o experienta "tranzactionala" si una "personala". Personalizarea (chiar minima) creste satisfactia si probabilitatea de review.

### Pasul 5 — Email 4: Mid-stay (conditional, doar pentru sejururi 3+ nopti, trimis in ziua 2)
Scopul: satisfactie proactiva + upsell + detectare probleme devreme.

**Subiect**: Cum e la Sapte Focuri, [Prenume]?

**Continut (max 100 cuvinte):**
```
Buna [Prenume],

Speram ca va bucurati de fiecare moment la noi!

Daca este orice am putea face mai bine sau daca aveti nevoie de ceva, suntem la [telefon] oricand.

Si daca maine vreti sa ramaneti la cina noastra speciala de [zi] — [descriere scurta preparate] — mai avem [X] locuri disponibile. Spuneti-ne pana la pranz!

Ne vedem la [activitate/masa]!
[Semnatura]
```

**IMPORTANT**: acest email trebuie personalizat manual sau semi-manual. NU trimiteti un template generic unui oaspete care a reclamat ceva. Verificati la receptie/gazda inainte de trimitere.

### Pasul 6 — Email 5: Post-stay (automat, 24-48h dupa check-out)
Scopul: multumire + solicitare recenzie + teaser re-booking. **Cel mai important email din flux.**

**Subiect**: Multumim, [Prenume] — [un detaliu personal sau sezonier]

**Continut:**
```
Draga [Prenume],

Speram ca ati ajuns acasa bine si ca weekendul la Sapte Focuri v-a lasat cu amintiri frumoase!

[O referinta personalizata: "Speram ca [copilul/cainele/prietena] s-a distrat la fel de mult ca voi!" sau "Am pastrat reteta pe care v-am promis-o — o gasiti mai jos"]

DACA ATI VREA SA NE AJUTATI:
O recenzie scurta pe Google ne ajuta enorm sa fim gasiti de alte familii care cauta exact ce aveti voi: [link scurt recenzie Google]
Dureaza sub 2 minute si inseamna enorm pentru noi.

REVENITI IN CURAND:
[Hook sezonier: "Toamna la noi este... altceva. Culorile padurii, zacusca proaspata, focul care arde mai mult." sau "Iarna cand zapada acopera totul si singurele sunete sunt jarului si pasarile."]

Daca vreti sa asigurati un weekend din [luna/sezon], scrieti-ne — avem cateva date libere.

Cu drag si recunostinta,
[Nume] & Echipa Sapte Focuri
```

**Optiuni de link recenzie (in ordinea prioritatii):**
1. Link Google review (prioritar — cel mai mare impact SEO)
2. Daca au rezervat prin Booking.com/Airbnb — mentionati ca review-ul pe platforma respectiva conteaza
3. Link TripAdvisor (secundar)

### Pasul 7 — Email 6: Follow-up recenzie (conditional, 7 zile dupa email post-stay, daca NU au lasat review)
**Subiect**: O ultima rugaminte de la Sapte Focuri

**Continut (FOARTE scurt — 50 cuvinte max):**
```
[Prenume], ne bucuram ca ati fost la noi!

Daca aveti 2 minute, o recenzie pe Google ne-ar ajuta enorm: [link]

Daca nu, nu-i nimic — speram sa ne revedem!

Cu drag,
[Semnatura]
```

**Regula**: NU trimiteti mai mult de 2 solicitari de review (email post-stay + acest follow-up). Dupa aceasta, stop.

### Pasul 8 — Newsletter trimestrial (pentru lista de oaspeti anteriori)
Scopul: mentinerea relatiei + stimulare re-booking + sezonalitate.

**Frecventa**: trimestrial (Martie, Iunie, Septembrie, Decembrie)
**Segmentare**: oaspeti anteriori (au vizitat) vs. abonati noi (nu au vizitat inca) — ton usor diferit.

**Structura newsletter (400-600 cuvinte):**
1. **O poveste de la Sapte Focuri** (150 cuvinte): ce s-a intamplat la noi de la ultimul newsletter — un moment real, un preparat nou, o schimbare de sezon, o intamplare cu un oaspete (cu acord).
2. **Preparatul sezonier** (100 cuvinte + 1 fotografie): ce se gateste acum la foc, de ce este special sezonul asta.
3. **Oferta sau eveniment viitor** (100 cuvinte): pachet sezonier din campania curenta (vezi FHM-007), eveniment special, workshop.
4. **Invitatie la revenire** (50 cuvinte): CTA clar — "Rezerva weekendul din [luna]" cu link direct.
5. **1 fotografie UGC** (cu credit): "Asa arata [luna] prin ochii [Prenume], oaspetele nostru" — social proof.

**Reguli GDPR pentru newsletter Romania:**
- Opt-in explicit la colectarea emailului (bifarea activa a checkbox-ului, NU pre-bifat)
- Link de dezabonare functional in FIECARE email (obligatoriu legal)
- Pastrati logul consimtamintelor (data, IP, text checkbox) — necesar in caz de audit ANSPDCP
- Nu vindeti/partajati adresele cu terti
- Dreptul la stergere: daca un oaspete cere sa fie sters, stergeti in 30 zile

### Pasul 9 — Automatizari avansate si segmentare in Mailchimp/ConvertKit
**Automatizari de setat (setup o data, ruleaza automat):**

1. **Welcome sequence** (pentru abonati noi de pe website/Instagram — NU oaspeti):
   - Email 1 (imediat): "Bine ai venit! Cine suntem si ce ne face speciali" + 3 fotografii cu foc/tuci/natura
   - Email 2 (3 zile): "Ce spun oaspetii nostri" + 3 recenzii + CTA "Rezerva prima vizita"
   - Email 3 (7 zile): "Meniul nostru preferat" + Reel embedded (daca suporta) + oferta "Prima vizita" cu mic bonus

2. **Post-stay sequence** (trigger: manual la check-out sau automat din PMS):
   - Email post-stay (24-48h) → Email follow-up review (7 zile) → Newsletter trimestrial (ongoing)

3. **Re-engagement** (pentru oaspeti care nu au revizitat in 12+ luni):
   - Email: "Ne este dor de voi la Sapte Focuri" + oferta speciala de revenire (10% discount sau activitate bonus)

**Segmente de audienta:**
| Segment | Criterii | Mesaj adaptat |
|---------|---------|--------------|
| Oaspeti recenti (< 6 luni) | Au vizitat in ultimele 6 luni | Re-booking sezon diferit |
| Oaspeti vechi (6-12 luni) | Vizita 6-12 luni in urma | "Ne este dor" + oferta |
| Oaspeti inactivi (12+ luni) | Nu au revizitat in 12+ luni | Re-engagement cu bonus |
| Abonati non-oaspeti | Nu au vizitat niciodata | Welcome sequence + social proof |
| Corporate | Contact B2B | Pachete corporate (vezi FHM-011) |

### Pasul 10 — Monitorizare metrici si optimizare continua
**KPI email marketing (lunar):**
| Metrica | Target | Industria hospitality media |
|---------|--------|---------------------------|
| Open rate | > 40% | 30-35% |
| Click rate | > 5% | 3-4% |
| Unsubscribe rate | < 0.5% | 0.3-0.5% |
| Review-uri generate / emailuri post-stay trimise | 1 din 5 (20%) | 10-15% |
| Rezervari din email | Track cu UTM | Variabil |

**Optimizare:**
- Testati 2 subiecte de email diferite (A/B testing) pe fiecare newsletter trimestrial — Mailchimp suporta nativ.
- Verificati open rate pe dispozitiv (mobil vs. desktop) — daca 80% deschid pe mobil, optimizati template-ul pentru mobil.
- Daca open rate scade sub 30%: verificati SPF/DKIM, schimbati subiectele (mai personale, mai scurte), verificati ca nu ajungeti in spam.
- Daca click rate scade sub 3%: simplificati email-ul (prea mult text), faceti CTA-ul mai vizibil, testati imagini vs. text-only.

## Verificare
- [ ] Platforma de email configurata cu domeniu propriu si SPF/DKIM/DMARC verificat
- [ ] Secventa completa de 5-6 emailuri creata si automatizata (confirmare → pre-arrival x2 → mid-stay → post-stay → follow-up review)
- [ ] Emailul de confirmare trimis automat la fiecare rezervare noua directa (maxim 5 minute delay)
- [ ] Emailul de pre-arrival programat automat cu 5 zile inainte de check-in
- [ ] Emailul de post-stay trimis la maximum 48h dupa check-out
- [ ] Newsletter trimestrial activ (Martie, Iunie, Septembrie, Decembrie)
- [ ] Rate de deschidere > 40%
- [ ] Minimum 1 recenzie generata per 5 emailuri post-stay trimise
- [ ] GDPR compliant: opt-in explicit, link dezabonare, log consimtaminte

## Instrumente
- Mailchimp (gratuit pana la 500 contacte) sau ConvertKit (gratuit pana la 1000 subs)
- Canva (template email design aliniat cu Brand Book)
- Google Analytics + UTM tags (tracking click-uri din email pe website)
- Google Sheets (tracking rezervari din email daca nu exista PMS)
- MXToolbox.com (verificare SPF/DKIM/DMARC setup)

## Note
- **Emailul de post-stay cu cerere de recenzie este emailul cu cel mai mare ROI din intregul flux** — nu il omiteti niciodata.
- Personalizarea face diferenta uriasa in hospitality — chiar si un singur detaliu personal (prenumele cainelui, activitatea preferata, mancarea care le-a placut cel mai mult) creste rata de raspuns cu 40%+.
- Nu automatizati complet emailul de mid-stay — verificati manual ca nu exista probleme semnalate inainte de trimitere.
- **Pre-arrival sequence (3 emailuri) creste satisfactia oaspetelui cu 15-20%** (studiu Cornell Hospitality Report) — oaspetii care stiu la ce sa se astepte si sunt entuziasmati INAINTE de sosire raporteaza experienta mai pozitiv.
- Mailchimp gratuit este suficient pentru primii 500 de oaspeti unici — nu investiti in platforma platita pana nu atingeti acest prag.
