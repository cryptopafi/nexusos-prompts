---
type: procedure
created: 2026-03-22
status: active
slug: m-055-brand-identity-design-visual
tags: [procedure, nexus]
---

# Brand Identity Design (Visual) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces de creare a identității vizuale de brand — logo, paletă de culori, tipografie și design system — aliniată cu brand story-ul și mesageria definite.

---

## 1. Problema

Brandurile fără o identitate vizuală coerentă par neprofesionale și nedemne de încredere, indiferent de calitatea produsului sau serviciului oferit. Inconsistența vizuală pe diferite canale (website, social media, materiale print) diluează brand recognition și crește costul de producție prin recrearea elementelor de la zero la fiecare proiect.

Situații acoperite:
- Crearea identității vizuale pentru un brand nou
- Rebranding al unui brand existent cu identitate depășită
- Standardizarea elementelor vizuale pentru o echipă de marketing care lucrează inconsistent

---

## 2. Procedura

### Pas 1: Audit vizual și cercetare competitivă
Analizează identitățile vizuale ale competitorilor principali pentru a evita similitudinea și a identifica oportunități de diferențiere. Documentează culorile dominante din categorie (ex: albastru în finance, verde în health). Caută inspirație vizuală în industrii adiacente și branduri aspiraționale ale publicului țintă. Auditul previne investiția în identitate care nu se distinge în piață.

### Pas 2: Definirea direcției creative și mood board
Bazat pe brand personality (din M-054), creează 2-3 direcții creative diferite în format mood board. Fiecare direcție include: referințe de culori, tipografie, imagerie, textură și stil grafic. Prezintă mood board-urile stakeholderilor pentru aliniere devreme, înainte de execuție. Selecția unei direcții evită iterații costisitoare la etapele finale.

### Pas 3: Design logo și variante
Designează logo-ul principal cu minimum 3 variante: logo complet (icon + wordmark), logo simplificat (icon standalone), varianta monocromatică. Logo-ul trebuie să funcționeze la dimensiuni mici (favicon, 32px) și mari (billboard). Testează logo-ul pe fundal alb, negru și culori de brand. Livrabile: fișiere SVG, PNG transparent și PDF vectorial.

### Pas 4: Construirea paletei de culori
Definește paleta completă de culori de brand: culoarea primară, culoarea secundară, culori de accent (1-2), culori neutrale (alb, gri, negru de brand). Specifică codurile exacte: HEX, RGB și CMYK pentru fiecare culoare. Definește regulile de utilizare: culoarea primară pentru CTA-uri și elemente cheie, secundara pentru background-uri, accentele pentru evidențieri. Testează accesibilitate (contrast ratio WCAG AA minimum).

### Pas 5: Selectarea și specificarea tipografiei
Alege maximum 2 familii de fonturi: un font de titlu (headline) și un font de corp (body text). Specifică weights și stiluri utilizate per context: H1 Bold, H2 SemiBold, body Regular, caption Light. Definește scale tipografică completă (H1-H6, body, caption, label). Asigură-te că fonturile sunt disponibile web (Google Fonts sau licență achiziționată) și că se citesc bine pe ecrane mici.

### Pas 6: Crearea design system-ului și elementelor grafice
Construiește setul complet de elemente vizuale: iconografie (set de 20-30 icoane consistente), pattern-uri sau texturi secundare, stil de fotografie și imagerie, template-uri pentru materiale uzuale (prezentare PowerPoint, banner social media, email header, letterhead). Design system-ul devine "lego kit" pentru echipă — piese standardizate care se combină rapid.

### Pas 7: Documentarea Brand Visual Guidelines
Compilează toate specificațiile într-un Brand Visual Guidelines document: utilizare corectă și incorectă a logo-ului, paleta de culori cu coduri, tipografie cu exemple, spațiere și layout, tone of voice vizual cu exemple de do's și don'ts. Distribuie ghidul tuturor celor care produc materiale vizuale (intern și agenții externe). Versiunile fișierelor sursă se stochează în cloud cu acces controlat.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-055 executată: Brand identity design complet — logo, paletă culori, tipografie, design system și brand visual guidelines livrate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-055",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["brand-identity", "visual-design", "logo", "design-system", "brand-guidelines"],
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
- Logo livrat fără variante multiple și fișiere vectoriale → violation
- Paletă de culori definită fără coduri HEX/RGB/CMYK → violation
- Brand visual guidelines create dar nedistribuite echipei → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Logo disponibil în SVG, PNG transparent și PDF vectorial?
- [ ] Contrast ratio culori testat pentru accesibilitate WCAG AA?

**VK-uri obligatorii:**
1. `✅ [PROC] M-055 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Brand Identity Design (Visual)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Brand Story Framework (M-054) | Fundamentul strategic al identității vizuale |
| Adobe Illustrator / Figma | Software design pentru logo și design system |
| Google Fonts / Adobe Fonts | Sursă tipografie web-safe |
| WCAG Contrast Checker | Verificare accesibilitate culori |
| Brand Style Guide Template | Structura documentului de guidelines |
| Infographic Creation (M-056) | Aplicare practică a design system-ului |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Logo variante livrate | Minimum 3 (complet, simplificat, mono) |
| Timp producție identitate completă | 2-4 săptămâni |
| Brand consistency score | > 90% pe audit vizual trimestrial |
| Adoptare guidelines de echipă | 100% materiale produse respectă guidelines |
