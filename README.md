# Claude Code Skills Collection

A collection of reusable skills for Claude Code and other AI coding agents that support the skills.sh format.

## What are Skills?

Skills are modular extensions that enhance AI coding agents with specialized capabilities. Each skill is self-contained with its own documentation, templates, and examples.

## Installation

### Using Skills CLI (Recommended)

```bash
# Install all skills from this repository
npx skills add https://github.com/kavantrudie/skills
```

### Manual Installation (Claude Code)

```bash
# Clone into your global Claude skills directory
git clone https://github.com/kavantrudie/skills.git ~/.claude/skills/custom

# Or for project-specific installation
git clone https://github.com/kavantrudie/skills.git ./.claude/skills/custom
```

## Available Skills

See the [skills directory](./skills/) for all available skills in this collection.

### Update Context

A universal skill for maintaining persistent context documentation across all your projects and AI coding agents. Combines conversation insights with git diff analysis to create comprehensive, accurate documentation of features, bugs, and architectural decisions.

**Key Features:**
- **Multi-agent support**: Works with Claude Code, Cursor, Aider, GitHub Copilot, and any AI coding agent
- **Auto-detection**: Automatically detects your agent and uses the appropriate directory
- **Flexible output**: Custom directories via `--output` flag or `AI_CONTEXT_DIR` environment variable
- **Git analysis**: Analyzes staged changes, all changes, or conversation-only modes
- **Comprehensive**: Automatically extracts code details, dependencies, and test coverage
- **Organized**: Creates structured markdown docs with consistent templates

**Supported Agents:**
- Claude Code → `.claude/context-docs/`
- Cursor → `.cursor/context-docs/`
- Aider → `.aider/context-docs/`
- GitHub Copilot → `.github/copilot/context-docs/`
- Generic/Unknown → `.ai-context/`

[Learn more about update-context →](./skills/update-context/)

## Repository Structure

```
skills/
├── README.md                    # Overview of all skills
└── update-context/
    ├── README.md                # Skill-specific documentation
    ├── SKILL.md                 # Skill definition (Claude reads this)
    ├── template.md              # Documentation template
    └── examples/
        └── sample-context.md    # Example output
```

## For Developers

### Adding New Skills

1. Create a new directory under `skills/` with your skill name
2. Add a `SKILL.md` file with YAML frontmatter (see existing skills)
3. Include a `README.md` with usage instructions
4. Add examples and templates as needed

### Skill Format

Each skill requires a `SKILL.md` file with this structure:

```markdown
---
name: skill-name
description: Brief description of what the skill does
argument-hint: [expected-arguments]
disable-model-invocation: false
allowed-tools: Write, Read, Glob, Bash, Edit
---

# Detailed instructions for Claude Code...
```

## How Context Documentation Works

**Important**: These context documents are **searchable knowledge bases**, not automatically-loaded context.

- AI agents don't automatically read all context docs at session start
- Docs must be **explicitly referenced** (e.g., "read .claude/context-docs/features/auth.md")
- Primary benefit is **persistent, version-controlled documentation** for humans and AI to search/reference
- Creates a **team knowledge base** that survives beyond individual AI sessions
- Acts as **architectural decision records (ADRs)** with actual code evidence

**Best used for**: Searchable history, team onboarding, and explicit context retrieval when needed.

## Compatibility

These skills are compatible with:
- **Claude Code** - Full native support with auto-detection
- **Cursor** - Auto-detects and creates docs in `.cursor/context-docs/`
- **Aider** - Auto-detects and creates docs in `.aider/context-docs/`
- **GitHub Copilot** - Auto-detects and creates docs in `.github/copilot/context-docs/`
- **Vercel AI agents** - Via skills.sh format
- **Any AI coding agent** - Falls back to `.ai-context/` directory or use custom `--output` flag

All skills automatically adapt to your agent and workflow!

## Contributing

Contributions are welcome! Feel free to:
- Add new skills to this collection
- Improve existing skills
- Report bugs or suggest improvements

Submit issues or pull requests on GitHub.

## License

MIT License - See [LICENSE](LICENSE) file for details.

---

Visit [skills.sh](https://skills.sh) to discover more skills for AI coding agents.
