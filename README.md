# AI Skills Creator

A framework for designing, creating, maintaining, and publishing AI skills for Claude and other AI assistants. Each skill is published as its own GitHub repository.

> _This repository is meant to be run in a Claude Code environment_

## Quick Start

```bash
# Create a new skill (creates standalone repo)
python scripts/init_skill.py my-skill-name

# Create and publish to GitHub in one step
python scripts/init_skill.py my-skill-name --github --org my-org

# Publish an existing skill to GitHub
python scripts/publish_skill.py ./my-skill
```

## Repository Structure

```
agent-skill-creator/
├── CLAUDE.md                 # Claude Code project configuration
├── README.md                 # This file
├── .claude/
│   ├── settings.json         # Claude permissions
│   └── skills -> ../skills   # Symlink to skills directory
│
├── skills/                   # Local skills for development/testing
│   └── [skill-name]/         # Each skill is a standalone repo
│       ├── SKILL.md          # Main skill definition (required)
│       ├── README.md         # Repository documentation
│       ├── CLAUDE.md         # Claude Code config for the skill
│       ├── scripts/          # Executable automation
│       ├── references/       # Documentation loaded as needed
│       └── assets/           # Templates and output files
│
├── references/               # Research archive for creating skills
│   ├── documentation/        # Official docs and specs
│   ├── patterns/             # Proven skill patterns
│   └── sources/              # Raw research materials
│
├── guides/                   # Skill creation guides
│   ├── SKILL-CREATION-GUIDE.md
│   ├── BEST-PRACTICES.md
│   └── PATTERNS.md
│
└── scripts/                  # Utility scripts
    ├── init_skill.py         # Create new skill repos
    ├── publish_skill.py      # Publish skills to GitHub
    └── package_skill.py      # Package skills for distribution
```

## Workflow

### 1. Research

Before creating a new skill, research existing patterns:

- [awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) - Community skills
- [Anthropic Skills](https://github.com/anthropics/skills) - Official skills
- Save findings to `references/sources/`

### 2. Create

```bash
# Create a new skill locally
python scripts/init_skill.py my-skill --path ./skills

# Or create directly to a different location
python scripts/init_skill.py my-skill --path ~/projects
```

### 3. Develop

Each skill follows this structure:

```
my-skill/
├── SKILL.md          # Required - skill definition with YAML frontmatter
├── README.md         # Repository documentation
├── CLAUDE.md         # Claude Code configuration
├── scripts/          # Python/Bash automation
├── references/       # Documentation for Claude to read
└── assets/           # Templates and output files
```

### 4. Test

Test the skill with Claude Code:

```bash
cd my-skill
claude  # Start Claude Code in the skill directory
```

### 5. Publish

```bash
# Publish to your GitHub
python scripts/publish_skill.py ./skills/my-skill

# Publish to an organization
python scripts/publish_skill.py ./skills/my-skill --org my-org

# Create private repository
python scripts/publish_skill.py ./skills/my-skill --private
```

## Skill Structure

### SKILL.md Format

```yaml
---
name: my-skill
description: |
  What this skill does and when to use it.
  Use when Claude needs to: (1) First case, (2) Second case.
---

# My Skill

## Overview
Brief description.

## Instructions
Step-by-step guidance for Claude.

## Examples
Concrete usage examples.

## Guidelines
Rules and constraints.
```

### Key Principles

- **Progressive Disclosure**: Metadata always loaded, body on trigger, resources on demand
- **Concise Instructions**: Keep SKILL.md under 500 lines
- **Clear Triggers**: Description should specify WHAT and WHEN to use
- **Modular Resources**: Split large content into reference files

## Scripts Reference

### init_skill.py

Create a new skill repository:

```bash
python scripts/init_skill.py <name> [options]

Options:
  --path, -p       Output directory (default: current directory)
  --description    Short description for the skill
  --github, -g     Create GitHub repository immediately
  --org, -o        GitHub organization
  --private        Create private repository
```

### publish_skill.py

Publish an existing skill to GitHub:

```bash
python scripts/publish_skill.py <skill-path> [options]

Options:
  --org, -o        GitHub organization
  --private        Create private repository
```

### package_skill.py

Validate and package a skill:

```bash
python scripts/package_skill.py <skill-path> [options]

Options:
  --output, -o     Output directory for .skill package
  --validate-only  Only validate, don't package
```

## Guides

- [Skill Creation Guide](guides/SKILL-CREATION-GUIDE.md) - Complete creation process
- [Best Practices](guides/BEST-PRACTICES.md) - Guidelines and anti-patterns
- [Design Patterns](guides/PATTERNS.md) - Common skill architectures

## Resources

### Official Documentation
- [Claude Agent Skills Docs](https://code.claude.com/docs/en/skills)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)
- [Skill Authoring Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

### Community
- [awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills)
- [Claude Skills Deep Dive](https://leehanchung.github.io/blogs/2025/10/26/claude-skills-deep-dive/)

### Related
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Claude Help Center - Custom Skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)
