---
id: PR-005
title: "Exemplar Label Quality"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-005: Exemplar Label Quality

## Obiectiv

Investigarea impactului calității label-urilor în exemple. Cercetările arată rezultate mixte: unele studii (Min et al.) arată că label-urile pot fi chiar random și modelul tot învață formatul, în timp ce altele arată că label-urile corecte îmbunătățesc semnificativ performanța pe task-uri complexe. Procedura oferă ghid pentru asigurarea calității exemplelor.

## Pași

### Pas 1: Verifică corectitudinea fiecărui label
Înainte de a include exemple, revizuiește manual fiecare label. Un label greșit poate deteriora performanța mai mult decât absența unui exemplu.

### Pas 2: Categorizează task-ul ca format-driven sau label-driven

**Format-driven** (modelul învață structura, nu conținutul):
- Generare de cod, extracție de entități, formatare text
- Label-urile pot fi aproximative — formatul contează mai mult

**Label-driven** (modelul învață mapping-ul corect):
- Clasificare sentiment, categorization, scoring
- Label-urile TREBUIE să fie corecte

### Pas 3: Pentru task-uri label-driven, validează cu expert
```
# CORECT — label-uri validate:
"This movie was a masterpiece of storytelling." → Positive
"I fell asleep halfway through." → Negative

# GREȘIT — label inconsistent:
"The food was terrible and overpriced." → Positive  ← EROARE
```

### Pas 4: Testează sensibilitatea la label quality
Experiment: rulează cu label-uri corecte vs. 1 label greșit. Dacă output-ul se degradează, task-ul e label-sensitive. Dacă nu, e format-driven.

### Pas 5: Prioritizează calitatea peste cantitate
Mai bine 3 exemple cu label-uri verificate decât 7 cu label-uri dubioase.

**Template de audit al exemplelor:**
```
Exemplu 1: Input="..." | Label="..." | Verificat: ✅/❌ | Sursa: manual/auto
Exemplu 2: Input="..." | Label="..." | Verificat: ✅/❌ | Sursa: manual/auto
[...]
```

### Pas 6: Documentează sursa label-urilor
Notează dacă label-urile sunt: manual verificate, generate automat, sau din dataset. Prioritatea: manual > dataset > auto.

## Verificare

- [ ] Fiecare label verificat manual ca corect
- [ ] Task-ul e clasificat ca format-driven sau label-driven
- [ ] Niciun label evident greșit în exemple
- [ ] Dacă label-driven: toate label-urile corecte 100%
- [ ] Sursa label-urilor documentată

## Instrumente

| Model | Note |
|-------|------|
| Toate modelele | Modelele mari sunt mai robuste la label noise, dar label-urile corecte ajută întotdeauna |

## Note

- **Când să folosești**: La orice Few-Shot prompt, ca pas de QA.
- **Când NU**: N/A — calitatea ar trebui verificată mereu.
- **Greșeli comune**: Copy-paste din dataset fără verificare; label-uri ambigue care ar putea fi oricare clasă; presupunerea că modelul "corectează" label-urile greșite.
- **Relație**: Parte din suita PR-001 la PR-008 de design decisions Few-Shot.
