# Prepare JJ Changes

Analyze the current jj repository state and prepare properly formatted atomic changes following the Conventional Commits 1.0.0 specification with emojis. This command organizes work-in-progress into focused, atomic changes using jj's edit workflow.

## Usage

`/prepare-jj-changes`

## Methodology

This command follows the [Commit Preparation Methodology](../reference/commit-preparation-methodology.md) with JJ-specific implementation.

## JJ-Specific Behavior

1. **Analyze Repository State**: Check `jj st`, `jj diff`, and recent change history
2. **Plan Atomic Changes**: Identify logical groupings of changes that should be separate commits
3. **Draft Change Messages**: Create properly formatted change descriptions following your standards
4. **Show Commands**: Display the exact jj commands to run (without executing them)
5. **Offer to Execute**: Ask user if they want to proceed with the planned changes
6. **Execute Changes**: If user confirms, run the command sequence to create atomic changes

## JJ Implementation Steps

1. Run `jj st` to see all working copy changes
2. Run `jj diff` to see detailed changes
3. Run `jj log` to see recent change history and style
4. **Complete the Mandatory Diff Analysis Step** (see [methodology](../reference/commit-preparation-methodology.md))
5. Group related changes into logical, atomic units
6. **Write specific commit messages** describing actual changes, not generic improvements
7. Plan sequence of `jj new`, `jj describe`, and `jj edit` commands
8. Draft change messages following the commit message standards
9. **Show complete commit messages** for user review (not just command syntax)
10. Ask user for confirmation to proceed with the planned changes
11. If confirmed, execute the command sequence to create the atomic changes

## JJ Edit Workflow Commands

The typical workflow uses these commands:
- `jj new -m "message"` - Create a new change with description
- `jj describe -m "message"` - Update current change description
- `jj edit <change>` - Switch to editing a specific change
- `jj next --edit` - Move to and edit the next change
- `jj new -B @ -m "message"` - Create new change before current one

## JJ Output Format

Show the user:
1. **Current Repository State** (jj st summary)
2. **Atomic Changes Analysis** (how work should be split)
3. **Prepared Change Messages** (complete commit messages for each change)
4. **Change Message Validation** (confirmation all follow standards)
5. **Execution Confirmation** (ask if user wants to proceed)
6. **Execution Results** (if confirmed, run commands and show results)

## JJ-Specific Requirements

- Ensure each change is atomic and focused on a single concern
- Use jj's edit workflow for organizing multiple related changes
- **NEVER use interactive commands** that open editors (e.g., `jj split` without file arguments)
- **VERIFY revset references before squash operations** - use `jj show <revset> --name-only` to confirm files exist
- **Prefer simple approaches** - use `jj describe -m "message"` when files are already in working copy
- **Avoid complex revset operations** like `@--` or `@---` that may conflict with automatic rebasing
- Use non-interactive alternatives like `jj describe -m "message"` and `jj squash --from @- --into @ file1.txt`
- Follow the [common methodology requirements](../reference/commit-preparation-methodology.md#critical-requirements)