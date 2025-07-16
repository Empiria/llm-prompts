# Code Review Prompt

Please conduct a thorough code review of the provided codebase, evaluating
it against established best practices for software engineering in general and
Python specific coding principles.

## Review Criteria

You must examine, review and report your findings for each and every criterion
listed here.

### Software Engineering

#### DRY - Don't Repeat Yourself

- Extract common logic, but only when you have 3+ instances

#### KISS - Keep It Simple, Stupid

- Favor clarity over cleverness. Boring code is maintainable code

#### SOLID

- Single responsibility
- Open/closed
- Liskov substitution
- Interface segregation
- Dependency inversion

#### YAGNI - You Ain't Gonna Need It

- Build for today's requirements, not tomorrow's maybes

#### OOP - Object Oriented Programming

- Prefer composition over inheritance
- Prefer combining simple behaviours over complex hierarchies

#### Names matter

- Self-documenting names
- No abbreviations
- Obvious intent
- Searchable across codebase

#### Fail Fast

- Validate inputs early
- crash on invariant violations
- make errors obvious

#### Single-purpose functions

- <20 lines ideal
- 20-50 lines, break up
- > 50 lines refactor or split unless ABSOLUTELY necessary.

#### Idempotency

- Operations should be safely repeatable without side effects

#### Global State

- Minimize global state.
- Where it's necessary, avoid mutable global variables. Use classes or function
  parameters.

### Python

#### PEP Standards

- **PEP 8**: Style Guide for Python Code (layout, naming, formatting)
- **PEP 20**: The Zen of Python (readability, simplicity)
- **PEP 257**: Docstring Conventions
- **PEP 484**: Type Hints

#### Use Built-in Constructs

- List/dictionary comprehensions over manual loops
- Proper use of `enumerate()`, `zip()`, and context managers (`with`)

#### EAFP over LBYL

- _"Easier to Ask Forgiveness than Permission"_: Use `try/except` for expected errors

#### Use the Standard Library

- `pathlib` instead of manual path handling
- `collections.defaultdict` or `dataclasses` where appropriate

#### Lambda Expressions

- Avoid naming lambdas (use `def` instead)
- Prefer operator module over trivial lambdas
- Use comprehensions over `map`/`filter` with lambda
- Keep lambdas simple and one-line only

#### Declarative Over Imperative

- Dictionary dispatch instead of complex conditionals
- Comprehension filtering over conditional loops
- Ternary expressions for simple assignments
- Pattern matching for complex structures (Python 3.10+)

## Review Output Format

Please structure your review as follows:

### Summary

Brief overview of code quality and adherence to best practices.

### Areas for Improvement

For each departure from best practice, include:

- Detailed description of the departure. Assume the author is reading the review
  and would not have departed from best practice if they were aware of and
  understood it.
- Importance and rationale.
- Impact and potential risks
- Example code from the codebase and a suggestion of how it could be improved.
  Quote the file names (relative to the repository root) and line numbers for
  the examples.
- Prevalence of the issue. Give a count of how many time it occurs
- References to standards or well-established best practices. Give links to resources
  for the reader to follow where possible.

Present these in order of importance based on the risk, impact and prevalence.

### Appendix

List all locations - file names (relative to the repository root) and line
numbers - for each reported departure.
