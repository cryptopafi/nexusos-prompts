---
id: LS-004
title: Multi-Platform Simulcast Setup (TikTok / YouTube / Instagram)
domain: live-streaming
source: Live Shopping Mastery — Distribution Strategy
version: 2.0
forge_score: "estimated M/3.5"
business_mapping: ["albastru"]
---

# Multi-Platform Simulcast Setup (TikTok / YouTube / Instagram)

## Obiectiv
Configurarea unui sistem de simulcast care transmite simultan pe TikTok, YouTube și Instagram Live dintr-o singură sursă, triplicând audiența potențială, cu management corect al comentariilor cross-platform, limitări de platformă și repurposing post-stream.

## Pași

### 1. Alegerea software-ului de simulcast
**Opțiunea A — Restream.io** (recomandat pentru beginners/intermediate):
- Plan gratuit: 2 platforme simultane; Pro ($19/lună): nelimitat
- Conectează conturile TikTok, YouTube, Facebook/Instagram
- Copiază RTMP URL + Stream Key din Restream → configurează în OBS
- Chat agregat: restream.io/chat — toate comentariile într-o fereastră

**Opțiunea B — OBS Studio + plugin multi-output** (avansat, gratuit):
- Instalează OBS Studio (obsproject.com)
- Plugin "obs-multi-rtmp" (GitHub: sorayuki/obs-multi-rtmp) — adaugă output RTMP per platformă
- Necesită upload bandwidth: minim 15 Mbps pentru 3 platforme simultane la 720p
- Control total, zero cost, dar setup complex

**Opțiunea C — StreamYard** (cel mai simplu, browser-based):
- streamyard.com → Connect Destinations → adaugă platforme
- $25/lună plan Essential: 3 platforme, branding custom
- Nu necesită OBS — funcționează 100% în browser
- Chat agregat built-in cu display on-screen

### 2. Obținerea cheilor RTMP per platformă
**TikTok** (RTMP — doar prin TikTok Studio):
- TikTok Studio (studio.tiktok.com) → Go Live → Use streaming software
- Copiază Server URL + Stream Key (expiră după fiecare sesiune — regenerează!)
- LIMITARE: TikTok Shop NU funcționează cu simulcast/OBS — doar app nativ sau TikTok Studio

**YouTube** (RTMP — persistent key):
- YouTube Studio → Create → Go Live → Stream → Copy Stream Key
- Server: `rtmp://a.rtmp.youtube.com/live2`
- Stream key persistent (nu expiră) — configurează o dată
- Cerință YPP: 1K subs + 4K ore watch time (sau YPP Lite: 500 subs + 3K ore)

**Instagram/Facebook** (RTMP):
- Meta Live Producer (live.facebook.com/producer) → Create Live Stream
- Sau Facebook Creator Studio pentru Instagram simultaneous
- Instagram LIVE din RTMP necesită cont Professional + Meta approval

### 3. Configurarea OBS pentru simulcast stabil
**Setări recomandate (720p — balans calitate/bandwidth)**:
- Video: 1280x720 (720p — suficient pentru mobile viewing)
- Bitrate: 3000-4500 kbps per output (total: 9-13 Mbps pentru 3 platforme)
- Encoder: x264 (CPU) sau NVENC (GPU Nvidia) — NVENC recomandat pentru multi-output
- FPS: 30 (nu 60 — reduce bandwidth cu 50%)
- Audio: AAC, 128kbps, 44.1kHz

**Scenă minimă OBS**:
```
Sources:
  - Video Capture Device (camera principală — Sony ZV-1 $750 sau webcam)
  - Audio Input Capture (microfon — Rode NT-USB $170)
  - Image (logo/watermark overlay)
  - Text (CTA: "Coș → link în bio")
  - Browser Source (Streamlabs alerts pentru donations/Super Chat)
```

**Echipament recomandat pentru simulcast stabil**:
- Router dedicat streaming: ASUS RT-AX55 (€80)
- Cablu ethernet direct (nu WiFi pentru simulcast)
- Upload speed: minim 15 Mbps pentru 3 platforme la 720p
- Backup: 4G hotspot (Huawei CPE Pro) dacă fibra pică

### 4. Managementul comentariilor multi-platform
**Problema**: comentariile vin din 3 locuri simultan — imposibil de monitorizat individual.

**Soluții**:
- **Restream Chat** (restream.io/chat): agregă TikTok + YouTube + Facebook într-o fereastră
- **StreamYard**: chat agregat built-in + display on-screen per platformă
- **Manual split-screen**: 3 ferestre browser (ultimul resort)
- **Asistent dedicat**: la 100+ vieweri totali, desemnează o persoană să modereze chat-urile secundare
- **Anunțuri cross-platform**: "Sunt live și pe YouTube, linkul e în bio"

**Elgato Stream Deck ($150)**: configurează butoane pentru a switch-ui între scene și a interacționa rapid cu fiecare platformă.

### 5. Strategia de platformă (ce pe care)
**TikTok** (stream nativ din app sau TikTok Studio):
- Prioritar pentru shopping live (TikTok Shop integrat)
- Audiență tânără (18-35), discovery puternic
- Stream nativ necesar pentru coșul de produse

**YouTube** (simulcast via Restream/OBS):
- Watch time contează pentru YPP (1K subs + 4K ore)
- Super Chat monetizare ($2-5/viewer avg)
- VOD automat salvat — repurposing continuu
- Audiență diaspora RO (UK, IT, DE) cu CPM 3-5x mai mare

**Facebook/Instagram** (simulcast via Restream/OBS):
- Audiență mai vârstnică (35-55) — familia, localnicii
- Instagram Live notifică followerii automat
- Facebook Groups = audiență dedicată

**Albastru/Șapte Focuri**: TikTok nativ pentru shopping + simulcast YouTube/Facebook pentru reach audiență diversă.

### 6. Limitări tehnice de știut
- **TikTok Shop NU funcționează cu OBS/Restream** — produsele din coș disponibile DOAR prin app nativ sau TikTok Studio
- **Soluție**: stream nativ TikTok pentru shopping live + simulcast separat YouTube/Facebook doar pentru reach
- **Instagram LIVE din RTMP**: quality reduced vs. Instagram nativ
- **Latency diferită**: YouTube ~10-15s, TikTok ~5-10s, Facebook ~10-20s — comentariile vin decalat
- **Copyright audio**: muzica pe YouTube = strike instant; pe TikTok = mai tolerant

### 7. Post-stream: salvare și repurposing
- **YouTube**: stream salvat automat ca VOD (video on demand)
- **TikTok**: activează "Save to device" ÎNAINTE de live
- **Facebook**: salvat automat în pagină
- **Repurposing imediat**: taie 3-5 clipuri highlight (10-30 sec) → Reels/Shorts/TikTok organic
- **Editing tools**: CapCut (gratuit, template-uri TikTok native) sau Kapwing (browser-based)
- **Postează clipurile în primele 2h post-live** pentru algoritm timing optim

## Verificare
- [ ] Software de simulcast configurat și testat (test privat de 5 min pe toate platformele)
- [ ] Toate platformele conectate și cheile RTMP valide (TikTok key regenerată)
- [ ] Chat agregat funcțional (Restream Chat sau StreamYard)
- [ ] Upload speed testată: minim 15 Mbps stabil pentru 3 platforme (fast.com)
- [ ] TikTok configurat nativ separat pentru Shop (nu simulcast)
- [ ] Test complet simulcast fără disconnect pe nicio platformă
- [ ] Backup internet disponibil (4G hotspot)
- [ ] Post-stream repurposing workflow definit

## Instrumente
- OBS Studio (obsproject.com) — gratuit, multi-output cu plugin
- Restream.io — simulcast management + chat agregat ($19/lună Pro)
- StreamYard ($25/lună) — alternativă browser-based cu chat built-in
- fast.com — test viteză upload
- CapCut / Kapwing — editare post-live clips
- Elgato Stream Deck ($150) — scene switching rapid
- obs-multi-rtmp plugin — multi-output gratuit OBS

## Note
- **IMPORTANT**: TikTok Shop NU funcționează cu simulcast — stream nativ obligatoriu pentru shopping
- **Albastru**: simulcast YouTube + Facebook ideal pentru audiența mai vârstnică + TikTok nativ pentru tânără
- Upload bandwidth: calcul 3000kbps x 3 platforme = 9 Mbps minim, recomandat 15+ Mbps
- Simulcast triplează reach dar dilute engagement per platformă — choose quality over quantity la început
