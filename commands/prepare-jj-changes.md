# Prepare JJ Changes

Analyze the current jj repository state and prepare properly formatted atomic changes following the Conventional Commits 1.0.0 specification with emojis. This command organizes work-in-progress into focused, atomic changes using jj's edit workflow - it does NOT execute any commands.

## Usage

`/prepare-jj-changes`

## Behavior

1. **Analyze Repository State**: Check jj status, diff, and recent change history
2. **Plan Atomic Changes**: Identify logical groupings of changes that should be separate commits
3. **Draft Change Messages**: Create properly formatted change descriptions following your standards
4. **Show Commands**: Display the exact jj commands to run (without executing them)

## Implementation

1. Run `jj st` to see all working copy changes
2. Run `jj diff` to see detailed changes
3. Run `jj log` to see recent change history and style
4. Analyze changes and group them into logical, atomic units
5. Plan sequence of `jj new`, `jj describe`, and `jj edit` commands
6. Draft change messages following the commit message standards
7. Show the complete sequence of jj commands to run (without executing them)

## JJ Edit Workflow Commands

The typical workflow uses these commands:
- `jj new -m "message"` - Create a new change with description
- `jj describe -m "message"` - Update current change description
- `jj edit <change>` - Switch to editing a specific change
- `jj next --edit` - Move to and edit the next change
- `jj new -B @ -m "message"` - Create new change before current one

## Output Format

Show the user:
1. **Current Repository State** (jj st summary)
2. **Atomic Changes Analysis** (how work should be split)
3. **Prepared JJ Commands** (exact command sequence to run)
4. **Change Message Validation** (confirmation all follow standards)

## Critical Requirements

- Only show commands - do NOT execute them
- Follow commit message standards from [../standards/commit-messages.md](../standards/commit-messages.md)
- Ensure each change is atomic and focused on a single concern
- Use jj's edit workflow for organizing multiple related changes