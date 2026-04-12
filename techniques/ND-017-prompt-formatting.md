---
id: ND-017
title: "Prompt Formatting and Structure"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-017: Prompt Formatting and Structure

## Obiectiv

Structurarea vizuală a prompt-urilor prin formatare (markdown, delimitatori, secțiuni) pentru a îmbunătăți parsing-ul modelului și calitatea output-ului. Formatarea bună separă clar instrucțiuni de context, input de constrângeri.

## Pași

### Pas 1: Folosește delimitatori pentru secțiuni
```
### Instructions
Analyze the customer feedback below and extract key themes.

### Input
"""
{CUSTOMER_FEEDBACK}
"""

### Output Format
Return a JSON array of themes, each with:
- "theme": string
- "sentiment": "positive" | "negative" | "neutral"
- "frequency": integer (estimated mentions)
```

### Pas 2: Tipuri de delimitatori
```
# Triple quotes (pentru text):
"""
This is the text to analyze.
"""

# XML tags (pentru secțiuni structurate):
<context>Background information here</context>
<task>What to do</task>
<input>The actual data</input>

# Markdown headers:
## Context
## Task
## Constraints

# Dashes/equals (separatori vizuali):
---
Section content
---
```

### Pas 3: Formatare pentru output structurat
Arată exact cum vrei output-ul:
```
Format your response EXACTLY like this:

SUMMARY: [1-2 sentence summary]
CATEGORY: [one of: Bug, Feature, Question, Other]
PRIORITY: [High/Medium/Low]
TAGS: [comma-separated tags]
DETAILS: [2-3 sentences of analysis]
```

### Pas 4: Bullet points și numbered lists
```
# Unordered (când ordinea nu contează):
Key considerations:
- Budget constraints
- Timeline requirements
- Technical feasibility

# Ordered (când ordinea contează):
Steps to implement:
1. Set up the database schema
2. Create the API endpoints
3. Build the frontend components
4. Write integration tests
```

### Pas 5: Tabele pentru comparații
```
Compare these two approaches:

| Aspect | Approach A | Approach B |
|--------|-----------|-----------|
| Cost | | |
| Speed | | |
| Scalability | | |
| Complexity | | |

Fill in each cell with a brief assessment.
```

### Pas 6: Template-uri de formatare per use case
```
# Code review:
## File: {filename}
## Changes: {diff}
## Review:
- **Bugs**: [list]
- **Style**: [list]
- **Suggestions**: [list]

# Meeting notes:
## Date: {date}
## Attendees: {list}
## Decisions:
1. [decision]
## Action Items:
- [ ] [item] — Owner: [name] — Due: [date]
```

## Verificare

- [ ] Secțiuni separate cu delimitatori clari
- [ ] Input delimitat de instrucțiuni (triple quotes, XML tags etc.)
- [ ] Format de output specificat cu exemplu
- [ ] Consistență în formatare pe tot prompt-ul
- [ ] Output-ul modelului respectă formatul cerut

## Instrumente

| Model | Note |
|-------|------|
| Claude | Excelent cu XML tags și markdown |
| GPT-4/5 | Excelent cu markdown |
| Toate | Beneficiază de formatare clară |

## Note

- **Când să folosești**: Orice prompt > 3 rânduri; output structurat; API automation.
- **Când NU**: Prompt-uri de o singură propoziție.
- **Greșeli comune**: Formatare inconsistentă; lipsa delimitatorilor input/instrucțiuni; format de output nespecificat.
- **Relație**: PR-006 (Exemplar Format), ND-008 (Constraints), ND-012 (Instructions).
