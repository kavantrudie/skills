---
name: update-context
description: "Capture conversation context AND git changes to create comprehensive markdown documentation. Analyzes both discussed reasoning and actual code changes (via git diff) for accurate, meaningful documentation of features, bugs, architecture, and refactors."
argument-hint: "[context-type] [optional-name]"
disable-model-invocation: false
allowed-tools: "Write, Read, Glob, Bash, Edit"
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

Each documentation file includes insights from both conversation and code changes:

- **Overview**: Clear description of what this is about (from conversation + git analysis)
- **Problem/Goal**: What are we solving? (from conversation context)
- **Solution/Implementation**: How did we solve it? (from conversation + actual code changes in git diff)
- **Key Decisions**: Why did we choose this approach? (from conversation)
- **Code Changes**: Specific files modified and key changes made (from git diff analysis)
- **Tradeoffs**: Benefits and costs (from conversation + code complexity analysis)
- **Related Files**: Which files are affected? (from git diff + conversation)
- **Testing**: Verification approach (from conversation + test file changes)
- **Status**: Current state (In Progress, Complete, etc.)
- **Date**: When was this documented?

The skill intelligently combines:
- **Conversation insights**: "Why" decisions were made, architectural reasoning
- **Code changes**: "What" actually changed, implementation details from git diff

## Template Structure

```markdown
# [Title]

**Type**: Feature | Bug | Architecture | Refactor | Performance | Security
**Date**: YYYY-MM-DD
**Status**: In Progress | Complete | Blocked | Review

## Overview
[Clear description from conversation + git analysis]

## Problem/Goal
[What are we solving?]

## Solution/Implementation
[How did we solve it? - from conversation]

## Code Changes
[If git analysis was performed, detailed breakdown of actual changes:]
- **Files Modified**: List of changed files with change summary
- **Key Changes**: Important code additions, modifications, or refactors
- **Patterns Used**: Design patterns or architectural approaches visible in code
- **Dependencies**: New packages or libraries added

## Key Decisions
- [Decision 1 and rationale]
- [Decision 2 and rationale]

## Tradeoffs
- **Pros**: [Benefits]
- **Cons**: [Costs/Downsides]

## Related Files
- `path/to/file1.ts` - [Purpose and changes made]
- `path/to/file2.tsx` - [Purpose and changes made]

## Testing / Verification
- [ ] Unit tests
- [ ] E2E tests
- [ ] Manual testing
[Include test file changes if present in git diff]

## Notes
[Additional context, gotchas, future improvements]
```

See [template.md](template.md) for the full detailed template.

## Implementation Steps

When this skill is invoked, I will:

1. **Parse arguments**: Extract context type ($0) and optional name ($1)
2. **Check git changes**: Run `git status` to see if there are any changes
3. **Ask user preference**: If changes exist, ask whether to analyze:
   - **Staged changes only** (`git diff --cached`) - cleaner, focused on what's ready to commit
   - **All changes** (`git diff HEAD`) - includes both staged and unstaged changes
   - **Skip git analysis** - use conversation context only
4. **Analyze git changes**: If user wants git analysis, run the appropriate git diff command and analyze:
   - Files modified
   - Nature of changes (added features, bug fixes, refactors)
   - Key code changes and patterns
5. **Ensure directory exists**: Create `.claude/context-docs/[type]/` if needed
6. **Generate filename**: Use provided name or derive from conversation/git changes
7. **Check for existing file**: Read existing content if updating
8. **Capture context**: Extract relevant information from:
   - Conversation history
   - Git diff analysis (if available)
   - File changes and patterns
9. **Structure content**: Use template format for consistency, integrating both conversation and code change insights
10. **Write/update file**: Save to appropriate location
11. **Confirm**: Report back with file path and summary

## Git Changes Analysis

When invoked, the skill will:

1. **Check for changes**: Run `git status` to see if there are uncommitted changes
2. **Prompt for preference**: If changes exist, ask you to choose:
   - **Staged changes only**: Analyzes `git diff --cached` - Best for documenting specific commits or focused changes
   - **All changes**: Analyzes `git diff HEAD` - Best for comprehensive session documentation
   - **Skip git analysis**: Uses conversation context only - Best when working without git or discussing future plans

3. **Analyze the diff**: Extract and understand:
   - Files modified and their purposes
   - Functions/components added or changed
   - Patterns and architectural decisions visible in code
   - Test coverage changes
   - Configuration changes

This dual-context approach ensures documentation is both accurate (based on actual code) and meaningful (based on discussed reasoning).

## Arguments

- **$0** (required): Context type
  - Values: `feature`, `bug`, `architecture`, `refactor`, `performance`, `security`
- **$1** (optional): Name/identifier for the documentation file
  - If not provided, I'll derive a name from the conversation context
  - Examples: `shopping-cart`, `issue-123`, `auth-redesign`

If no arguments are provided, I'll ask you what type of context to document.

## Examples

### Example 1: Document a Feature with Git Changes
```
/update-context feature data-grid-qis
```
I will:
1. Check git status (finds uncommitted changes)
2. Ask: "Would you like me to analyze: (1) Staged changes only, (2) All changes, or (3) Skip git analysis?"
3. User selects "Staged changes only"
4. Run `git diff --cached` and analyze the actual code changes
5. Combine conversation context + git analysis
6. Create: `.claude/context-docs/features/data-grid-qis.md` with detailed implementation info

### Example 2: Document a Bug Fix
```
/update-context bug connection-modal-crash
```
With git changes, the documentation will include:
- Conversation: Root cause analysis and reasoning
- Git diff: Exact lines changed, files affected, fix implementation
- Result: Complete picture of the bug and its solution

### Example 3: Document Architecture Decision
```
/update-context architecture websocket-communication
```
Creates: `.claude/context-docs/architecture/websocket-communication.md`
Includes discussion of alternatives + actual implementation patterns from code changes

### Example 4: Update Existing Documentation
If the file already exists, I'll read it first and merge/update the content with new information from the conversation and any new git changes.

### Example 5: Document Without Git (Planning Phase)
```
/update-context feature new-auth-system
```
If no git changes exist yet, or user selects "Skip git analysis", documentation will be based purely on conversation context - perfect for planning and design discussions.

## Tips

- **Use descriptive names**: Choose clear, searchable names for your documentation
- **Document as you go**: Invoke this skill during or right after implementation discussions
- **Stage your changes first**: For cleanest documentation, stage (`git add`) the files you want to document before running the skill
- **Use staged-only for commits**: When preparing a commit, use "staged changes only" to document exactly what's being committed
- **Use all changes for sessions**: When documenting a full work session with multiple changes, use "all changes"
- **Review later**: These docs serve as great onboarding material and decision logs
- **Update when needed**: Re-run the skill with the same name to append new findings
- **Commit to git**: Include `.claude/context-docs/` in your repository for team visibility
- **Works with or without git**: The skill adapts - use it for planning (no git) or post-implementation (with git)

## Integration with Development Workflow

This skill complements your existing workflow by combining conversation insights with actual code changes:

**Planning Phase** (no git changes):
- During architecture discussions → Record decisions and alternatives
- Before implementing → Document the planned approach

**Implementation Phase** (with git changes):
- While implementing → Stage changes and document incrementally
- After implementing a feature → Document with actual code analysis
- After fixing a bug → Capture root cause + exact fix from git diff
- After refactoring → Document the changes with before/after code comparison
- After performance improvements → Note the results with actual code changes

**Review Phase**:
- Before committing → Document staged changes for commit context
- After completing work session → Document all changes made

The documentation becomes a living record of your project's evolution, grounded in both reasoning (conversation) and reality (code changes).
