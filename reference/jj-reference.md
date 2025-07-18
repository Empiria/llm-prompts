# Jujutsu (jj) Reference

Comprehensive reference for using Jujutsu version control system.

## Core Commands

### Repository State
- `jj st` / `jj status` - Show high-level repo status
- `jj log` - Show revision history
- `jj show` - Show commit description and changes in a revision
- `jj diff` - Compare file contents between two revisions

### Change Management
- `jj new` - Create a new, empty change and edit it in working copy
- `jj describe` / `jj desc` - Update change description or other metadata
- `jj edit` - Set specified revision as working-copy revision
- `jj commit` - Update description and create new change on top

### Moving Changes
- `jj squash` - Move changes from one revision into another
- `jj split` - Split a revision in two
- `jj abandon` - Abandon a revision
- `jj duplicate` - Create new changes with same content as existing ones

### Navigation
- `jj next` - Move working-copy commit to child revision
- `jj prev` - Change working copy revision relative to parent
- `jj edit <change>` - Switch to editing a specific change

### File Operations
- `jj restore` - Restore paths from another revision
- `jj diffedit` - Touch up content changes with diff editor

## The Edit Workflow

The edit workflow is ideal for organizing multiple related changes using jj's edit capabilities:

### Basic Edit Workflow Pattern

1. **Start with a change**: `jj new -m "description"` or `jj describe -m "description"`
2. **Work on your changes**: Edit files normally
3. **Insert changes before current**: `jj new -B @ -m "earlier change"`
4. **Edit the inserted change**: Make your changes
5. **Return to main change**: `jj next --edit`
6. **Continue with main change**: Make final changes

### Key Edit Workflow Commands

- `jj new -B @ -m "message"` - Create new change **before** current one
- `jj next --edit` - Move to and edit the next change
- `jj edit <change>` - Switch to editing a specific change
- `jj new -A @ -m "message"` - Create new change **after** current one

### Automatic Rebasing

When you create changes before existing ones, jj automatically rebases dependent changes. This always succeeds and maintains the change IDs while updating commit hashes.

## Recommended Workflow for Mixed Changes

When you have a working copy with mixed changes that need to be organized into atomic commits:

### The Sequential Commit Method (Most Reliable)

1. **Start from a clean working copy with all your changes**
2. **Create first atomic commit:**
   ```bash
   jj new -m "first atomic change description"
   jj squash --from @- --into @ file1.txt file2.txt
   ```
3. **Create second atomic commit:**
   ```bash
   jj new -m "second atomic change description"  
   jj squash --from @-- --into @ file3.txt file4.txt
   ```
4. **Continue until all changes are organized**
5. **Clean up the original change:**
   ```bash
   jj abandon @---  # Abandon the now-empty original change
   ```

### Why This Works
- Creates new empty changes first
- Pulls specific files FROM the original change INTO each new change
- Leaves remaining files in the original change for the next iteration
- Avoids the complexity of splitting and parallel changes

## Common Patterns

### Organizing Mixed Changes into Atomic Commits

**Pattern 1: Using split (interactive)**
```bash
# Split current change interactively
jj split

# Split specific files into new change
jj split file1.txt file2.txt
```

**Pattern 2: Using squash to move changes (CORRECTED)**
```bash
# Create new change for subset of files
jj new -m "first atomic change"

# Move specific files FROM parent INTO current change
jj squash --from @- --into @ file1.txt file2.txt

# Continue organizing remaining files
jj new -m "second atomic change"
# Move remaining files as needed
```

**Pattern 3: Using edit workflow**
```bash
# Start with current mixed changes
jj describe -m "main change"

# Create change before current for subset
jj new -B @ -m "first atomic change"
# Edit files for first change
# Files automatically rebase to next change

# Return to main change
jj next --edit
# Clean up remaining changes
```

### Moving Files Between Changes

**Move changes from working copy to parent:**
```bash
jj squash --into @- file1.txt file2.txt
```

**Move changes from specific revision:**
```bash
jj squash --from <revision> --into <target> file1.txt file2.txt
```

**Move all changes from revision to parent:**
```bash
jj squash -r <revision>
```

## Advanced Options

### Split Command Options
- `-i, --interactive` - Interactively choose which parts to split
- `--tool <NAME>` - Specify diff editor (implies --interactive)
- `-r, --revision <REVSET>` - The revision to split (default: @)
- `-p, --parallel` - Split into parallel revisions instead of parent/child

### Squash Command Options
- `-r, --revision <REVSET>` - Revision to squash into its parent
- `-f, --from <REVSETS>` - Revision(s) to squash from (default: @)
- `-t, --into <REVSET>` - Revision to squash into (default: @)
- `-i, --interactive` - Interactively choose which parts to squash
- `-k, --keep-emptied` - Don't abandon empty source revision

### New Command Options
- `-B, --insert-before <REVSETS>` - Insert before specified revisions
- `-A, --insert-after <REVSETS>` - Insert after specified revisions
- `-m, --message <MESSAGE>` - Set description without opening editor

## Troubleshooting

### Common Issues

**Split command hanging**: The split command opens an interactive diff editor. If it appears to hang, check if an editor window opened or try setting `--tool` to specify your preferred diff editor.

**Split command limitations**: `jj split` with file arguments may create empty changes if files don't exist in the current revision. Use `jj squash` to move files between changes instead.

**Empty changes**: Use `jj abandon` to remove empty changes, or `jj squash -r <empty-change>` to fold them into their parent.

**Complex rebases**: When jj rebases changes automatically, conflicts may occur. Use `jj log` to see the current state and `jj resolve` to handle conflicts.

## Global Options

- `-R, --repository <REPOSITORY>` - Path to repository to operate on
- `--ignore-working-copy` - Don't snapshot working copy
- `--at-operation <AT_OPERATION>` - Load repo at specific operation
- `--config <NAME=VALUE>` - Additional configuration options
- `--debug` - Enable debug logging
- `--color <WHEN>` - When to colorize output (always, never, debug, auto)

## Revset Syntax

- `@` - Current working copy revision
- `@-` - Parent of current revision
- `@+` - Child of current revision
- `main` - Named branch
- `<change-id>` - Specific change by ID
- `<commit-hash>` - Specific commit by hash

## Tips

1. **Change IDs are stable**: Even when commits are rebased, the change ID remains the same
2. **Automatic rebasing**: jj automatically rebases dependent changes when you modify history
3. **Empty changes**: jj will often abandon empty changes automatically unless you use `--keep-emptied`
4. **Working copy snapshots**: jj snapshots the working copy at the beginning of every command
5. **Undo operations**: Use `jj op log` to see operations and `jj undo` to revert them
