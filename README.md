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

A skill for maintaining persistent context documentation across all your projects. Combines conversation insights with git diff analysis to create comprehensive, accurate documentation of features, bugs, and architectural decisions.

**Key Features:**
- Analyzes both conversation context and actual git changes
- Supports staged changes, all changes, or conversation-only modes
- Automatically extracts code details, dependencies, and test coverage
- Creates organized markdown docs in `.claude/context-docs/`

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

## Compatibility

These skills are compatible with:
- Claude Code CLI
- Vercel AI agents (via skills.sh)
- Any AI coding agent that supports the skills.sh format

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
