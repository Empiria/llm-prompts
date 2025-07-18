# Prepare Git Commits

Analyze the current git repository state and prepare properly formatted commit messages following the Conventional Commits 1.0.0 specification with emojis.

## Usage

`/prepare-commits`

## Methodology

This command follows the [Commit Preparation Methodology](../reference/commit-preparation-methodology.md) with Git-specific implementation.

## Git-Specific Behavior

1. **Analyze Repository State**: Check git status, diff, and recent commit history
2. **Draft Commit Messages**: Create properly formatted commit messages following your standards
3. **Show Commands**: Display the exact git commands to run (without executing them)
4. **Offer to Execute**: Ask user if they want to proceed with the planned commits
5. **Execute Commits**: If user confirms, run the command sequence to create the commits

## Git Implementation Steps

1. Run git status to see all untracked files and modifications
2. Run git diff to see staged and unstaged changes  
3. Run git log to see recent commit message style
4. **Complete the Mandatory Diff Analysis Step** (see [methodology](../reference/commit-preparation-methodology.md))
5. Group related changes into logical, atomic units
6. **Write specific commit messages** describing actual changes, not generic improvements
7. Draft commit message(s) following the commit message standards
8. Show the git add and git commit commands to run (without executing them)
9. **VERIFY commands before execution**: Show exactly what will be committed
10. Ensure no automated tool references are included in commit messages
11. Ask user for confirmation to proceed with the planned commits
12. If confirmed, execute the command sequence with verification after each step
13. **VERIFY final state**: Run git log to confirm commits were created correctly

## Git Output Format

Show the user:
1. **Current Repository State** (git status summary)
2. **Specific Changes Analysis** (what was added, removed, or modified in each file)
3. **Atomic Commits Planning** (how changes should be grouped)
4. **Prepared Commit Commands** (exact commands to run)
5. **Commit Message Validation** (confirmation it follows standards and avoids generic language)
6. **Execution Confirmation** (ask if user wants to proceed)
7. **Execution Results** (if confirmed, run commands with verification at each step)

## Git-Specific Requirements

- **VERIFY state before and after each operation**
- Handle both staged and unstaged changes appropriately
- Ensure no automated tool references are included in commit messages
- **Include recovery steps** if operations fail or produce unexpected results
- Follow the [common methodology requirements](../reference/commit-preparation-methodology.md#critical-requirements)