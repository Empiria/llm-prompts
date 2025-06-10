<!--
SPDX-License-Identifier: MIT
Copyright (c) Empiria Ltd
This file is part of the LLM Prompts project, published at https://github.com/empiria/llm-prompts
-->

# Pythonic Code Generation Guidelines

## Objective
Generate clean, readable, and maintainable Python code adhering to "pythonic" principles as defined by expert Python programmers and PEP standards. Prioritize idiomatic constructs, simplicity, and clarity.

---

## What is *Pythonic* Code?
Code that uses Python's unique features and design philosophy effectively, as outlined in:
- **PEP 8**: Style Guide for Python Code (layout, naming, formatting)
- **PEP 20**: The Zen of Python (readability, simplicity)
- **PEP 257**: Docstring Conventions
- **PEP 484**: Type Hints

### Key Principles with Examples

#### 1. **Readability First**
   - Use clear variable/function names (e.g., `user_data` instead of `ud`).
   - Break complex operations into simple steps.
   - Include docstrings for public modules/classes/functions.

   ```python
   # Non-Pythonic
   def f(x): return x**2 + 3*x + 2

   # Pythonic
   def quadratic_equation(x: float) -> float:
       """Calculate the value of x² + 3x + 2."""
       return x ** 2 + 3 * x + 2
   ```

#### 2. **Use Built-in Constructs**
   - Prefer list/dictionary comprehensions over manual loops.
   - Use `enumerate()`, `zip()`, and context managers (`with`).

   ```python
   # Non-Pythonic
   squares = []
   for i in range(10):
       squares.append(i**2)

   # Pythonic
   squares = [i**2 for i in range(10)]

   # With context manager (PEP 343)
   with open('file.txt') as file:
       data = file.read()
   ```

#### 3. **EAFP over LBYL**
   - *"Easier to Ask Forgiveness than Permission"*: Use `try/except` for expected errors.

   ```python
   # Non-Pythonic (LBYL)
   if key in my_dict:
       value = my_dict[key]
   else:
       value = default

   # Pythonic (EAFP)
   try:
       value = my_dict[key]
   except KeyError:
       value = default
   ```

#### 4. **Follow PEP 8 Standards**
   - Indent with 4 spaces.
   - Limit lines to 88 characters (Ruff default for line length).
   - Use underscores for variables/functions (`snake_case`), CamelCase for classes.

#### 5. **Type Hints (PEP 484)**
   ```python
   def greet_all(names: list[str]) -> None:
       for name in names:
           print(f"Hello, {name}")
   ```

#### 6. **Use the Standard Library**
   - Use `pathlib` instead of manual path handling.
   - Choose `collections.defaultdict` or `dataclasses` where appropriate.

#### 7. **Minimize Global State**
   Avoid mutable global variables; use classes or function parameters.

---

#### 8. **Lambda Expression Preferences**
Use lambda judiciously with these guidelines:
- **Avoid naming lambdas**: Use `def` for named functions
- **Prefer operator module**: Use `operator.itemgetter`, `attrgetter`, `methodcaller` instead of trivial lambdas
- **Use comprehensions**: Better than `map`/`filter` with lambda 
- **Keep trivial**: Only for simple one-line operations
- **Avoid complex logic**: Use named functions for anything non-trivial

**When to use**:
- Simple key functions in sorted/max/min
- Trivial one-off predicates for filter()
- Short-lived callback handlers

**When to avoid**:
```python
# Non-Pythonic: named lambda
sort_key = lambda x: x[1]  # Use operator.itemgetter(1) instead

# Non-Pythonic: redundant lambda 
sorted(nums, key=lambda n: abs(n))  # Use key=abs

# Non-Pythonic: complex logic
sorted(pairs, key=lambda p: (p[0]//100, p[1].lower()))
```

**Pythonic alternatives**:
```python
from operator import itemgetter, attrgetter, methodcaller

# Use built-in functions
sorted(nums, key=abs)

# Use operator module 
sorted(pairs, key=itemgetter(1))
sorted(objs, key=attrgetter('name'))
sorted(strs, key=methodcaller('casefold'))

# Use comprehensions 
squares = (x**2 for x in numbers)  # Instead of map+lambda
odds = (x for x in nums if x % 2)  # Instead of filter+lambda
```

---

#### 9. **Composition Over Inheritance**
Prefer object composition over class inheritance to promote code reuse and flexible designs. Follow these guidelines:

- **Favor "has a" relationships**: Use composition when one class contains another (e.g. `Car` has an `Engine`)
- **Avoid deep hierarchies**: Limit inheritance chains to 1-2 levels; use composition for additional features
- **Encapsulate behavior**: Create small, focused classes that can be combined
- **Use interfaces**: Define clear protocols for interaction between components
- **Favour 'has a' relationships**: Use composition when one class contains another (e.g. `Car` has an `Engine`)
- **Avoid deep hierarchies**: Limit inheritance chains to 1-2 levels; use composition for additional features
- **Encapsulate behaviour**: Create small, focused classes that can be combined

**When to use composition**:
- Modeling parts/features of an object
- Runtime behavior changes needed
- Sharing functionality between unrelated classes
- Avoiding tight coupling of inheritance

**Non-Pythonic (inheritance)**:
```python
# Fragile base class problem
class Manager(Employee, ProductivityRole, PayrollPolicy):
    # Multiple inheritance complexity
    pass
```

**Pythonic (composition)**:
```python
class Manager:
    def __init__(self):
        self.role = ProductivityRole()
        self.payroll = PayrollPolicy()
        self.contact = ContactInfo()

    def perform_work(self, hours):
        return self.role.perform_duties(hours)

    def calculate_pay(self):
        return self.payroll.calculate()
```

**Key benefits**:
- Easier maintenance and testing
- More flexible class design
- Reduced dependency between components
- Clearer separation of concerns
- Standardised spelling for British English readers

---

#### 10. **Declarative Over Imperative**
Prefer declarative patterns that describe *what* over *how*, using these techniques:

**a. Dictionary Dispatch**  
Replace conditional logic with dictionary lookups:
```python
# Non-Pythonic
def handle_operation(op, a, b):
    if op == 'add':
        return a + b
    elif op == 'sub':
        return a - b
    elif op == 'mul':
        return a * b
    else:
        raise ValueError

# Pythonic
def handle_operation(op, a, b):
    from operator import add, sub, mul
    operations = {
        'add': add,
        'sub': sub, 
        'mul': mul
    }
    return operations[op](a, b)
```

**b. Comprehension Filtering**  
Use built-in filtering instead of conditional loops:
```python
# Non-Pythonic
results = []
for x in data:
    if x % 2 == 0 and x > 10:
        results.append(x*2)

# Pythonic
results = [x*2 for x in data if x % 2 == 0 and x > 10]
```

**c. Ternary Expressions**  
For simple value assignments:
```python
# Non-Pythonic
if score > 50:
    status = 'pass'
else:
    status = 'fail'

# Pythonic
status = 'pass' if score > 50 else 'fail'
```

**d. Structural Pattern Matching (Python 3.10+)**  
For complex conditional structures:
```python
match response:
    case {'status': 200, 'data': [*items]}:
        process(items)
    case {'status': 404}:
        handle_not_found()
    case {'status': 418}:
        raise TeapotError()
    case _:
        handle_unknown()
```

**When to use if/else**:
- When dealing with mutually exclusive conditions
- When logic requires complex side effects
- When pattern matching improves readability over nested conditionals

---

## Checklist for Generated Code
Ensure all code:
- Passes the Ruff linter and formatter.
- Uses type hints for function signatures.
- Includes docstrings for public functions/classes.
- Prefers f-strings (PEP 498) over `%`/`format()`.
- Avoids nested list comprehensions or overly dense one-liners.
- Uses `if __name__ == "__main__":` for script execution.

---

