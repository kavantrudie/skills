# Update Context Skill

A universal AI coding skill for maintaining persistent context documentation across all your projects and agents. Works with Claude Code, Cursor, Aider, GitHub Copilot, and any AI coding agent. Combines conversation insights with actual git changes to create comprehensive, accurate documentation.

## Installation

### Option 1: Using Skills CLI (Recommended)

```bash
# Install using the Vercel skills CLI
npx skills add https://github.com/kavantrudie/skills
```

This will install the skill globally for all AI coding agents that support the skills format.

### Option 2: Manual Installation

#### For Claude Code
```bash
# Global installation
git clone https://github.com/kavantrudie/skills.git ~/.claude/skills/update-context

# Project-specific installation
git clone https://github.com/kavantrudie/skills.git ./.claude/skills/update-context
```

#### For Cursor
```bash
# Global installation
git clone https://github.com/kavantrudie/skills.git ~/.cursor/skills/update-context

# Project-specific installation
git clone https://github.com/kavantrudie/skills.git ./.cursor/skills/update-context
```

#### For Aider
```bash
# Global installation
git clone https://github.com/kavantrudie/skills.git ~/.aider/skills/update-context

# Project-specific installation
git clone https://github.com/kavantrudie/skills.git ./.aider/skills/update-context
```

#### For Other Agents
```bash
# Use a generic location
git clone https://github.com/kavantrudie/skills.git ~/.ai-skills/update-context
```

After installation, restart your AI coding agent to load the skill.

## Quick Start

### Option 1: Manual Invocation

```bash
# Document a feature you're working on
/update-context feature my-feature-name

# Document a bug analysis
/update-context bug issue-123

# Document an architectural decision
/update-context architecture auth-redesign
```

### Option 2: Automatic Invocation

Just mention it in conversation:
```
"Let's document this feature implementation"
"Can you capture this architectural decision?"
"Document this bug fix for future reference"
```

Your AI agent will automatically invoke the skill and create organized documentation.

## What It Does

This skill combines two powerful sources of information:

1. **Conversation Context**: Captures the "why" - your reasoning, decisions, and discussions
2. **Git Changes Analysis**: Captures the "what" - actual code changes, files modified, and implementation details

Together, they create comprehensive documentation that's both meaningful and accurate.

The skill **automatically detects your AI agent** and creates documentation in the appropriate directory:

**Agent Detection:**
- Claude Code → `.claude/context-docs/`
- Cursor → `.cursor/context-docs/`
- Aider → `.aider/context-docs/`
- GitHub Copilot → `.github/copilot/context-docs/`
- Generic/Unknown → `.ai-context/`
- Custom → Use `--output` flag or set `AI_CONTEXT_DIR` environment variable

**Directory Structure (example for Cursor):**
```
your-project/
└── .cursor/
    └── context-docs/
        ├── features/
        │   └── my-feature.md
        ├── bugs/
        │   └── issue-123.md
        ├── architecture/
        │   └── auth-redesign.md
        ├── refactors/
        ├── performance/
        └── security/
```

## Git Changes Analysis

When you run `/update-context`, the skill automatically:

1. **Detects git changes** in your working directory
2. **Asks your preference**:
   - **Staged changes only** (`git diff --cached`) - Perfect for documenting what's about to be committed
   - **All changes** (`git diff HEAD`) - Great for comprehensive session documentation
   - **Skip git analysis** - Use conversation context only (useful for planning)
3. **Analyzes the diff** to extract:
   - Files modified and their purposes
   - Key code changes and patterns
   - Dependencies added/removed
   - Test coverage changes
   - Configuration updates

### Why This Matters

**Without git analysis:** "We added authentication"
**With git analysis:** "We added authentication - modified 5 files (auth.ts, middleware.ts, routes.ts, types.ts, auth.test.ts), implemented JWT strategy using jsonwebtoken package, added 3 middleware functions, created 15 unit tests covering token generation, validation, and expiry"

The git diff provides the concrete details that make documentation actually useful.

## Documentation Types

| Type | Use For | Example |
|------|---------|---------|
| `feature` | New features and implementations | `/update-context feature shopping-cart` |
| `bug` | Bug analysis and fixes | `/update-context bug checkout-crash` |
| `architecture` | Design decisions and patterns | `/update-context architecture microservices` |
| `refactor` | Code refactoring efforts | `/update-context refactor state-management` |
| `performance` | Performance optimizations | `/update-context performance query-optimization` |
| `security` | Security implementations | `/update-context security rate-limiting` |

## What Gets Captured

Each documentation file includes insights from **conversation + git analysis**:

**From Conversation:**
- **Overview**: Clear summary of what this is about
- **Problem/Goal**: What you're solving and why
- **Key Decisions**: Why you chose this approach over alternatives
- **Tradeoffs**: Benefits and costs

**From Git Analysis:**
- **Code Changes**: Actual files modified with before/after comparison
- **Implementation Details**: Functions added, classes created, patterns used
- **Dependencies**: New packages added or removed
- **Test Coverage**: Test files created or modified
- **Related Files**: Which files are involved with change summaries

**Combined:**
- **Solution**: Technical approach (conversation) + actual implementation (git)
- **Testing**: Verification approach (conversation) + test file changes (git)
- **Status**: Current state with evidence from git changes
- **Future Improvements**: Known limitations and next steps

## Examples

### Example 1: Feature Documentation with Git Analysis
```bash
/update-context feature data-grid-improvements
```

**What happens:**
1. Agent detects you're using Cursor (finds `.cursor/` directory)
2. Detects you have uncommitted changes
3. Prompts: "Would you like me to analyze: (1) Staged changes only, (2) All changes, or (3) Skip git analysis?"
4. You select "Staged changes only"
5. Agent runs `git diff --cached` and analyzes:
   - Modified: `DataGrid.tsx` (+87 / -34)
   - Modified: `hooks/useDataGrid.ts` (+45 / -12)
   - Added: `DataGrid.test.tsx` (+120 / -0)
   - Modified: `package.json` (+1 / -0)
6. Creates: `.cursor/context-docs/features/data-grid-improvements.md` with:
   - Your discussed reasoning (from conversation)
   - Exact code changes (from git diff)
   - New dependencies (react-window for virtualization)
   - Test coverage (120 new test lines)

### Example 2: Bug Documentation
```bash
/update-context bug connection-modal-crash
```

**Result includes:**
- **From conversation**: Root cause analysis, debugging steps taken
- **From git diff**: Exact line that caused the crash, the fix applied, test cases added
- **Complete picture**: Why it happened + how it was fixed

### Example 3: During Conversation
```
You: "I just implemented WebSocket communication for real-time updates"
Agent: "Great! Let me document this architectural decision..."
Agent: "I see you have changes. Would you like me to analyze staged changes, all changes, or skip git analysis?"
You: "All changes"
Agent: [Analyzes git diff showing WebSocket setup, connection handling, reconnection logic]
[Creates documentation in your agent's context directory with both discussion and code details]
```

### Example 5: Custom Output Directory
```bash
# Use a custom directory for your team's documentation
/update-context feature new-api --output ./team-docs/ai-context

# Or set environment variable for all invocations
export AI_CONTEXT_DIR=".shared-context"
/update-context feature new-api
```

### Example 4: Planning Without Git (No Changes Yet)
```bash
/update-context architecture new-auth-system
```

If no git changes exist (planning phase), documentation is based purely on conversation - perfect for design decisions before implementation.

## Tips

1. **Stage changes before documenting**: Run `git add` on files you want to document, then use "staged changes only" for cleanest results
2. **Document as you go**: Invoke this skill right after implementing, while changes are fresh
3. **Use descriptive names**: Choose clear, searchable names for your docs
4. **Choose the right git analysis mode**:
   - **Staged changes only**: When preparing a commit, for focused documentation
   - **All changes**: When documenting a full work session
   - **Skip git analysis**: When planning or discussing future work
5. **Update when needed**: Re-run with the same name to update existing docs with new changes
6. **Commit to git**: Include your context directory in your repository (e.g., `.cursor/context-docs/` or `.ai-context/`)
7. **Share with team**: These docs become great onboarding material with actual code evidence
8. **Works with any agent**: Use with Claude Code, Cursor, Aider, GitHub Copilot, or any AI coding agent
9. **Custom directories**: Use `--output` flag or `AI_CONTEXT_DIR` environment variable for custom locations
10. **Multi-agent projects**: Each agent maintains its own context directory, or share one with `AI_CONTEXT_DIR`

## File Structure

```
skills/
├── README.md             # Repository overview
├── LICENSE               # MIT License
└── skills/
    └── update-context/
        ├── README.md             # This file (for you!)
        ├── SKILL.md              # Main skill instructions (Claude reads this)
        ├── template.md           # Full documentation template
        └── examples/
            └── sample-context.md # Example of what documentation looks like
```

## Benefits

- **Dual Context**: Captures both "why" (conversation) and "what" (git changes) for complete picture
- **Accurate Documentation**: Git analysis ensures docs reflect actual implementation, not just intentions
- **Knowledge Preservation**: Never lose track of why decisions were made and what was actually changed
- **Team Onboarding**: New developers see both reasoning and concrete code changes
- **Searchable Knowledge Base**: Future AI sessions can read these docs when explicitly referenced
- **Living Documentation**: Update docs as implementation evolves, with git evidence of each iteration
- **Cross-Project**: One skill works across all your projects
- **No Manual Effort**: Automatically extracts code details from git - you just provide the "why"

**Important Note**: Context docs are not automatically loaded by AI agents at session start. They must be explicitly read (e.g., "read the auth context doc" or "grep context-docs for performance"). Think of them as **searchable ADRs (Architectural Decision Records)** rather than automatic context injection.

## Customization

Want to customize the template? Edit the template file in your agent's skills directory:

**Claude Code:**
```
~/.claude/skills/update-context/skills/update-context/template.md
```

**Cursor:**
```
~/.cursor/skills/update-context/skills/update-context/template.md
```

**Aider:**
```
~/.aider/skills/update-context/skills/update-context/template.md
```

**Generic:**
```
~/.ai-skills/update-context/skills/update-context/template.md
```

Or if you cloned the repository directly, edit `skills/update-context/template.md` in your clone.

Changes will apply to all future documentation generated by this skill.

## Workflow Integration

Perfect for these workflows:

1. **Feature Development**:
   - Start feature → Document plan with `/update-context`
   - During implementation → Update with key decisions
   - After completion → Final update with outcomes

2. **Bug Fixing**:
   - Investigate bug → Document findings
   - Implement fix → Document root cause and solution
   - Test → Document verification steps

3. **Architecture Reviews**:
   - During design discussion → Capture alternatives considered
   - After decision → Document rationale and tradeoffs
   - Post-implementation → Update with lessons learned

## Multi-Agent Compatibility

This skill works natively with multiple AI coding agents:

**Fully Supported (Auto-Detection):**
- **Claude Code** - Auto-detects `.claude/` directory
- **Cursor** - Auto-detects `.cursor/` directory
- **Aider** - Auto-detects `.aider/` directory
- **GitHub Copilot** - Auto-detects `.github/copilot/` directory

**Generic Support:**
- Any AI agent - Falls back to `.ai-context/` directory
- Custom configuration - Use `--output` flag or `AI_CONTEXT_DIR` variable

**How AI Agents Use These Docs:**
The generated markdown files serve as a searchable knowledge base:
- **Explicit reference**: AI can read them when you say "read the context doc about X"
- **Search/grep**: Find relevant past decisions and implementations
- **Not automatic**: AI agents don't auto-load these at session start
- **Team documentation**: Primary value is for human developers to understand reasoning + implementation
- **Version controlled**: Track evolution of decisions over time in git

## Available on skills.sh

This skill is compatible with [skills.sh](https://skills.sh) and can be installed using:

```bash
npx skills add https://github.com/kavantrudie/skills
```

The skill is automatically discoverable on skills.sh once published to GitHub, as it follows the standard skills directory structure with:
- ✅ Valid `SKILL.md` file with proper frontmatter in `skills/update-context/`
- ✅ Clear documentation in `README.md`
- ✅ MIT License
- ✅ Example usage in `skills/update-context/examples/`

Users can also install directly from this GitHub repository using the manual installation instructions above.

## Troubleshooting

### Skill not appearing?
1. Verify file exists in your agent's skills directory:
   - Claude Code: `ls ~/.claude/skills/update-context/SKILL.md`
   - Cursor: `ls ~/.cursor/skills/update-context/SKILL.md`
   - Aider: `ls ~/.aider/skills/update-context/SKILL.md`
2. Restart your AI coding agent
3. Try manual invocation: `/update-context feature test`

### Context directory not created?
The skill creates the context directory in your **project root** (not global).
Navigate to your project directory first. Check which directory was used with:
- Claude Code: `.claude/context-docs/`
- Cursor: `.cursor/context-docs/`
- Aider: `.aider/context-docs/`
- GitHub Copilot: `.github/copilot/context-docs/`
- Generic: `.ai-context/`

### Want a custom directory?
Use the `--output` flag or set the `AI_CONTEXT_DIR` environment variable:
```bash
# Using flag
/update-context feature test --output ./my-docs

# Using environment variable
export AI_CONTEXT_DIR="./team-context"
/update-context feature test
```

### Want project-specific skill installation?
Copy this skill to your project (adjust path for your agent):
```bash
# For Claude Code
cp -r ~/.claude/skills/update-context ./.claude/skills/

# For Cursor
cp -r ~/.cursor/skills/update-context ./.cursor/skills/

# For Aider
cp -r ~/.aider/skills/update-context ./.aider/skills/
```

## Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest improvements to the template
- Add new documentation types
- Improve the README

Submit issues or pull requests on GitHub.

## Support

- **GitHub Repository**: https://github.com/kavantrudie/skills
- **Claude Code Issues**: https://github.com/anthropics/claude-code/issues
- **Skill Documentation**: See `skills/update-context/SKILL.md`
- **Template Reference**: See `skills/update-context/template.md`
- **Example**: See `skills/update-context/examples/sample-context.md`

## License

MIT License - See [LICENSE](LICENSE) file for details.

---

Happy documenting!

_Last updated: 2026-01-30_
