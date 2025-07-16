# Prepare Git Commits

Analyze the current git repository state and prepare properly formatted commit messages following the Conventional Commits 1.0.0 specification with emojis. This command only prepares and shows commit messages - it does NOT create actual commits.

## Usage

`/prepare-commits`

## Behavior

1. **Analyze Repository State**: Check git status, diff, and recent commit history
2. **Draft Commit Messages**: Create properly formatted commit messages following your standards
3. **Show Commands**: Display the exact git commands to run (without executing them)
4. **Follow Standards**: Ensure all commit messages follow your global CLAUDE.md requirements

## Commit Message Format

All commit messages must follow this format:
```
<emoji> <type>(<scope>): <description>

[optional body with bullet points]

[optional footer(s)]
```

### Commit Types & Emojis

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

### Rules

- Use imperative mood (e.g., "add", "fix", "update")
- No capitalization of first word after colon
- No period at the end of description
- Maximum 100 characters for subject line
- Body uses bullet points with "-"
- Maximum 100 characters per line in body
- Reference issues: `Fixes #123`, `Closes #456`, `Resolves #789`

## Implementation

1. Run git status to see all untracked files and modifications
2. Run git diff to see staged and unstaged changes  
3. Run git log to see recent commit message style
4. Analyze changes and determine appropriate type, scope, and description
5. Draft commit message(s) following the exact format requirements
6. Show the git add and git commit commands to run (without executing them)
7. Ensure NO AI references are included in commit messages

## Output Format

Show the user:
1. **Current Repository State** (git status summary)
2. **Proposed Changes Analysis** (what changed and why)
3. **Prepared Commit Commands** (exact commands to run)
4. **Commit Message Validation** (confirmation it follows standards)

## Critical Requirements

- **NEVER include AI references** in commit messages
- **NEVER mention Claude, AI, assistants, or code generation**
- Write commits as if the human developer authored them completely
- Only show commands - do NOT execute them
- Follow the emoji + type + scope + description format exactly