---
type: procedure
created: 2026-03-20
status: active
slug: m-008-cpa-tracking-voluum
tags: [procedure, nexus]
---

# M-008 — CPA Campaign Tracking Setup with Voluum

**Domeniu**: affiliate | **Sub-domeniu**: tracking-voluum
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Configurarea completa a sistemului de tracking (Voluum, BeMob, RedTrack, sau Binom self-hosted) pentru campanii CPA, de la setup domeniu custom si integrare retele pana la split testing, event tracking, si optimizare bazata pe date. Fara tracking profesional, afiliatul opereaza orbeste — nu stie care landing page converteste, care oferta performeaza, sau care sursa de trafic aduce ROI pozitiv. Erori in setup duc la date incorecte care genereaza decizii proaste de optimizare.

**SMSads Context**: Binom (self-hosted, accesat de Kody) este tracker-ul actual. Toate campaniile SMS/RCS necesita tracking prin tracker pentru atribuire corecta a conversiilor si optimizare EC/1K SMS. Setup-ul corect al postback-urilor cu 3SNET/MaxBounty este critic — tracking rupt = zero date = zero optimizare. Problema actuala: MarathonBet tracking rupt → zero date CR reale → estimari nevalidate.

**Ce se intampla fara procedura:**
- Campanii lansate fara tracking → zero date de optimizare → buget ars
- Postback configurat gresit → conversii pierdute din rapoarte (discrepanta tracker vs network: >20% = problema)
- Split test fara tracking granular → imposibil sa identifici LP castigator
- Cost tracking absent → ROI calculat gresit → scale pe campanii neprofitabile

---

## Pasi

### Pas 1: SELECTIE TRACKER SI SETUP INITIAL

Selecteaza tracker-ul potrivit:

| Tracker | Cost | Best For | Self-Hosted | AI Optimization |
|---------|------|----------|-------------|-----------------|
| **Voluum** | $89-999/luna | Affiliates cu buget | NU (cloud) | DA (auto-optimize) |
| **BeMob** | $0-499/luna | Start gratuit | NU (cloud) | Basic |
| **RedTrack** | $49-749/luna | Multi-channel | NU (cloud) | DA |
| **Binom** | $69/luna | Power users, control total | DA (VPS) | NU |

**Recomandare SMSads**: Binom self-hosted (deja activ) — control total, zero limita de evenimente, cost fix. Voluum ca alternativa cloud daca nu ai VPS.

**Setup initial**:
1. Creaza cont pe platforma aleasa (sau instaleaza Binom pe VPS)
2. Configureaza timezone corect (UTC sau timezone GEO principal — afecteaza raportarea zilnica)
3. Seteaza currency (USD standard)
4. Configureaza user roles (admin + read-only pentru raportare)

### Pas 2: CONFIGURARE DOMENIU CUSTOM TRACKING

Domeniu custom = deliverability mai buna si evitarea blocarii de ad networks si carrier-i:

1. **Achizitioneaza domeniu dedicat tracking**: ex: `trk.yourdomain.com` sau `go.yourdomain.com` (nu folosi domeniul principal)
2. **Configureaza DNS**: CNAME record pointing to tracker server (Voluum: instrucțiuni in dashboard; Binom: A record la IP VPS)
3. **SSL obligatoriu**: Let's Encrypt gratuit sau Cloudflare SSL (HTTPS obligatoriu — HTTP = blocked de browsere)
4. **Verifica**: acceseaza `https://trk.yourdomain.com` → trebuie sa returneze pagina tracker-ului

**Eroare frecventa**: Folosirea domeniului default al tracker-ului (ex: `campaign.voluum.com`) → blocked de carrier-i SMS, flagged de AdSense/Facebook. Domeniu custom = OBLIGATORIU.

### Pas 3: ADAUGARE SURSE DE TRAFIC

Adauga fiecare sursa de trafic in tracker cu configuratie specifica:

| Sursa Trafic | Template | Token-uri | Cost Tracking |
|-------------|----------|-----------|---------------|
| SMS (VOX carrier) | Custom | seg={segment}, geo={geo}, carrier={carrier} | Manual (cost/SMS known) |
| PropellerAds (push) | Pre-built template | zone_id, campaign_id, creative_id | Auto (postback) |
| RichAds (push) | Pre-built template | creative_id, campaign_id | Auto (API) |
| MGID (native) | Pre-built template | widget_id, teaser_id | Auto (postback) |
| Evadav (push/pop) | Pre-built template | site_id, campaign_id | Auto |
| Facebook Ads | Custom | campaign_id, adset_id, ad_id | Manual (export) |

**Token-uri obligatorii per campanie**:
- `{clickid}` — identificator unic per click (generat de tracker)
- `{source}` — sursa de trafic
- `{campaign}` — campanie ID
- `{creative}` — creative ID (pentru A/B testing)
- `{geo}` — tara (ISO code)

### Pas 4: ADAUGARE RETELE AFFILIATE CU POSTBACK

Adauga fiecare retea CPA si configureaza postback URL:

**Setup per retea:**

**MaxBounty:**
- Postback URL: `https://trk.yourdomain.com/postback?clickid={subid1}&payout={commission_amount}&txid={transaction_id}`
- In MaxBounty: Campaign Settings → Server Postback URL → paste URL-ul de mai sus
- Test: cere AM-ului sa fire manual postback → verifica in tracker

**3SNET:**
- Postback URL: `https://trk.yourdomain.com/postback?clickid={clickid}&payout={payout}`
- In 3SNET: configurare per oferta → Postback → paste URL
- Test: fire postback manual din dashboard

**ClickDealer / Mobidea / Advidi:**
- Template pre-built in Voluum/Binom → selecteaza network → copy postback URL generat → paste in network dashboard

**Validare CRITICA**: Postback-ul trebuie sa fire in <5 secunde. Dupa configurare, MEREU test cu conversie test. Discrepanta tracker vs network >5% = problema de setup.

### Pas 5: CREARE CAMPAIGN FLOW

Defineste flow-ul complet per campanie:

```
Traffic Source → Tracking Link (click capture) → Landing Page(s) → Offer(s)
```

**Configurare:**
1. Creaza campaign in tracker cu: traffic source, offer, landing page(s)
2. Seteaza landing page URL cu click URL embeded: link-ul pe care user-ul il clickeaza din LP duce la oferta prin tracker
3. Seteaza offer URL cu affiliate link + `{clickid}` parameter (pentru postback matching)
4. Redirect method: 302 redirect standard (Voluum) sau meta redirect (Binom — mai rapid)

**Verificare end-to-end (OBLIGATORIE):**
1. Click pe tracking URL → verifica ca LP se incarca
2. Click pe CTA din LP → verifica ca oferta se incarca
3. Verifica in tracker: click inregistrat cu toate token-urile populate
4. Fire test postback → verifica conversie in tracker

### Pas 6: SETUP SPLIT TESTING (A/B/C)

Configurare split testing pentru landing pages si oferte:

**Landing page split:**
- Adauga 2-3 LP-uri in campaign → seteaza weight 33/33/33 (egal initial)
- Minim 100 clicks per LP inainte de decizie
- Dupa 48h: LP cu CTR <50% din media → pause. LP winner → 100% trafic

**Offer split:**
- Aceeasi LP, 2-3 oferte diferite → seteaza weight egal
- Minim 2x payout spend per oferta inainte de decizie
- Winner = oferta cu cel mai mare EPC (nu cel mai mare CR — EPC include payout)

**Smart rotation (Voluum AI):**
- Activeaza dupa 100+ conversii totale pe campaign
- Voluum distribuie trafic automat catre winner-ul curent
- Override manual: daca vrei sa testezi varianta noua, forteaza weight

### Pas 7: GENERARE TRACKING URLs

Genereaza tracking URL-uri per campanie:
- **URL unic per canal**: un URL pentru SMS Bolivia, altul pentru push PropellerAds Bolivia, altul pentru native MGID Kenya
- **Format**: `https://trk.yourdomain.com/campaign-id?source=sms&geo=BO&seg=ARTS_ENT`
- **URL scurtat** (pentru SMS): foloseste link shortener propriu (nu bit.ly — risc de block)
- **Test FIECARE URL**: click → verifica in tracker → verifica ca LP si oferta se incarca corect

### Pas 8: IMPLEMENTARE POSTBACK/PIXEL INTEGRATION

**Preferinta**: Postback (S2S, server-to-server) — mai reliable decat pixel (browser-based):
- Postback: serverul network-ului notifica serverul tracker-ului → zero dependenta de browser-ul user-ului
- Pixel: JavaScript pe pagina de multumiri → depinde de browser → blocked de ad blockers (20-30% pierdere)

**Setup S2S postback:**
1. In tracker: genereaza postback URL cu `{clickid}` si `{payout}` tokens
2. In network: adauga postback URL la nivel de oferta
3. Test: fire test conversie → verifica in tracker in <5 secunde
4. Monitor: verifica zilnic discrepanta tracker vs network (<5% = OK, >10% = problema)

### Pas 9: CONFIGURARE EVENT TRACKING (MICRO-CONVERSII)

Tracking micro-conversii pentru optimizare chiar cu volum scazut:
- **Click pe LP CTA** (event: lp_click) → JavaScript pe LP care fire-aza event la tracker
- **Form start** (event: form_start) → fire la focus pe primul camp
- **Form submit** (event: form_submit) → fire la submit
- **Scroll depth 50%** (event: scroll_50) → indicator de engagement
- **Video play** (event: video_play) → daca LP are video

**Implementare**: adauga tracking script pe LP:
```html
<script>
// Voluum/Binom event tracking pixel
var img = new Image();
img.src = 'https://trk.yourdomain.com/event?clickid={clickid}&event=lp_click';
</script>
```

**Beneficiu**: poti optimiza pe LP CTR (click pe CTA) inainte de a avea suficiente conversii finale — mai rapid decat asteptarea conversiilor.

### Pas 10: ANALIZA INITIALA 48H SI OPTIMIZARE SAPTAMANALA

**Analiza la 48 ore:**
1. CTR per landing page: care LP obtine cele mai multe clicks pe CTA?
2. CR per oferta: care oferta converteste cel mai bine?
3. Cost per conversie per sursa/geo/creative
4. Raport: top performers si underperformers pe fiecare nivel al funnel-ului

**Optimizare saptamanala (REPEAT):**
- LP cu CTR <50% din media → PAUSE
- Oferta cu CR <50% din media → PAUSE
- Creative cu CTR <50% din media → PAUSE
- La fiecare ciclu: testeaza 1-2 variante noi vs winner curent
- Documenteaza: ce s-a schimbat, rezultat, decizie

**Monitorizare discrepanta tracker vs network:**
- Export raport din tracker (conversii, revenue)
- Export raport din network (conversii, revenue)
- Calculeaza delta: `(tracker_conversions - network_conversions) / network_conversions`
- <5% = normal (timing differences). 5-15% = investigheaza (postback delay?). >15% = PROBLEMA (postback rupt sau shaving)

---

## Verificare

- [ ] Tracker configurat cu timezone si currency corecte?
- [ ] Custom domain cu SSL activ?
- [ ] Toate sursele de trafic adaugate cu token-uri corecte?
- [ ] Postback URLs functionale per retea (testate cu conversie test)?
- [ ] Campaign flow verificat end-to-end (click → LP → oferta → conversie)?
- [ ] Split test activ cu minim 2 variante LP?
- [ ] Event tracking implementat pe landing pages?
- [ ] Analiza 48h documentata?
- [ ] Discrepanta tracker vs network <5%?
- [ ] Optimizare saptamanala programata?

---

## Instrumente

| Tool | Rol |
|------|-----|
| Voluum / BeMob / RedTrack / Binom | Tracking platform |
| Custom tracking domain + SSL | Deliverability si anti-block |
| Network dashboards (MaxBounty, 3SNET, etc.) | Postback configuration |
| Google Sheets | Tracking discrepancy monitoring |
| PageSpeed Insights | Verificare LP speed cu tracking code |

---

## Note

- Depinde de M-007 (oferte CPA selectate). Este prerequisite pentru M-009 si M-112.
- Binom (self-hosted) = recomandat pentru SMSads (control total, zero limita evenimente, $69/luna).
- Discrepanta >15% intre tracker si network = semn de shaving. Contacteaza AM cu dovezi.
- Postback vs Pixel: MEREU postback (S2S) pentru CPA campaigns. Pixel doar ca backup.
- La >10,000 clicks/zi, evalueaza upgrade tracking infrastructure (CDN, multi-server).
