# Python Coding Standards

All Python code must follow these Pythonic principles and standards:

## Core Principles

1. **Readability First**: Use clear variable/function names, break complex operations into simple steps, include docstrings for public modules/classes/functions.

2. **Type Hints**: Always use type hints for function signatures (PEP 484):

   ```python
   def greet_all(names: list[str]) -> None:
       for name in names:
           print(f"Hello, {name}")
   ```

3. **PEP 8 Compliance**:

   - Indent with 4 spaces
   - Limit lines to 88 characters (Ruff default)
   - Use snake_case for variables/functions, CamelCase for classes

4. **Use Built-in Constructs**: Prefer list/dictionary comprehensions, `enumerate()`, `zip()`, context managers (`with`)

5. **EAFP over LBYL**: Use try/except for expected errors rather than checking conditions first

## Lambda Guidelines

- **Avoid naming lambdas**: Use `def` for named functions
- **Prefer operator module**: Use `operator.itemgetter`, `attrgetter`, `methodcaller` instead of trivial lambdas
- **Use comprehensions**: Better than `map`/`filter` with lambda
- **Keep trivial**: Only for simple one-line operations

## Design Patterns

6. **Composition Over Inheritance**:

   - Favor "has a" relationships over "is a"
   - Limit inheritance chains to 1-2 levels
   - Use composition for additional features

7. **Declarative Over Imperative**:
   - Use dictionary dispatch instead of long if/elif chains
   - Use comprehension filtering
   - Use ternary expressions for simple assignments
   - Use structural pattern matching (Python 3.10+) for complex conditionals

## Code Quality Requirements

All Python code must:

- Pass Ruff linter and formatter
- Pass basedpyright type checking
- Include docstrings for public functions/classes
- Use f-strings over `%`/`format()`
- Avoid nested list comprehensions or overly dense one-liners
- Use `if __name__ == "__main__":` for script execution
- Minimize global state - use classes or function parameters instead

## Linting Commands

Run these commands to ensure code quality:

```bash
ruff check --fix .
basedpyright
```

All code must pass both linters with zero errors and warnings before being committed.