---
id: LS-010
title: Live Stream Production Setup Checklist (Equipment / Software)
domain: live-streaming
source: Live Shopping Mastery — Production & Technical Setup
version: 2.0
forge_score: "estimated M/3.5"
business_mapping: ["albastru"]
---

# Live Stream Production Setup Checklist (Equipment / Software)

## Obiectiv
Un checklist complet de echipamente și software la 3 niveluri de buget (Starter/Intermediate/Pro), cu prețuri exacte, setări tehnice OBS detaliate, workflow de setup per platformă, și checklist pre-live pas cu pas — optimizat pentru live cooking/commerce.

## Pași

### 1. Setup Starter (sub €150 — validare concept, primele 3 luni)
**Camera**: Telefonul actual (iPhone 12+ / Samsung S21+) — 4K nativ suficient
**Stabilizator**: Trepied cu clemă de telefon (€15-25, eMAG/Amazon)
**Iluminare**: Ring light LED 26cm ($30-80 / €25-70) — elimină umbrele, skin tone bun
**Microfon**: Lavalieră cu jack 3.5mm (€15-30) — ESENȚIAL (audio > video ca prioritate)
**Internet**: Cablu ethernet direct (dacă posibil) sau WiFi 5GHz dedicat — minim 10 Mbps upload
**Software**: TikTok app nativ (stream direct, zero setup)
**Fundal**: Spațiu curat, iluminare naturală sau ring light
**Total estimat**: €55-120

### 2. Setup Intermediate (€300-1.000 — primele venituri, 1K+ followeri)
**Camera**: Webcam Logitech C920 (€80) sau Sony ZV-E10 ($750 / €700) cu capture card
**Capture Card**: Elgato Cam Link 4K ($150 / €120) — mirrorless → laptop
**Iluminare**: 2x Softbox LED (€60-100 set) sau Key Light + Fill Light
**Microfon**: Rode NT-USB Mini ($170 / €100) USB plug-and-play SAU Shure MV7 (€230)
**Internet**: Router dedicat ASUS RT-AX55 (€80) + cablu ethernet
**Software**: OBS Studio (gratuit) + Streamlabs (gratuit/paid)
**Trepied**: Full-size Manfrotto entry (€80)
**Stream Controller**: Elgato Stream Deck Mini ($150 / €80) — scene switching rapid
**Audio Interface** (opțional): Focusrite Scarlett Solo (€120) pentru XLR mic
**Total estimat**: €450-900

### 3. Setup Pro (€1.000-3.000 — full-time streamer/business)
**Camera primară**: Sony ZV-1 ($750) sau Canon EOS M50 Mark II cu 16-50mm
**Camera secundară**: telefon montat bird's eye (overhead view pentru cooking)
**Switcher video**: ATEM Mini Pro (€300) — schimb între camere live fără glitch
**Iluminare**: Elgato Key Light Air x2 (€200 total) + LED strip ambiance
**Microfon**: Shure SM7B (€380) + Focusrite Scarlett 2i2 (€160)
**Wireless mic**: DJI Mic (€300) — lavalier wireless, libertate mișcare în bucătărie
**PC/Mac dedicat**: Mac Mini M4 sau PC cu RTX 3060 pentru encoding hardware
**Internet**: Fibră 1Gbps + backup 4G router (Huawei CPE Pro)
**Software**: OBS + Restream Pro (€20/lună) + Streamlabs Pro
**Greenscreen** (opțional): background virtual custom (€50-100)
**Total estimat**: €1.500-3.000

### 4. Setări tehnice OBS Studio detaliate
```
Scene: "LIVE COOKING"
Sources:
  - Video Capture Device (camera principală)
  - Video Capture Device 2 (camera bird's eye — opțional)
  - Audio Input Capture (microfon principal)
  - Image (logo/watermark overlay — colț dreapta sus)
  - Text (CTA: "Coș → link sus | Patreon → link bio")
  - Browser Source (Streamlabs alerts pentru Super Chat/donations)
  - Color Source (background color sau greenscreen)

Output Settings:
  - Encoder: NVENC (GPU, recomandat) sau x264 (CPU fallback)
  - Rate Control: CBR
  - Bitrate: 3500 kbps (720p) / 5000 kbps (1080p)
  - Keyframe Interval: 2
  - Profile: High
  - Preset: Quality (NVENC) / veryfast (x264)

Video Settings:
  - Base Resolution: 1920x1080
  - Output Resolution: 1280x720 (reduce load, suficient pentru mobile)
  - FPS: 30 (nu 60 — economisește bandwidth)
  - Downscale Filter: Lanczos

Audio Settings:
  - Sample Rate: 44.1 kHz
  - Channels: Stereo
  - Mic Input: -14 LUFS target (nu clip audio)
  - Noise Gate: activat (elimină zgomot ambient)
```

### 5. Setup specific per platformă (RTMP keys)
**TikTok** (via TikTok Studio — OBLIGATORIU pentru Shop):
- studio.tiktok.com → Go Live → Use streaming software
- Server URL + Stream Key (regenerează la fiecare sesiune!)
- Shop funcționează DOAR prin TikTok Studio sau app nativ

**YouTube** (via OBS/Restream):
- YouTube Studio → Create → Go Live → Stream → Copy persistent key
- Server: `rtmp://a.rtmp.youtube.com/live2`
- Cerință: YPP activ (1K subs + 4K ore) pentru monetizare

**Multi-platform** (via Restream.io):
- restream.io → Add Channels → configurează RTMP per platformă
- OBS → Settings → Stream → Server: Restream RTMP URL
- Chat agregat: restream.io/chat

### 6. Checklist pre-live complet (T-30 minute)
**Tehnic**:
- [ ] Internet: upload speed minim 10 Mbps (fast.com) — 15 Mbps pentru simulcast
- [ ] Camera: lentilă curată, unghi verificat, focus pe zona de demo
- [ ] Audio: test microfon (vorbește 10 sec, ascultă playback), zgomote fundal eliminate
- [ ] Iluminare: ring light/softbox pornite, zero umbrele pe față
- [ ] OBS: deschis, toate sursele funcționale, preview 30 sec OK
- [ ] Stream key: regenerat (TikTok expiră!) și configurat corect

**Produse și conținut**:
- [ ] Produse pregătite fizic, etichetate cu prețuri vizibile
- [ ] Ingrediente gata (dacă cooking demo)
- [ ] Run sheet printat/salvat pe telefon, vizibil
- [ ] Live Cart TikTok Shop: toate produsele adăugate cu prețuri live speciale

**Personal**:
- [ ] Apă la îndemână (vocea obosește la 30+ min)
- [ ] Telefon pe silent / airplane mode (dacă streamezi de pe laptop)
- [ ] Backup plan: telefonul ca stream backup dacă OBS/laptop pică

**Promovare**:
- [ ] Story pe TikTok + Instagram cu 15 min înainte
- [ ] 3-5 persoane seed audience confirmate
- [ ] Notificări LIVE Events TikTok activate

### 7. Backup plan (ce faci când ceva pică)
- **Internet pică**: switch la 4G hotspot backup, anunță audiența "Revin în 2 minute"
- **Camera pică**: switch la telefon (backup camera mereu la îndemână)
- **OBS crash**: restart OBS, reconectează stream key (TikTok: regenerare necesară)
- **Audio issues**: switch la lavalieră backup sau built-in mic telefon
- **TikTok Shop bug**: anunță "Link în bio" + postează link în comentarii

### 8. Prioritizare investiții echipamente (ROI order)
1. **Microfon** (€15-170): cel mai mare impact pe retenție — audiența iartă video mediocru, NU iartă audio prost
2. **Iluminare** (€25-200): ring light $30-80 = diferență vizuală dramatică cu investiție minimă
3. **Internet stabil** (€0-80): cablu ethernet > WiFi. Router dedicat dacă WiFi instabil
4. **Camera** (€0-750): telefonul actual e suficient 6+ luni. Upgrade doar dacă generezi revenue
5. **Software** (€0-20/lună): OBS gratuit e suficient. Restream Pro doar la simulcast 3+ platforme
6. **Stream Deck** ($150): quality of life, nu necesitate — invenstește după ce ai revenue stabil

## Verificare
- [ ] Setup ales și echipamentele achiziționate (minim Starter)
- [ ] Test stream privat de 10 min completat (video + audio + internet stabil)
- [ ] OBS configurat cu scene și sources corect
- [ ] Checklist pre-live printat și accesibil lângă stația de stream
- [ ] Backup plan documentat (internet, camera, OBS, audio)
- [ ] RTMP keys obținute și testate per platformă
- [ ] Upload speed verificat: minim 10 Mbps (15 Mbps simulcast)
- [ ] Prioritate investiții next documentată (ce cumperi cu primele revenue)

## Instrumente
- OBS Studio (obsproject.com) — gratuit, industry standard
- Streamlabs Desktop — OBS fork cu UI simplu + alert overlays
- fast.com — test viteză upload
- Elgato Key Light Air — lighting profesional cu control app
- ATEM Mini Pro (€300) — video switcher hardware multi-camera
- Elgato Stream Deck ($150) — scene switching + hotkeys
- Restream.io — simulcast management + chat agregat
- TikTok Studio (studio.tiktok.com) — TikTok Shop compatible stream

## Note
- **Audio > Video** ca prioritate investiție: lavalieră €15 face mai mult decât cameră €500
- **Albastru/Șapte Focuri**: setup ideal = Intermediate cu cameră bird's eye overhead deasupra focului + DJI Mic wireless pentru libertate în bucătărie
- Ring light ($30-80) = cel mai bun ROI din toate echipamentele
- Investiția totală pentru start: sub €120 (telefon + ring light + lavalieră + trepied)
