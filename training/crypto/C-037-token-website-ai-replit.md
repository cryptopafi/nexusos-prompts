---
type: procedure
created: 2026-03-22
status: active
slug: c-037-token-website-ai-replit
tags: [procedure, nexus]
---

# Build a Token Website with AI (Replit) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru construirea unui website funcțional pentru un token crypto folosind Replit și AI (Claude/GPT), de la structura paginilor până la deployment.

---

## 1. Problema

Un website profesional este o cerință de credibilitate pentru orice token crypto — fără el, platforme ca CoinGecko și CMC resping aplicația de listare, iar investitorii potențiali nu au unde să verifice informațiile proiectului. Mulți fondatori care nu sunt developeri nu știu cum să construiască rapid un site funcțional fără a cheltui mii de dolari pe un developer.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Token nou care are nevoie de un website de prezentare înainte de lansare
- Proiect care migrează de la un website slab la unul profesional
- Founder non-developer care vrea să construiască și să mențină singur site-ul

---

## 2. Procedura

### Pas 1: Definește structura și paginile necesare
Structura minimă necesară pentru un token website: Hero section (token name, tagline, CTA buy), About (ce problemă rezolvă), Tokenomics (vizual — supply, distribuție, vesting), Roadmap, Team (sau "Anonymous Team" dacă anon), How to Buy (pas-cu-pas), FAQ, și obligatoriu: Disclaimer + Terms of Use. Documentează wireframe-ul înainte de a construi.

### Pas 2: Configurează Replit și alege template-ul
Creează cont pe replit.com dacă nu ai. Alege "Create Repl" → "HTML, CSS, JS" pentru site simplu sau "React" pentru mai multă interactivitate. Alternativ, folosește Replit Ghostwriter (AI integrat) sau importă un template HTML gratuit de pe ThemeForest/HTML5UP. Configurează proiectul.

### Pas 3: Folosește AI pentru a genera structura HTML/CSS
În Replit Ghostwriter sau Claude, furnizează prompt detaliat: "Creează un website one-page pentru un token crypto numit [NAME]. Include secțiunile: hero, about, tokenomics, roadmap, how to buy, disclaimer. Design dark theme cu accent [COLOR]. Responsive mobile." Iterează pe output până obții structura dorită.

### Pas 4: Personalizează conținutul și design-ul
Înlocuiește textele placeholder cu conținutul real al proiectului. Adaugă logo-ul și culorile brandului. Integrează graficul de tokenomics (poți folosi Chart.js pentru pie chart animat). Adaugă animații CSS simple pentru professionalism. Verifică că toate link-urile (Twitter, Telegram, contract) sunt corecte.

### Pas 5: Adaugă secțiunea Disclaimer/Terms obligatorie
Aceasta este cerința critică: adaugă o pagină separată sau secțiune vizibilă cu disclaimer complet: "Nu constituie sfat de investiții. Tokenul este un activ speculativ. Investiți doar ce vă puteți permite să pierdeți. Nu există garanții de randament." Asigură-te că este vizibil și neambiguu.

### Pas 6: Testează website-ul pe mobile și desktop
Verifică: responsive design pe mobil (Chrome DevTools device emulation), viteza de încărcare (Google PageSpeed Insights — target: 80+), toate link-urile funcționale, formularele dacă există, și că adresa contractului este copiabilă cu un click. Fixează orice erori identificate.

### Pas 7: Deployează și configurează domeniul
Pe Replit, folosește "Deploy" pentru hosting gratuit (replit.app subdomain) sau conectează un domeniu custom (ex. [token].xyz). Alternativ, deployează pe Vercel sau Netlify din GitHub pentru hosting mai robust. Verifică că site-ul este accesibil public și documentează URL-ul. Salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Website token creat cu AI pe Replit. Toate secțiunile obligatorii incluse (inclusiv Disclaimer/Terms), deployment live, URL documentat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-037",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "website", "replit", "ai", "token-launch", "development"],
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
- Website lansat fără secțiune Disclaimer/Terms → violation
- Site neresponsiv pe mobil → violation (respingere CoinGecko)
- Contract address incorect sau lipsă pe site → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Secțiunea Disclaimer/Terms prezentă și vizibilă?
- [ ] Site testat pe mobile și PageSpeed ≥ 80?

**VK-uri obligatorii:**
1. `✅ [PROC] C-037 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Build a Token Website with AI (Replit)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Replit | IDE și hosting online |
| Replit Ghostwriter / Claude | Generare cod cu AI |
| Google PageSpeed Insights | Test performanță site |
| Vercel / Netlify | Hosting alternativ producție |
| Namecheap / GoDaddy | Domeniu custom (.xyz, .io) |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Secțiuni obligatorii prezente | 8/8 (inclusiv Disclaimer) |
| Google PageSpeed Score | ≥ 80 |
| Responsive mobile | 100% |
| Timp construire site | < 4 ore |
| Site live și accesibil | Verificat pre-listare CoinGecko/CMC |
