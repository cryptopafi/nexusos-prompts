---
id: PR-038
title: "Program-of-Thoughts"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-038: Program-of-Thoughts

## Obiectiv

Program-of-Thoughts (Chen et al., 2023) folosește LLM-uri pentru a genera cod de programare ca pași de raționament, în loc de text natural. Codul e apoi executat pentru a produce răspunsul. Avantaj: eliminarea erorilor aritmetice (calculatorul nu greșește) și raționament verificabil (codul e executabil).

## Pași

### Pas 1: Cere modelului să genereze cod, nu text

**Template:**
```
Solve this problem by writing Python code. The code should compute the answer step by step.

Problem: A company has 3 departments. Department A has 45 employees with average salary $65,000. Department B has 30 employees with average salary $72,000. Department C has 25 employees with average salary $58,000. What is the company's total payroll and overall average salary?

```python
# Department data
dept_a_employees = 45
dept_a_salary = 65000
dept_b_employees = 30
dept_b_salary = 72000
dept_c_employees = 25
dept_c_salary = 58000

# Calculate payrolls
payroll_a = dept_a_employees * dept_a_salary
payroll_b = dept_b_employees * dept_b_salary
payroll_c = dept_c_employees * dept_c_salary

total_payroll = payroll_a + payroll_b + payroll_c
total_employees = dept_a_employees + dept_b_employees + dept_c_employees
average_salary = total_payroll / total_employees

print(f"Total payroll: ${total_payroll:,}")
print(f"Average salary: ${average_salary:,.2f}")
```
```

### Pas 2: Execută codul
Rulează codul generat într-un sandbox Python. Output-ul e răspunsul final.

```python
import subprocess

def program_of_thought(problem):
    # Generate code
    code = llm(f"Write Python code to solve: {problem}")
    # Execute in sandbox
    result = subprocess.run(
        ["python", "-c", code],
        capture_output=True, text=True, timeout=10
    )
    return result.stdout
```

### Pas 3: Furnizează exemplu Few-Shot
```
Problem: If you invest $1000 at 5% annual compound interest, how much do you have after 10 years?

```python
principal = 1000
rate = 0.05
years = 10
amount = principal * (1 + rate) ** years
print(f"${amount:.2f}")
```
Output: $1628.89

Problem: {NEW_PROBLEM}

```python
```

### Pas 4: Gestionează cazuri non-numerice
Pentru raționament logic, generează cod cu assertions sau boolean checks:
```python
# Logic problem: All cats are animals. Tom is a cat. Is Tom an animal?
class Entity:
    def __init__(self, name, categories):
        self.name = name
        self.categories = set(categories)

cat_properties = {"animal"}  # All cats are animals
tom = Entity("Tom", {"cat"} | cat_properties)

answer = "animal" in tom.categories
print(f"Is Tom an animal? {answer}")  # True
```

### Pas 5: Verifică codul înainte de execuție
Scanează pentru: imports periculoase, system calls, infinite loops, network access. Sandbox-ul trebuie restricționat.

### Pas 6: Combină cu CoT text
Pentru output explicativ: generează ATÂT cod CÂT ȘI text explicativ.
```
Solve this with code AND explain your reasoning in natural language.

Problem: {PROBLEM}

Reasoning: [natural language explanation]
Code verification:
```python
[code that confirms the reasoning]
```
```

## Verificare

- [ ] Codul generat e valid și executabil
- [ ] Codul e executat în sandbox securizat
- [ ] Output-ul corespunde cu așteptările
- [ ] Nu conține operații periculoase
- [ ] Variabilele au nume descriptive (readability)
- [ ] Rezultatul numeric e corect (verificat)

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus/Sonnet | Excelent — cod Python curat |
| GPT-4/5 | Excelent |
| Code-specialized models | Ideal |
| Python sandbox | Necesar pentru execuție |

## Note

- **Când să folosești**: Math, statistică, financial calculations, orice task cu calcule precise; data analysis.
- **Când NU**: Task-uri pur textuale/creative; când nu ai sandbox de execuție; raționament pur conceptual.
- **Greșeli comune**: Cod generat fără execuție (pierde avantajul); sandbox nesecurizat; cod complex ne-verificat.
- **Relație**: PR-022 (CoT text), PR-039 (Faithful CoT), PR-034 (DECOMP cu funcții).
