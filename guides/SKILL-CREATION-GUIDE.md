# Skill Creation Guide

A comprehensive guide for creating effective Claude skills that extend AI capabilities with specialized knowledge, workflows, and tool integrations.

## What Skills Provide

1. **Specialized workflows** - Multi-step procedures for specific domains
2. **Tool integrations** - Instructions for working with specific file formats or APIs
3. **Domain expertise** - Company-specific knowledge, schemas, business logic
4. **Bundled resources** - Scripts, references, and assets for complex tasks

## Core Principles

### 1. Concise is Key

The context window is a shared resource. Claude is already very smart—only add context it doesn't already have.

- Prefer concise examples over verbose explanations
- Remove redundant information
- Trust Claude's existing capabilities

### 2. Set Appropriate Degrees of Freedom

Match specificity to task fragility and variability:

| Freedom Level | When to Use | Example |
|---------------|-------------|---------|
| **High** (text-based) | Multiple valid approaches, context-dependent | General guidance, creative tasks |
| **Medium** (pseudocode) | Preferred pattern exists, some variation OK | Structured workflows |
| **Low** (specific scripts) | Fragile operations, critical consistency | File format manipulation |

### 3. Progressive Disclosure

Three-level loading system for context efficiency:

1. **Metadata** (name + description) - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<5k words)
3. **Bundled resources** - As needed by Claude (unlimited)

## Skill Anatomy

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter metadata (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── Markdown instructions (required)
└── Bundled Resources (optional)
    ├── scripts/     - Executable code (Python/Bash)
    ├── references/  - Documentation to load as needed
    └── assets/      - Files used in output (templates, etc.)
```

## SKILL.md Structure

### Frontmatter (YAML)

```yaml
---
name: skill-name
description: |
  Clear description of what skill does and when to use it.
  Use when Claude needs to: (1) First case, (2) Second case.
---
```

**Validation rules:**
- `name`: Max 64 chars, lowercase letters/numbers/hyphens only
- `description`: Max 1024 chars, non-empty

**Description best practices:**
- Include both WHAT and WHEN
- All "when to use" information goes here
- Be comprehensive about triggers/contexts

### Body (Markdown)

The body contains instructions Claude follows when the skill is active:

- Step-by-step procedures
- Output format specifications
- Error handling guidelines
- References to bundled resources

**Keep under 500 lines** for optimal performance.

### Optional: allowed-tools

Limit which tools Claude can use when the skill is active:

```yaml
---
name: reading-files-safely
description: Read files without making changes.
allowed-tools:
  - Read
  - Grep
  - Glob
---
```

## Bundled Resources

### Scripts (`scripts/`)

**When to include:**
- Same code being rewritten repeatedly
- Deterministic reliability needed
- Complex calculations or transformations

**Example:** `scripts/rotate_pdf.py` for PDF rotation tasks

**Benefits:**
- Token efficient
- Deterministic
- May execute without loading into context

### References (`references/`)

**When to include:**
- Documentation Claude should reference while working
- Information needed only for specific operations

**Examples:**
- `references/schema.md` - Database schemas
- `references/api_docs.md` - API specifications
- `references/policies.md` - Company policies

**Best practices:**
- For files >10k words, include grep patterns in SKILL.md
- Include table of contents for files >100 lines
- Keep references focused on single topics

### Assets (`assets/`)

**When to include:**
- Files used in the final output
- Templates and boilerplate

**Examples:**
- `assets/logo.png` - Brand assets
- `assets/template.html` - HTML templates
- `assets/boilerplate/` - Starter code

## Creation Process

### Step 1: Understand with Concrete Examples

Before writing anything:
- Collect concrete examples of how the skill will be used
- Identify what triggers should activate the skill
- Understand the expected outputs

**Questions to answer:**
- What functionality should this skill support?
- What would a user say that should trigger this skill?
- What does success look like?

### Step 2: Plan Reusable Contents

For each example, consider:
1. How would you execute this from scratch?
2. What would be helpful when doing this repeatedly?

**Identify candidates for:**
- Scripts: Repeatedly writing the same code
- References: Repeatedly discovering same information
- Assets: Using same templates/boilerplate

### Step 3: Initialize the Skill

```bash
python scripts/init_skill.py <skill-name>
python scripts/init_skill.py <skill-name> --topic development
```

This creates the skill directory with:
- SKILL.md template with proper frontmatter
- Example resource directories

### Step 4: Implement the Skill

Remember: You're creating the skill for another instance of Claude to use.

**Order of operations:**
1. Implement scripts, references, and assets first
2. Test all scripts by running them
3. Write SKILL.md instructions
4. Remove any unused template files

**Writing guidelines:**
- Use imperative/infinitive form ("Create", "Extract")
- Reference resources by relative path
- Include examples in SKILL.md

### Step 5: Package and Validate

```bash
python scripts/package_skill.py skills/<topic>/<skill-name>
```

The script validates:
- YAML frontmatter format
- Naming conventions
- Directory structure
- Description completeness

### Step 6: Iterate

Based on real usage:
1. Use the skill on actual tasks
2. Notice struggles or inefficiencies
3. Identify improvements needed
4. Implement changes and test again

## What NOT to Include

Do not create extraneous documentation:
- ~~README.md~~ (SKILL.md is the documentation)
- ~~INSTALLATION_GUIDE.md~~
- ~~QUICK_REFERENCE.md~~
- ~~CHANGELOG.md~~

Only include information needed for Claude to do the job.

## Progressive Disclosure Patterns

### Pattern 1: High-Level Guide with References

```markdown
# PDF Processing

## Quick start
Extract text with pdfplumber: [code example]

## Advanced features
- **Form filling**: See [FORMS.md](references/FORMS.md)
- **API reference**: See [REFERENCE.md](references/REFERENCE.md)
```

### Pattern 2: Domain-Specific Organization

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── references/
    ├── finance.md (revenue, billing)
    ├── sales.md (opportunities, pipeline)
    └── product.md (API usage, features)
```

### Pattern 3: Conditional Details

```markdown
# DOCX Processing

## Creating documents
Use docx-js for new documents. See [DOCX-JS.md](references/DOCX-JS.md).

## Editing documents
For simple edits, modify XML directly.

**For tracked changes**: See [REDLINING.md](references/REDLINING.md)
```

## Key Takeaways

1. Skills are modular packages that transform Claude from general-purpose to specialized
2. Context is precious—be concise and intentional
3. Progressive disclosure keeps context efficient
4. Clear descriptions are critical for triggering
5. Reusable resources (scripts, references, assets) reduce repetition
6. Iterate based on real usage
