# Prepare Git Commits

Analyze the current git repository state and prepare properly formatted commit messages following the Conventional Commits 1.0.0 specification with emojis. This command only prepares and shows commit messages - it does NOT create actual commits.

## Usage

`/prepare-commits`

## Behavior

1. **Analyze Repository State**: Check git status, diff, and recent commit history
2. **Draft Commit Messages**: Create properly formatted commit messages following your standards
3. **Show Commands**: Display the exact git commands to run (without executing them)

## Implementation

1. Run git status to see all untracked files and modifications
2. Run git diff to see staged and unstaged changes  
3. Run git log to see recent commit message style
4. Analyze changes and determine appropriate type, scope, and description
5. Draft commit message(s) following the commit message standards
6. Show the git add and git commit commands to run (without executing them)
7. Ensure no automated tool references are included in commit messages

## Output Format

Show the user:
1. **Current Repository State** (git status summary)
2. **Proposed Changes Analysis** (what changed and why)
3. **Prepared Commit Commands** (exact commands to run)
4. **Commit Message Validation** (confirmation it follows standards)

## Critical Requirements

- Only show commands - do NOT execute them
- Follow commit message standards from [../standards/commit-messages.md](../standards/commit-messages.md)