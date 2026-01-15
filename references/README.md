# References Archive

This directory stores raw reference materials used when researching and creating skills. Materials here inform skill development but are not bundled with skills.

## Directory Structure

```
references/
├── README.md           # This file
├── documentation/      # Official docs, API references, specs
├── patterns/           # Skill patterns and architectures
└── sources/            # Raw source materials and notes
```

## Workflow

### 1. Research Phase

When starting a new skill, gather relevant materials:

```bash
# Create a folder for the skill research
mkdir -p references/sources/<skill-name>

# Save research notes
# - Web pages (saved as markdown)
# - API documentation
# - Existing implementations
# - User requirements
```

### 2. Documentation

Store official documentation that informs skill development:

```
references/documentation/
├── claude-skills-spec.md      # Official Claude skills specification
├── file-formats/              # Format specifications (PDF, DOCX, etc.)
└── apis/                      # API documentation
```

### 3. Patterns

Document proven patterns discovered during research:

```
references/patterns/
├── workflow-patterns.md       # Common workflow structures
├── progressive-disclosure.md  # Context loading strategies
└── error-handling.md          # Error handling approaches
```

## Archiving Web Content

When saving web content for reference:

1. **Save as Markdown** - Convert HTML to markdown for readability
2. **Include source URL** - Add source URL at top of file
3. **Add retrieval date** - Note when content was fetched
4. **Extract key sections** - Don't save entire pages if only parts are relevant

### Template for Web Content

```markdown
# [Page Title]

> Source: [URL]
> Retrieved: [Date]

## Summary

[Brief summary of key points]

## Relevant Content

[Extracted relevant sections]

## Notes

[Your analysis or notes]
```

## File Naming Conventions

- Use lowercase with hyphens: `skill-creation-guide.md`
- Include date for time-sensitive content: `claude-skills-2025-01.md`
- Group related files in subdirectories

## What Goes Where

### documentation/

- Official specifications
- API references
- Format documentation
- Standard protocols

### patterns/

- Proven skill architectures
- Workflow templates
- Best practice collections
- Example implementations

### sources/

- Raw research notes
- User requirements
- Competitor analysis
- Feature requests

## Relationship to Skills

```
references/ (research phase)
    ↓
    Synthesis and analysis
    ↓
guides/ (derived documentation)
    ↓
templates/ (reusable starting points)
    ↓
skills/ (final skills)
```

References are the **input** to skill creation. They should:

1. Inform skill design decisions
2. Provide source material for instructions
3. Document rationale for approaches
4. Preserve knowledge for future reference

## Maintenance

- **Clean periodically** - Remove outdated or unused references
- **Update docs** - Keep documentation current with upstream changes
- **Cross-reference** - Link related materials
- **Archive old versions** - Move superseded content to `archive/` subdirectory
