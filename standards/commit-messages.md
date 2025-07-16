# Git Commit Message Standards

All commit messages must follow the Conventional Commits 1.0.0 specification with these requirements:

## Format
```
<emoji> <type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

## Commit Types

| Type     | Emoji | Description                                                    |
|----------|-------|----------------------------------------------------------------|
| feat     | ✨     | A new feature                                                  |
| fix      | 🐛     | A bug fix                                                      |
| docs     | 📝     | Documentation only changes                                     |
| style    | 💄     | Code style changes (formatting, missing semi-colons, etc)     |
| refactor | ♻️     | Code change that neither fixes a bug nor adds a feature       |
| perf     | ⚡️     | A code change that improves performance                        |
| test     | ✅     | Adding missing tests or correcting existing tests             |
| build    | 🏗️     | Changes that affect the build system or external dependencies |
| ci       | 👷     | Changes to CI configuration files and scripts                 |
| chore    | 🔧     | Other changes that don't modify src or test files             |
| revert   | ⏪️     | Reverts a previous commit                                      |

## Rules

**Subject Line:**
- Use imperative mood (e.g., "add", "fix", "update")
- No capitalization of first word after colon
- No period at the end
- Maximum 100 characters
- Always include appropriate emoji at the beginning

**Body:**
- Use bullet points with "-"
- Maximum 100 characters per line
- Explain what and why, be objective
- Use line breaks for long bullet points without adding extra bullets

**Footer:**
- Reference issues: `Fixes #123`, `Closes #456`, `Resolves #789`
- Breaking changes: `BREAKING CHANGE: description`
- Co-authorship: `Co-authored-by: Name <email>`

## Examples

```
✨ feat(auth): add two-factor authentication support

- implement TOTP-based 2FA using authenticator apps
- add backup recovery codes for account recovery
- integrate with existing user management system

Closes #245
```

```
🐛 fix(api): resolve memory leak in data processing

- fix unclosed database connections in batch processor
- add proper cleanup in error handling paths

Fixes #123
```

All commit messages must be in English and follow these standards exactly.

## 🚨 CRITICAL COMMIT MESSAGE REQUIREMENTS 🚨

**ABSOLUTELY NO AI REFERENCES IN COMMITS - EVER!**

❌ **NEVER INCLUDE:**
- "Generated with Claude Code"
- "Co-Authored-By: Claude" 
- Any mention of AI, Claude, assistants, or automated generation
- References to code generation tools
- Any footers with AI attribution

✅ **ALWAYS:**
- Write commits as if the human developer authored them completely
- Use only the standard conventional commit format with emoji + type + description + body
- Include only legitimate co-authorship (actual human collaborators)
- Keep commit messages authentic and human-authored

**EXAMPLES OF WHAT NOT TO DO:**
```
❌ BAD: Added new feature

🤖 Generated with [AI Tool](https://example.com)
Co-Authored-By: AI Assistant <noreply@example.com>
```

**CORRECT FORMAT:**
```
✅ GOOD: ✨ feat(auth): add two-factor authentication support

- implement TOTP-based 2FA using authenticator apps
- add backup recovery codes for account recovery
- integrate with existing user management system

Closes #245
```

**THIS IS A ZERO-TOLERANCE RULE - NO EXCEPTIONS!**