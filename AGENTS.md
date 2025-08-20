# AGENT.md

This file provides guidance to AI coding agents.

## Radicle Commands (CRITICAL)

**ALWAYS use these exact commands for Radicle issue management:**

```bash
# Create issues
rad issue open --title "Title" --description "Description" --label "priority:high"

# Comment on issues  
rad issue comment <issue-id> --message "Comment text" --no-announce

# Close issues (NOT "rad issue close")
rad issue state <issue-id> --closed --no-announce

# Assign issues
rad issue assign --add $(rad self --did) --no-announce <issue-id>
rad issue assign --delete $(rad self --did) --no-announce <issue-id>

# Label management
rad issue label --add "status:in-progress" --no-announce <issue-id>
rad issue label --delete "status:in-progress" --no-announce <issue-id>
```

**Key Points:**
- Use `rad issue state <issue-id> --closed` (NOT `rad issue close`)
- Always include `--no-announce` flag for local-only operations
- Use `$(rad self --did)` for self-assignment

## Python Coding Standards

See [Python Coding Standards](standards/python-coding.md) for complete requirements.

## Git Commit Message Standards

See [Git Commit Message Standards](standards/commit-messages.md) for complete requirements.

## Documentation Standards

See [Documentation Standards](standards/documentation.md) for complete requirements.

## Development Workflow

See [Development Workflow Standards](standards/workflow.md) for complete requirements.

