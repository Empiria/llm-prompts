# Documentation Standards

## Comments Policy

Do not add unnecessary comments to code. Code should be self-documenting through clear variable names, function names, and structure. Only add comments when:
- Required for complex algorithms or business logic that isn't obvious
- Documenting public APIs with docstrings/documentation comments
- Explaining why something is done (not what is done)
- Adding TODO/FIXME notes for temporary solutions
- Suppressing type checker or linter warnings (e.g., `# pyright: ignore`, `# noqa: E501`, `// eslint-disable`)
- Language-specific documentation requirements (JSDoc, Rust doc comments, etc.)

Avoid obvious comments like:
- `# Import modules` / `// Import statements`
- `# Remove default handler` / `// Remove handler`
- `# Log to console/stdout` / `// Console logging`
- `# Windows: path explanation` / `// Path handling`
- Comments that just restate what the code does

## Language and Style Guidelines

### Forbidden Words and Phrases

**Never use "leverage" as a verb.** Use clearer alternatives instead:
- Instead of "leverage the API" → "use the API"
- Instead of "leveraging existing code" → "using existing code" or "building on existing code"
- Instead of "leverages X to achieve Y" → "uses X to achieve Y" or "builds on X to achieve Y"

This applies to all writing: code comments, documentation, commit messages, and any generated text.

## Documentation Organization

Documentation should be organized to help users find what they need quickly - whether learning, implementing, or troubleshooting. Structure content into clear categories:

- **tutorials/** - Step-by-step learning guides for beginners
- **guides/** - Task-oriented how-to instructions  
- **reference/** - Technical specifications and API docs
- **explanations/** - Background concepts and design decisions

## Documentation Quality Requirements

All documentation must:
- Be tested and working (especially code examples)
- Use clear, concise language
- Focus on practical value over theoretical frameworks
- Be accessible to the target audience
- Include working examples where appropriate