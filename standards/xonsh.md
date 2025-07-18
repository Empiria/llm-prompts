# Shell Environment Standards

**IMPORTANT**: The user's shell is **xonsh**. Always use xonsh syntax for shell commands:

- Environment variables: `$VAR = "value"` (not `export VAR=value`)
- Command execution: `$(command)` for command substitution
- Background processes: `command &` works the same
- Pipes and redirection work as in bash
- Never use bash-style `VAR=value command` syntax
