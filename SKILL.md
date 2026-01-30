---
name: update-context
description: Capture conversation context and create/update markdown documentation organized by feature or bug type. Use when documenting architectural decisions, bug analysis, feature implementations, or design discussions.
argument-hint: [context-type] [optional-name]
disable-model-invocation: false
allowed-tools: Write, Read, Glob, Bash, Edit
---

# Update Context Skill

Maintain persistent context documentation by capturing conversation insights and organizing them into markdown files.

## When to Use This Skill

Use this skill to document:
- **Features**: New feature implementations and requirements
- **Bugs**: Bug analysis, root causes, and fixes
- **Architecture**: Architectural decisions and design patterns
- **Refactors**: Refactoring efforts and outcomes
- **Performance**: Performance improvements and optimizations
- **Security**: Security considerations and implementations

## Usage

### Automatic Invocation
During technical conversations, I may proactively use this skill to capture important context, decisions, and implementation details.

### Manual Invocation
```bash
# Document a feature
/update-context feature my-feature-name

# Document a bug analysis
/update-context bug issue-123

# Document an architectural decision
/update-context architecture auth-system-redesign

# Document a refactor
/update-context refactor state-management

# Document a performance improvement
/update-context performance query-optimization

# Document a security implementation
/update-context security rate-limiting
```

## Documentation Structure

Documentation is organized in the project's `.claude/context-docs/` directory:

```
.claude/context-docs/
├── features/
│   └── [feature-name].md
├── bugs/
│   └── [bug-id].md
├── architecture/
│   └── [decision-name].md
├── refactors/
│   └── [description].md
├── performance/
│   └── [optimization-name].md
└── security/
    └── [security-feature].md
```

## What Gets Captured

Each documentation file includes:

- **Overview**: Clear description of what this is about
- **Problem/Goal**: What are we solving?
- **Solution/Implementation**: How did we solve it?
- **Key Decisions**: Why did we choose this approach?
- **Tradeoffs**: Benefits and costs
- **Related Files**: Which files are affected?
- **Testing**: Verification approach
- **Status**: Current state (In Progress, Complete, etc.)
- **Date**: When was this documented?

## Template Structure

```markdown
# [Title]

**Type**: Feature | Bug | Architecture | Refactor | Performance | Security
**Date**: YYYY-MM-DD
**Status**: In Progress | Complete | Blocked | Review

## Overview
[Clear description]

## Problem/Goal
[What are we solving?]

## Solution/Implementation
[How did we solve it?]

## Key Decisions
- [Decision 1 and rationale]
- [Decision 2 and rationale]

## Tradeoffs
- **Pros**: [Benefits]
- **Cons**: [Costs/Downsides]

## Related Files
- `path/to/file1.ts` - [Purpose]
- `path/to/file2.tsx` - [Purpose]

## Testing / Verification
- [ ] Unit tests
- [ ] E2E tests
- [ ] Manual testing

## Notes
[Additional context, gotchas, future improvements]
```

See [template.md](template.md) for the full detailed template.

## Implementation Steps

When this skill is invoked, I will:

1. **Parse arguments**: Extract context type ($0) and optional name ($1)
2. **Ensure directory exists**: Create `.claude/context-docs/[type]/` if needed
3. **Generate filename**: Use provided name or derive from conversation
4. **Check for existing file**: Read existing content if updating
5. **Capture context**: Extract relevant information from conversation
6. **Structure content**: Use template format for consistency
7. **Write/update file**: Save to appropriate location
8. **Confirm**: Report back with file path and summary

## Arguments

- **$0** (required): Context type
  - Values: `feature`, `bug`, `architecture`, `refactor`, `performance`, `security`
- **$1** (optional): Name/identifier for the documentation file
  - If not provided, I'll derive a name from the conversation context
  - Examples: `shopping-cart`, `issue-123`, `auth-redesign`

If no arguments are provided, I'll ask you what type of context to document.

## Examples

### Example 1: Document a Feature
```
/update-context feature data-grid-qis
```
Creates: `.claude/context-docs/features/data-grid-qis.md`

### Example 2: Document a Bug Fix
```
/update-context bug connection-modal-crash
```
Creates: `.claude/context-docs/bugs/connection-modal-crash.md`

### Example 3: Document Architecture Decision
```
/update-context architecture websocket-communication
```
Creates: `.claude/context-docs/architecture/websocket-communication.md`

### Example 4: Update Existing Documentation
If the file already exists, I'll read it first and merge/update the content with new information from the conversation.

## Tips

- **Use descriptive names**: Choose clear, searchable names for your documentation
- **Document as you go**: Invoke this skill during or right after implementation discussions
- **Review later**: These docs serve as great onboarding material and decision logs
- **Update when needed**: Re-run the skill with the same name to append new findings
- **Commit to git**: Include `.claude/context-docs/` in your repository for team visibility

## Integration with Development Workflow

This skill complements your existing workflow:
- After implementing a feature → Document it
- After fixing a bug → Capture root cause analysis
- During architecture discussions → Record decisions
- Before refactoring → Document the plan
- After performance improvements → Note the results

The documentation becomes a living record of your project's evolution.
