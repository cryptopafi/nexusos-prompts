# PromptForge Training Plan

**Versiune**: 2.0 (actualizat 2026-03-19)
**Autor**: Genie + Pafi
**Obiectiv**: Îmbunătățire sistematică a calității prompturilor prin calibrare cu surse externe, fără cargo cult și fără diluat viteza/directness-ul care ne diferențiază.

---

## Filosofia de Training

**Nu copiem ce arată bine. Disecăm de ce funcționează.**

Procesul: primim un prompt extern → îl rulăm cu Opus → comparăm output-ul cu al nostru → extragem PRINCIPIILE, nu pattern-urile literale → asimilăm în PromptForge.

**Regula de aur**: Dacă un element extern produce output mai bun pe o dimensiune concretă (structură, stakeholder awareness, acoperire, measurability) → asimilăm. Dacă produce output mai lung fără valoare adăugată → notăm ca anti-pattern și excludem.

---

## Lecțiile Sesiunii 2026-02-23 (Prompt Genie vs Genie-Opus)

### Ce am testat
- Prompt Genie (extern) vs. Genie-Opus optimizat intern
- Task: audit Altari Agent OS + cross-reference cu arhitectura curentă

### Metrici comparative
| Dimensiune | Genie-Opus | Prompt Genie | Câștigător |
|-----------|-----------|-------------|-----------|
| Viteză | 185s / 38K tokens | 473s / 43K tokens | Genie-Opus (2.5x) |
| Directness | "demo/marketing artifact" ferm | diplomatic | Genie-Opus |
| Agent inflation analysis | 15 reali / 27 umflați (concret) | absent | Genie-Opus |
| Opțiuni arhitecturale | 1 recomandare | 3 opțiuni (O1/O2/O3) | Prompt Genie |
| TL;DR per stakeholder | absent | fiecare secțiune | Prompt Genie |
| JSON summary block | absent | prezent | Prompt Genie |
| Întrebări deschise | absent | 10 explicite cu context | Prompt Genie |
| Estimări efort | "4-6h" vag | FTE-luni min/max + presupuneri | Prompt Genie |
| Rollback criteria | absent | per fază de migrare | Prompt Genie |
| Componente noi propuse | skills layer | n8n + Redis + 3 agenți noi | Prompt Genie |

### Concluzia sesiunii
Prompt Genie aduce **structură formală de deliverable** (bun pentru decizii de echipă, documentare, stakeholderi multipli).
Genie-Opus aduce **viteză + directness + analiză mai ascuțită** (bun pentru decizii rapide, operator solo, verdicts clare).

**Nu sunt în competiție. Sunt pentru contexte diferite.**

### Elemente asimilate în PromptForge v2.0 (Cortex ID: `bca64dee`)
1. Triple Option Pattern (O1/O2/O3) — pentru decizii arhitecturale reale
2. TL;DR per stakeholder pe secțiuni majore — când audiența e mixtă
3. JSON `architecture_summary` block — pentru portabilitate
4. Presupuneri explicite (minim 6) — "Fără X, nu putem decide Y"
5. FTE-month estimates cu presupuneri declarate
6. Rollback criteria per fază de migrare
7. Priority queue explicită pentru conflicte de resurse
8. Coverage percentage quantificat
9. Adversarial framing explicit în instrucțiuni
10. Idempotency keys în API contracts

### Anti-pattern-uri identificate (de EVITAT)
- **Lungime = calitate**: 43K tokens nu e mai bun decât 38K dacă verdictul e mai bland
- **Structură fără context**: 15 secțiuni formale generate fără a ști că bugetul e $250/lună sau că e operator solo
- **Cargo cult de pattern-uri**: Triple Option e util pentru arhitectură, e noise pentru research simplu
- **Diplomatic hedging**: "pare să fie un demo" < "este un demo/marketing artifact"

---

## Cadrul de Training

### Când facem o sesiune de training
- **Trigger natural**: primim un prompt extern de la o sursă credibilă (Prompt Genie, ChatGPT, Claude.ai, comunitate)
- **Frecvență recomandată**: 1-2 ori/săptămână, nu mai mult — overhead prea mare altfel
- **Durata**: 30-45 minute per sesiune (rulare Opus + comparație + asimilare)

### Protoculul sesiunii de training (5 pași)

**Pasul 1: Rulare comparativă**
- Rulăm prompt-ul extern cu Opus pe același task
- Nu modificăm prompt-ul înainte de rulare — testăm exact ce ne-a venit

**Pasul 2: Comparație structurată pe 5 dimensiuni**
```
1. VITEZĂ: tokens + secunde (măsurate din usage)
2. DIRECTNESS: verdict clar vs diplomatic
3. ACOPERIRE: ce acoperă extern ce nu acoperim noi?
4. CONCISENESS: raport valoare/cuvinte
5. APLICABILITATE LA CONTEXTUL NOSTRU: știa promptul extern că suntem operator solo cu $250/lună?
```

**Pasul 3: Extragere principii (nu copy-paste)**
- Nu copiem secțiuni întregi
- Identificăm CE principiu produce avantajul
- Formulăm principiul generic (aplicabil la orice task de același tip)

**Pasul 4: Decizie de asimilare**
```
ASIMILĂM dacă:
  - Produce output measurably mai bun pe o dimensiune specifică
  - Aplicabil la >3 tipuri de task-uri din activitatea noastră
  - Nu crește overhead fără valoare (lungime, complexitate inutilă)

EXCLUDEM dacă:
  - Arată bine dar verdictul e mai vag decât al nostru
  - Presupune echipă mare / buget enterprise / contexte irelevante
  - Produce text mai lung fără informație nouă
  - E "cargo cult" — structură fără substanță
```

**Pasul 5: Update PromptForge + salvare Cortex**
- Update `promptforge.md` cu tehnica nouă
- Salvare în Cortex `procedures` cu tag `promptforge` + versiune
- Update acest fișier cu lecțiile sesiunii

---

## Catalog Tehnici PromptForge (actualizat)

### Tehnici de bază (existente pre-training)
| Tehnică | Trigger | Efect |
|---------|---------|-------|
| Role definition | Întotdeauna | "You are a [role] specializing in [domain]" |
| Chain-of-thought | Raționament complex | "Think step by step" |
| Structured output | Format specificat | Format specification (JSON, table, list) |
| Constraints | Scope neclar | "Do NOT include...", "Focus only on..." |
| Multi-shot examples | Pattern-uri existente | 1-2 exemple din library |
| Model recommendation | Per COST-H-001 | Haiku/Sonnet/Opus bazat pe complexitate |
| Decomposition | Prea complex | Sub-prompturi cu secvențiere |
| Specificity boost | Termeni vagi | "better" → metrici concrete |

### Tehnici noi (asimilate 2026-02-23 din Prompt Genie)
| Tehnică | Trigger | Când NU se aplică |
|---------|---------|-----------------|
| **Triple Option (O1/O2/O3)** | Decizii arhitecturale, alegeri de strategie | Research simplu, task-uri cu răspuns unic |
| **TL;DR per stakeholder** | Audiență mixtă (tehnic + PM) | Comunicare internă Pafi-Genie |
| **JSON summary block** | Output care va fi copiat/folosit programatic | Conversații informale |
| **Presupuneri explicite (6+)** | Informații lipsă critice, decizii cu risc | Task-uri cu context complet |
| **FTE-month estimates** | Planificare proiecte, roadmap-uri | Estimări de câteva ore |
| **Rollback criteria** | Planuri de migrare, deployments | Task-uri non-reversibile |
| **Priority queue** | Conflicte de resurse, sisteme multi-task | Sisteme single-user simple |
| **Coverage percentage** | Comparații sistem vs sistem | Audituri single-system |
| **Adversarial framing** | Validare arhitecturi, security review | Creative tasks, content |
| **Idempotency în API** | Contract API design | Documentare internă |

---

## Surse de Training Recomandate

### Surse de înaltă calitate (folosit deja)
- **Prompt Genie** — bun pentru structură formală, deliverabile enterprise

### Surse de explorat (plan)
- Anthropic prompt library (oficial) — pentru tehnici Claude-specifice
- LearnPrompting.org — pentru tehnici generale, multi-model
- r/ChatGPT / r/ClaudeAI — pattern-uri emergente din comunitate
- Promptbase.com — pattern-uri specializate per domeniu

### Surse de evitat
- Prompt-uri virale fără context — arată bine pe Twitter, nu funcționează în producție
- Prompt-uri pentru GPT-4 fără adaptare — modelele se comportă diferit
- "Mega-prompturi" de 2000+ cuvinte — overhead vs valoare negativ

---

## Anti-pattern-uri Documentate

### Anti-pattern 1: Lungime = Calitate (din sesiunea 2026-02-23)
**Simptom**: Prompt mai lung → output mai lung → pare mai bun
**Realitate**: 43K tokens / 473s vs 38K tokens / 185s — verdictul mai rapid era mai direct
**Regulă**: Dacă output-ul extern e >20% mai lung fără informație nouă = anti-pattern

### Anti-pattern 2: Cargo Cult de Structură
**Simptom**: Adăugăm Triple Option la orice pentru că "arată profesional"
**Realitate**: Triple Option e util pentru arhitecturi. E noise pentru "ce cărți să citesc despre SEO?"
**Regulă**: Fiecare tehnică nouă asimilată primește o coloană "Când NU se aplică"

### Anti-pattern 3: Context Ignorat
**Simptom**: Prompt Genie a propus n8n + Redis fără să știe bugetul și stack-ul actual
**Realitate**: Un prompt fără context specific produce recomandări generice
**Regulă**: Înainte de orice optimizare, Genie injectează contextul sistemului (budget, stack, team size)

### Anti-pattern 4: Diplomatic Hedging
**Simptom**: "Pare să fie un marketing artifact" în loc de "Este un marketing artifact"
**Realitate**: Verdictele clare reduc cognitive load. Diplomația e pentru relații, nu pentru analiză tehnică.
**Regulă**: Verdictele tehnice trebuie să fie ferme. "Cred că" și "poate că" sunt interzise în outputs tehnice.

### Anti-patterns 5-25+ (Sessions 6-10)
25 anti-patterns documentate din Sessions 6-10. Full catalog: `~/.nexus/prompt-library/patterns/PATTERN-INDEX.md` §Anti-Patterns Summary. Key additions:
- **AP-5 Personality Padding**: Excessive persona traits that don't affect output quality
- **AP-6 Technique Stuffing**: Applying >3 techniques per cycle (diminishing returns)
- **AP-7 Model-Agnostic Prompting**: Same prompt for all models ignoring capabilities
- **AP-8 Over-constraining Creativity**: Too many rules kill creative output

---

## Roadmap Training

### Completat
- [x] Sesiunea 1 (2026-02-23): Prompt Genie vs Genie-Opus pe audit arhitectură
  - 10 tehnici asimilate, 4 anti-pattern-uri documentate
  - PromptForge v2.0 în Cortex (ID: `bca64dee`)

### Planificat
- [x] Sesiunea 2: Prompt de research/intelligence (Perplexity-style vs Genie)
  - Target: îmbunătățim Research & Intelligence Agent prompts
  - Sursă propusă: Anthropic prompt library pentru research
- [x] Sesiunea 3: Prompt de content creation (Grok-style vs Genie)
  - Target: îmbunătățim Content & Social Agent pentru social media
  - Sursă propusă: prompt-uri optimizate pentru viral content
- [x] Sesiunea 4: Prompt de code review / security audit
  - Target: îmbunătățim Deep Audit Agent
  - Sursă propusă: prompt-uri de securitate din OWASP community
- [x] Sesiunea 5: Prompt de financial analysis
  - Target: îmbunătățim Finance Agent pentru rapoarte RON/EUR
  - Sursă propusă: prompt-uri CFA/CFO din comunitatea finance

### Completat — Sessions 6-10 (2026-03-19, Mega-Analysis pe 620+ prompturi)

Prompt Library crescută de la 160 la 620+ prompturi (Cortex collection `technical`, tag `prompting-library`).

- [x] **Session 6**: Agentic System Prompts (GPT-5, Windsurf, Manus, v0, Bolt, Cline, Claude Code) → **11 patterns noi (P25-P35)**: Anti-Affirmation, Tiered Autonomy, Ephemeral Memory, Single Action, Identity Anchor, Proactive State, No-Estimate, Implicit Context, Conciseness Hard, Scope Immutability, XML Sectioning
- [x] **Session 7**: Marketing + Business + SEO (~80 prompturi) → **12 patterns (P36-P47)**: Awareness Segmentation, Multi-Framework Copy, Repurposing Matrix, Role-with-Metric, Comparative Version, Business Framework Sequencer, Platform Constraints, Emotional Driver, SEO Intent-Format, Persona-Voice, Objection Pre-Emption, Decision Framework
- [x] **Session 8**: Coding + Technical (~60 prompturi) → **7 patterns (P48-P54)**: Severity-Tiered Feedback, Spec-First Code-Second, Debug RIHV, Progressive Complexity, Before/After Contrast, Language Constraints, Output-Only JSON
- [x] **Session 9**: Creative + Writing + Education (~70 prompturi) → **8 patterns (P55-P62)**: Show-Don't-Tell, Voice Capture Transfer, Story Engine Want/Need, Feynman Gap Detector, Spaced Retrieval, Content Unbundling, Pedagogical Scaffolding, Self-Consistency Synthesis
- [x] **Session 10**: Web Design + UI + Persona (~160 prompturi) → **7 patterns (P63-P69)**: Component Matrix, Library Mandate, Persona Activation Stack, Visual Quality Tokens, Design Token Parameterization, Negative Image Scaffolding, Persona-to-Agent Promotion

**Total**: 45 patterns noi (P25-P69) + 25 anti-patterns. PromptForge updated to v3.7.
Full index: `~/.nexus/prompt-library/patterns/PATTERN-INDEX.md`

### Planificat — Sessions 11-15

- [ ] Session 11: Production prompt optimization via SOL (top 5 from queue.json)
- [ ] Session 12: Cross-model comparison (Claude vs Gemini prompt patterns)
- [ ] Session 13: Long-context prompt patterns (100K+ token inputs)
- [ ] Session 14: Multi-modal prompt patterns (vision + text)
- [ ] Session 15: Prompt caching optimization patterns

### Criterii de succes training — STATUS (la 3 luni → EXCEEDED la 3 săptămâni)
- ✅ PromptForge library: 20+ tehnici → **69 patterns documented** (345% of target)
- ✅ Anti-pattern catalog: 10+ → **25+ anti-patterns** (250% of target)
- ✅ Agent templates testate: **12 agent prompts + 5 SOULs** optimized
- 🟡 Retry rate <5%: TBD — needs production measurement via SOL cycles

---

## Note de Sesiune

### 2026-02-23 — Sesiunea 1: Prompt Genie Benchmark
**Context**: Pafi a primit un prompt structurat de la Prompt Genie pentru audit arhitectural.
**Finding principal**: Prompt Genie excelează la structură formală de deliverable (enterprise-ready), Genie-Opus excelează la viteză și directness (operator-solo-ready).
**Decizie**: Nu înlocuim abordarea noastră — o completăm cu tehnicile de structurare pentru contexte cu stakeholderi multipli.
**Asimilat**: 10 tehnici → PromptForge v2.0 (Cortex `bca64dee`)
**Exclus**: Diplomatic hedging, lungime artificială, context-agnostic recommendations.
