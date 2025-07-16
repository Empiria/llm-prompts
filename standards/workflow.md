# Development Workflow Standards

## 🚨 CRITICAL WORKFLOW REQUIREMENTS 🚨

**DOCUMENTATION-FIRST APPROACH - MANDATORY**

**NEVER** make assumptions or guess how things work. **ALWAYS** read documentation first.

## Mandatory Pre-Action Documentation Check

**Before taking ANY action, you MUST:**

1. **Read relevant documentation first** - Check README, CLAUDE.md, setup guides, troubleshooting docs
2. **Search for existing procedures** - Use Task/Grep tools to find established workflows
3. **Understand project patterns** - Learn how the project handles similar tasks
4. **Follow documented procedures** - Use the project's established methods

## Zero-Tolerance Rule

**NEVER:**
- ❌ Guess how commands should work
- ❌ Make assumptions about project structure  
- ❌ Immediately attempt fixes without understanding the system
- ❌ Skip reading documentation because something "seems simple"
- ❌ Try system-wide installations without checking project setup

**ALWAYS:**
- ✅ Read documentation before acting
- ✅ State explicitly: "Let me check the project documentation first"
- ✅ Follow established project patterns
- ✅ Use project-specific environments (virtual environments, containers, etc.)
- ✅ Respect existing workflows and tooling choices

## When Something Fails

**Mandatory failure response protocol:**

1. **STOP immediately** - Do not attempt quick fixes
2. **Read relevant documentation** about the failing component
3. **Understand the intended workflow** from the docs
4. **Follow documented troubleshooting procedures** if available
5. **Only then** attempt fixes using documented patterns

## Documentation Sources Priority Order

1. **Project CLAUDE.md** - Project-specific guidance (highest priority)
2. **README files** - Project overview and setup
3. **Setup/installation guides** - Environment and dependency setup
4. **Development guides** - Workflow and contribution guidelines
5. **API/reference documentation** - Technical specifications

## Explicit Documentation Statements

Before taking significant actions, explicitly state:
- "I need to check the project documentation for the proper procedure"
- "Let me read the setup guide to understand how dependencies are managed"
- "The documentation shows the correct approach is [documented method]"

**VIOLATION OF THESE RULES IS A CRITICAL FAILURE**

This documentation-first approach is not optional. Failing to read documentation before acting wastes time, potentially breaks things, and ignores the careful work put into documenting proper procedures.