# Commit Preparation Methodology

This document describes the common methodology for preparing commits and changes across different version control systems (Git, JJ). It provides the shared analysis framework, requirements, and workflow patterns used by commit preparation commands.

## Mandatory Diff Analysis Step

Before writing any commit messages, complete this analysis:

1. **For each changed file, list exactly what was added:**
   - File: `filename`
   - Added: [specific lines/sections/functions that were added]
   
2. **For each changed file, list exactly what was removed:**
   - File: `filename`  
   - Removed: [specific lines/sections/functions that were removed]
   
3. **For each changed file, list exactly what was modified:**
   - File: `filename`
   - Modified: [specific lines/sections/functions that were changed]

4. **Only after completing steps 1-3, write commit messages that describe these specific changes**

**DO NOT write commit messages until you have completed the line-by-line analysis above.**

## Core Methodology

The commit preparation workflow follows this pattern:

1. **Analyze Repository State**: Check status, diff, and recent history
2. **Complete Mandatory Diff Analysis**: Analyze what was actually changed
3. **Plan Atomic Changes**: Group related changes into logical, atomic units
4. **Write Specific Messages**: Create messages describing actual changes, not generic improvements
5. **Show Commands**: Display exact commands to run (without executing them)
6. **Offer to Execute**: Ask user if they want to proceed with the planned changes
7. **Execute Changes**: If user confirms, run the command sequence

## Critical Requirements

### Message Quality Standards
- Follow commit message standards from [../standards/commit-messages.md](../standards/commit-messages.md)
- **Analyze diff content to write specific, meaningful commit messages**
- **Avoid generic descriptions** like "improve", "enhance", "update"
- Write messages that describe actual changes, not vague improvements

### Workflow Standards
- Show planned commands first, then ask for confirmation before executing
- Ensure each change is atomic and focused on a single concern
- Only execute commands after explicit user confirmation
- Provide clear feedback on execution results

### Safety Standards
- **VERIFY commands before execution**: Show exactly what will be committed
- Ensure no automated tool references are included in commit messages
- **Include recovery steps** if operations fail or produce unexpected results

## Common Output Format

Show the user:
1. **Current Repository State** (status summary)
2. **Specific Changes Analysis** (what was added, removed, or modified in each file)
3. **Atomic Changes Planning** (how changes should be grouped)
4. **Prepared Commands** (exact commands to run)
5. **Message Validation** (confirmation messages follow standards and avoid generic language)
6. **Execution Confirmation** (ask if user wants to proceed)
7. **Execution Results** (if confirmed, run commands with verification at each step)

## Implementation Steps Template

1. Run status command to see all changes
2. Run diff command to see detailed changes  
3. Run log command to see recent commit/change history and style
4. **Complete the Mandatory Diff Analysis Step above**
5. Group related changes into logical, atomic units
6. **Write specific commit messages** describing actual changes, not generic improvements
7. Draft messages following the commit message standards
8. Show the commands to run (without executing them)
9. Ask user for confirmation to proceed with the planned changes
10. If confirmed, execute the command sequence
11. **VERIFY final state**: Confirm changes were created correctly
