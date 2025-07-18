# Git Commit Message Standards

This document defines the commit message format and standards for all repositories.

## Commit Message Format

All commit messages must follow this format:
```
<emoji> <type>(<scope>): <description>

[optional body with bullet points]

[optional footer(s)]
```

## Commit Types & Emojis

- ✨ **feat**: A new feature
- 🐛 **fix**: A bug fix  
- 📝 **docs**: Documentation only changes
- 💄 **style**: Code style changes (formatting, missing semi-colons, etc)
- ♻️ **refactor**: Code change that neither fixes a bug nor adds a feature
- ⚡️ **perf**: A code change that improves performance
- ✅ **test**: Adding missing tests or correcting existing tests
- 🏗️ **build**: Changes that affect the build system or external dependencies
- 👷 **ci**: Changes to CI configuration files and scripts
- 🔧 **chore**: Other changes that don't modify src or test files
- ⏪️ **revert**: Reverts a previous commit

## Rules

- Use imperative mood (e.g., "add", "fix", "update")
- No capitalization of first word after colon
- No period at the end of description
- Maximum 100 characters for subject line
- Body uses bullet points with "-"
- Maximum 100 characters per line in body
- Reference issues: `Fixes #123`, `Closes #456`, `Resolves #789`

## Critical Requirements

- **NEVER include AI references** in commit messages
- **NEVER mention Claude, AI, assistants, or code generation**
- Write commits as if the human developer authored them completely
- Follow the emoji + type + scope + description format exactly
