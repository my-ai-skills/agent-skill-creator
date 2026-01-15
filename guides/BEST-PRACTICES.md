# Skill Authoring Best Practices

Guidelines for creating effective, maintainable Claude skills.

## SKILL.md Guidelines

### Structure

1. **Keep body under 500 lines** - Longer files bloat context and reduce effectiveness
2. **Use imperative form** - "Create", "Extract", not "Creates", "Extracts"
3. **Front-load critical information** - Most important instructions first
4. **Use clear headings** - Help Claude navigate the document

### Description Field

The description is the primary triggering mechanism. Make it count:

```yaml
# Good
description: |
  Comprehensive document creation, editing, and analysis with support
  for tracked changes, comments, and formatting preservation.
  Use when Claude needs to work with professional documents (.docx files) for:
  (1) Creating new documents, (2) Modifying content, (3) Working with
  tracked changes, (4) Adding comments, or any document tasks.

# Bad
description: Works with Word documents.
```

**Include:**
- What the skill does
- Specific triggers/contexts
- File types or domains involved

### Instructions

- Be specific about expected outputs
- Include examples of correct behavior
- Document error handling
- Reference resources by relative path

## Context Efficiency

### Progressive Disclosure

| Level | Content | When Loaded |
|-------|---------|-------------|
| Metadata | name + description | Always (~100 words) |
| Body | SKILL.md content | When triggered (<5k words) |
| Resources | scripts, references, assets | On demand (unlimited) |

### Avoid Context Bloat

- Don't duplicate information across SKILL.md and references
- Don't include information Claude already knows
- Don't add "just in case" content
- Keep references focused and single-purpose

### Resource Loading Tips

For large reference files (>10k words):
```markdown
## Schema Reference

See `references/schema.md` for the complete database schema.

Key tables to search for:
- `users` - User accounts
- `orders` - Purchase orders
- `products` - Product catalog
```

## Bundled Resources

### Scripts Best Practices

```python
#!/usr/bin/env python3
"""
Brief description of what this script does.

Usage:
    python script.py <input_file> <output_file>
"""

import argparse
import sys

def main():
    parser = argparse.ArgumentParser(description="Script description")
    parser.add_argument("input", help="Input file path")
    parser.add_argument("output", help="Output file path")
    parser.add_argument("--verbose", "-v", action="store_true", help="Verbose output")
    args = parser.parse_args()

    try:
        # Implementation
        pass
    except Exception as e:
        print(f"Error: {e}", file=sys.stderr)
        sys.exit(1)

if __name__ == "__main__":
    main()
```

**Guidelines:**
- Include docstring with usage
- Use argparse for CLI arguments
- Handle errors gracefully
- Exit with appropriate codes
- Test before packaging

### References Best Practices

- **One topic per file** - Don't mix schemas with policies
- **Include table of contents** for files >100 lines
- **Use clear headings** for searchability
- **Format for scanning** - Lists, tables, code blocks

### Assets Best Practices

- **Use relative paths** in SKILL.md
- **Include README** in complex asset directories
- **Keep assets minimal** - Only what's needed for output

## Common Patterns

### Read-Process-Write

```markdown
## Workflow

1. Read the input file using the Read tool
2. Process the content according to [specific rules]
3. Write the output using the Write tool

## Output Format

[Specify exact format expected]
```

### Search-Analyze-Report

```markdown
## Workflow

1. Search for relevant files using Glob
2. Analyze content using Grep patterns
3. Read specific sections for detail
4. Generate report with findings
```

### Command Chains

```markdown
## Workflow

1. Execute `scripts/validate.py` to check prerequisites
2. Run `scripts/process.py` for transformation
3. Run `scripts/finalize.py` for cleanup
```

## Testing Checklist

Before packaging:

- [ ] All scripts run without errors
- [ ] SKILL.md is under 500 lines
- [ ] Description includes what AND when
- [ ] Resources load only when needed
- [ ] No duplicate information
- [ ] Examples are accurate
- [ ] Error handling is documented

## Anti-Patterns to Avoid

### Over-Documentation

```markdown
# Bad
## Introduction
This skill helps you work with PDFs...

## Background
PDFs were invented in 1993...

## History of Changes
v1.0 - Initial release...

# Good
## PDF Processing
Extract text: `python scripts/extract.py input.pdf`
```

### Vague Descriptions

```yaml
# Bad
description: Helps with documents

# Good
description: |
  Extract text and tables from PDF files. Use when Claude needs to
  read, analyze, or extract content from PDF documents.
```

### Deeply Nested Resources

```
# Bad
references/
  domain/
    subdomain/
      topic/
        file.md

# Good
references/
  domain-topic.md
```

### Redundant Content

```markdown
# Bad - duplicates reference content
## API Reference

### GET /users
Returns list of users...

See references/api.md for more details.
[api.md contains the same content]

# Good
## API Reference

See `references/api.md` for the complete API documentation.

Common operations:
- List users: `GET /users`
- Get user: `GET /users/{id}`
```

## Maintenance

### Version Control

- Use git tags for skill versions
- Document breaking changes
- Keep commit messages descriptive

### Iteration

- Monitor skill performance on real tasks
- Collect feedback on failures
- Update instructions based on observed struggles
- Add examples for edge cases

### Deprecation

When retiring a skill:
1. Add deprecation notice to description
2. Point to replacement skill if available
3. Remove from main README index
4. Archive rather than delete
