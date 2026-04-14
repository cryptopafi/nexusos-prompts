---
type: procedure
created: 2026-03-22
status: active
slug: m-064-wordpress-website-setup
tags: [procedure, nexus]
---

# WordPress Website Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurare completă a unui site WordPress de la hosting până la conectarea cu analytics.

---

## 1. Problema

Un site WordPress nou trebuie configurat corect de la zero, incluzând hosting, instalare, temă, plugin-uri esențiale, securitate și tracking. Fără un proces standard, apar configurări incomplete, vulnerabilități de securitate și lipsă de tracking.

Situații acoperite:
- Lansare site nou pentru client sau proiect propriu
- Migrare de la altă platformă la WordPress
- Setup rapid pentru landing page sau blog

---

## 2. Procedura

### Pas 1: Alege și configurează hostingul
Selectează provider în funcție de buget și trafic așteptat:
- **SiteGround** — pentru proiecte mici/medii, suport excelent
- **WP Engine** — pentru proiecte enterprise, managed WordPress
- **Cloudways** — pentru flexibilitate cloud (DigitalOcean/AWS)

Achiziționează planul, configurează domeniul și activează SSL din panoul de control.

### Pas 2: Instalează WordPress
Folosește instalatorul automat din cPanel/Plesk sau instalează manual:
- Descarcă WordPress de pe wordpress.org
- Creează baza de date MySQL și user cu privilegii complete
- Copiază `wp-config-sample.php` în `wp-config.php` și completează credențialele DB
- Rulează instalatorul accesând domeniul în browser

### Pas 3: Selectează și configurează tema
Alege una din temele recomandate pentru performanță:
- **Kadence** — flexibil, FSE-ready, gratis
- **Astra** — ușor, integrare bună cu page buildere
- **GeneratePress** — minimal, rapid, ideal pentru SEO

Instalează tema, activează și importă demo dacă există. Personalizează logo, culori, fonturi din Customizer.

### Pas 4: Instalează plugin-uri esențiale
Instalează și configurează setul minim:
- **Yoast SEO** — meta tags, sitemap, breadcrumbs
- **WooCommerce** — dacă site-ul are magazin online
- **Contact Form 7** — formulare de contact
- **Wordfence** — securitate, firewall, malware scan

Activează fiecare plugin și parcurge wizard-ul de configurare inițial.

### Pas 5: Configurează structura permalink
Mergi la Settings → Permalinks și selectează `/%postname%/` (post name). Salvează pentru a regenera `.htaccess`. Verifică că URL-urile sunt curate și indexabile.

### Pas 6: Activează SSL și verifică HTTPS
Verifică că SSL este activ (lacăt verde în browser). Instalează plugin **Really Simple SSL** dacă redirecționarea nu este automată. Actualizează Settings → General: schimbă URL-urile din `http://` în `https://`.

### Pas 7: Creează paginile esențiale
Creează și publică minimum:
- **Home** — pagina principală
- **About** / Despre noi
- **Contact** — cu formular funcțional
- **Privacy Policy** — obligatorie GDPR (WordPress generează draft automat la Tools → Privacy)

Setează Home page în Settings → Reading.

### Pas 8: Conectează Google Analytics
Instalează **GA Google Analytics** plugin sau adaugă codul GA4 via header. Alternativ, folosește **GTM** (Google Tag Manager) pentru management centralizat al tagurilor. Verifică funcționarea în GA4 Real-Time report.

---

## 3. Cortex Logging

```json
{
  "text": "WordPress Website Setup executat: hosting configurat, WP instalat, temă + plugin-uri esențiale activate, SSL activ, pagini create, GA conectat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-064",
    "domain": "website-landing-pages",
    "rule_id": "META-H-002",
    "tags": ["wordpress", "setup", "hosting", "website", "ssl", "plugins"],
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

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Site lansat fără SSL activ → violation (risc securitate)
- Plugin securitate (Wordfence) absent la lansare → violation
- Privacy Policy absentă → violation (non-conformitate GDPR)
- GA conectat fără verificare în Real-Time report → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum website-landing-pages

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-064 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "WordPress Website Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| SiteGround / WP Engine / Cloudways | Hosting WordPress |
| Kadence / Astra / GeneratePress | Temă WordPress |
| Yoast SEO | Optimizare SEO on-page |
| Wordfence | Securitate site |
| Google Analytics 4 | Tracking trafic |
| Google Tag Manager | Management centralizat taguri |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| SSL activ | Da |
| GA tracking funcțional | Da |
| Plugin-uri esențiale instalate | Minimum 4 |
