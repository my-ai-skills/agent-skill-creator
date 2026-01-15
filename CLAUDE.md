# AI Skills Creator - Claude Configuration

This is a framework for creating, maintaining, and publishing Claude AI skills. Each skill is published as its own GitHub repository.

## Project Overview

- **Purpose**: Design, create, test, and publish reusable AI skills
- **Model**: One skill per GitHub repository
- **Workflow**: Research → Create → Develop → Test → Publish

## Common Commands

```bash
# Create a new skill repository
python scripts/init_skill.py <skill-name>
python scripts/init_skill.py <skill-name> --path ./skills
python scripts/init_skill.py <skill-name> --github --org my-org

# Publish a skill to GitHub
python scripts/publish_skill.py ./skills/<skill-name>
python scripts/publish_skill.py ./skills/<skill-name> --org my-org

# Validate a skill
python scripts/package_skill.py ./skills/<skill-name> --validate-only
```

## Skill Repository Structure

Each skill is a standalone repository with this structure:

```
skill-name/
├── SKILL.md          # Required - main skill definition
├── README.md         # Repository documentation
├── CLAUDE.md         # Claude Code configuration
├── .gitignore        # Git ignore rules
├── scripts/          # Python/Bash automation
├── references/       # Documentation loaded as needed
└── assets/           # Templates and output files
```

## SKILL.md Format

```yaml
---
name: skill-name
description: |
  What this skill does.
  Use when Claude needs to: (1) First case, (2) Second case.
---

# Skill Name

[Instructions for Claude when this skill is active]
```

## Skill Development Workflow

### 1. Research Phase

Before creating a new skill:
- Search [awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills)
- Check [Anthropic's official skills](https://github.com/anthropics/skills)
- Document findings in `references/sources/`

### 2. Create Phase

```bash
# Create locally for development
python scripts/init_skill.py my-skill --path ./skills

# Create and publish immediately
python scripts/init_skill.py my-skill --github --org my-org
```

### 3. Development Phase

Edit the skill files:
- `SKILL.md` - Main instructions for Claude
- `scripts/` - Add automation scripts
- `references/` - Add documentation
- `assets/` - Add templates

### 4. Testing Phase

```bash
# Navigate to skill directory
cd skills/my-skill

# Start Claude Code to test
claude
```

### 5. Publish Phase

```bash
python scripts/publish_skill.py ./skills/my-skill --org my-org
```

## Best Practices

### SKILL.md Guidelines

- Keep body under 500 lines
- Use imperative form ("Create", not "Creates")
- Include both WHAT and WHEN in description
- Reference resources by relative path

### Progressive Disclosure

Three-level context loading:
1. **Metadata** (~100 tokens) - name + description, always loaded
2. **SKILL.md body** (<5k tokens) - loaded when skill triggers
3. **Resources** (unlimited) - loaded only when needed

### Resource Organization

- `scripts/` - Deterministic operations (file conversion, calculations)
- `references/` - Documentation Claude reads (schemas, APIs)
- `assets/` - Files used in output (templates, logos)

## Directory Structure

```
agent-skill-creator/
├── skills/              # Local skills for development
├── references/          # Research archive
│   ├── documentation/   # Official docs
│   ├── patterns/        # Proven patterns
│   └── sources/         # Raw research
├── guides/              # Creation guides
└── scripts/             # Utility scripts
```

## References

- [Skill Creation Guide](guides/SKILL-CREATION-GUIDE.md)
- [Best Practices](guides/BEST-PRACTICES.md)
- [Design Patterns](guides/PATTERNS.md)
