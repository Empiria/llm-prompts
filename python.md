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

## Checklist for Generated Code
Ensure all code:
- Passes the Ruff linter and formatter.
- Uses type hints for function signatures.
- Includes docstrings for public functions/classes.
- Prefers f-strings (PEP 498) over `%`/`format()`.
- Avoids nested list comprehensions or overly dense one-liners.
- Uses `if __name__ == "__main__":` for script execution.

---

