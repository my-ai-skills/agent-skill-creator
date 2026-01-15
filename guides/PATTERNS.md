# Skill Design Patterns

Common patterns for building effective Claude skills.

## Workflow Patterns

### Pattern 1: Script Automation

Offload complex, deterministic logic to Python scripts.

**When to use:**
- Same code being rewritten repeatedly
- Deterministic reliability needed
- Complex calculations or transformations

**Structure:**
```
skill/
├── SKILL.md
└── scripts/
    ├── process.py
    ├── validate.py
    └── convert.py
```

**SKILL.md example:**
```markdown
## Processing Files

To process a file, run:
```bash
python scripts/process.py <input> <output>
```

For validation first:
```bash
python scripts/validate.py <input> && python scripts/process.py <input> <output>
```
```

### Pattern 2: Read-Process-Write

Simple transformation workflows without scripts.

**When to use:**
- Transformations Claude can do inline
- No need for external libraries
- Context-dependent processing

**SKILL.md example:**
```markdown
## Document Transformation

1. Read the source file with the Read tool
2. Transform content according to these rules:
   - Rule 1: [specific transformation]
   - Rule 2: [another transformation]
3. Write the result with the Write tool
```

### Pattern 3: Search-Analyze-Report

Codebase analysis and pattern detection.

**When to use:**
- Code review tasks
- Pattern detection
- Audit and compliance checks

**SKILL.md example:**
```markdown
## Security Audit

1. Search for patterns using Grep:
   - `eval\(` - Potential code injection
   - `password.*=` - Hardcoded credentials
   - `http://` - Unencrypted connections

2. For each match, analyze the context:
   - Is it user input?
   - Is there validation?
   - What's the risk level?

3. Generate report with:
   - File path and line number
   - Code snippet
   - Risk assessment
   - Recommendation
```

### Pattern 4: Command Chains

Multi-step operations with dependencies.

**When to use:**
- Sequential operations
- Each step depends on previous
- Complex workflows

**SKILL.md example:**
```markdown
## Build and Deploy

Execute in order:

1. **Validate**: `python scripts/validate.py`
   - Checks prerequisites
   - Fails fast if issues found

2. **Build**: `python scripts/build.py`
   - Compiles assets
   - Generates output

3. **Deploy**: `python scripts/deploy.py`
   - Only runs if build succeeds
   - Handles rollback on failure
```

### Pattern 5: Iterative Refinement

Progressive deepening across multiple passes.

**When to use:**
- Quality-critical outputs
- Complex creative tasks
- Analysis requiring multiple perspectives

**SKILL.md example:**
```markdown
## Document Review

### Pass 1: Structure
- Check overall organization
- Verify section completeness
- Note structural issues

### Pass 2: Content
- Verify accuracy of claims
- Check for consistency
- Identify gaps

### Pass 3: Style
- Check tone and voice
- Verify formatting
- Polish language

### Final Output
Combine findings into prioritized list:
1. Critical issues
2. Important improvements
3. Nice-to-have suggestions
```

## Progressive Disclosure Patterns

### Pattern A: Hub and Spoke

Central SKILL.md points to specialized references.

**Structure:**
```
skill/
├── SKILL.md (navigation hub)
└── references/
    ├── feature-a.md
    ├── feature-b.md
    └── feature-c.md
```

**SKILL.md example:**
```markdown
## Features

This skill supports three main features:

- **Feature A**: Basic operations. See `references/feature-a.md`
- **Feature B**: Advanced processing. See `references/feature-b.md`
- **Feature C**: Export options. See `references/feature-c.md`
```

### Pattern B: Conditional Loading

Load references based on specific triggers.

**SKILL.md example:**
```markdown
## Document Processing

### Basic Operations
Create, read, and edit documents directly.

### Advanced Operations

**If working with tracked changes:**
See `references/track-changes.md` for detailed guidance.

**If working with complex formatting:**
See `references/formatting.md` for supported styles.

**If generating reports:**
See `references/report-templates.md` for templates.
```

### Pattern C: Domain Organization

Organize by domain/topic for large skills.

**Structure:**
```
analytics-skill/
├── SKILL.md
└── references/
    ├── finance/
    │   ├── revenue.md
    │   └── costs.md
    ├── sales/
    │   ├── pipeline.md
    │   └── forecasting.md
    └── product/
        ├── usage.md
        └── features.md
```

## Resource Patterns

### Script Pattern: CLI Tool

```python
#!/usr/bin/env python3
"""
Tool description.

Usage:
    python tool.py <command> [options]

Commands:
    process     Process input files
    validate    Validate input
    convert     Convert between formats
"""

import argparse
import sys

def cmd_process(args):
    """Process command implementation."""
    pass

def cmd_validate(args):
    """Validate command implementation."""
    pass

def cmd_convert(args):
    """Convert command implementation."""
    pass

def main():
    parser = argparse.ArgumentParser(description="Tool description")
    subparsers = parser.add_subparsers(dest="command", required=True)

    # Process command
    p_process = subparsers.add_parser("process", help="Process input files")
    p_process.add_argument("input", help="Input file")
    p_process.add_argument("-o", "--output", help="Output file")

    # Validate command
    p_validate = subparsers.add_parser("validate", help="Validate input")
    p_validate.add_argument("input", help="Input file")

    # Convert command
    p_convert = subparsers.add_parser("convert", help="Convert formats")
    p_convert.add_argument("input", help="Input file")
    p_convert.add_argument("output", help="Output file")
    p_convert.add_argument("-f", "--format", required=True, help="Target format")

    args = parser.parse_args()

    commands = {
        "process": cmd_process,
        "validate": cmd_validate,
        "convert": cmd_convert,
    }

    try:
        commands[args.command](args)
    except Exception as e:
        print(f"Error: {e}", file=sys.stderr)
        sys.exit(1)

if __name__ == "__main__":
    main()
```

### Reference Pattern: Schema Documentation

```markdown
# Database Schema

## Table of Contents

- [Users](#users)
- [Orders](#orders)
- [Products](#products)

## Users

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| email | VARCHAR(255) | Unique email address |
| created_at | TIMESTAMP | Account creation time |

### Relationships
- Has many Orders
- Has many Products (as seller)

### Indexes
- `idx_users_email` on email (unique)

## Orders

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to Users |
| total | DECIMAL(10,2) | Order total |
| status | ENUM | pending, completed, cancelled |

### Relationships
- Belongs to User
- Has many OrderItems

## Products

[Continue pattern...]
```

### Asset Pattern: Template Directory

```
assets/
├── README.md
├── templates/
│   ├── basic.html
│   ├── report.html
│   └── dashboard.html
└── components/
    ├── header.html
    ├── footer.html
    └── sidebar.html
```

**assets/README.md:**
```markdown
# Template Assets

## Templates

- `basic.html` - Minimal HTML5 template
- `report.html` - Report layout with header/footer
- `dashboard.html` - Dashboard with sidebar navigation

## Components

Reusable components for templates:
- `header.html` - Standard page header
- `footer.html` - Standard page footer
- `sidebar.html` - Navigation sidebar
```

## Anti-Patterns

### Anti-Pattern: Monolithic SKILL.md

**Problem:** Everything in one file, >1000 lines.

**Solution:** Extract to references:
```markdown
## Overview
[Keep brief overview in SKILL.md]

## Details
See `references/detailed-guide.md` for comprehensive documentation.
```

### Anti-Pattern: Deep Nesting

**Problem:**
```
references/
  category/
    subcategory/
      topic/
        detail.md
```

**Solution:** Flatten with clear naming:
```
references/
  category-subcategory-topic.md
```

### Anti-Pattern: Duplicate Information

**Problem:** Same content in SKILL.md and references.

**Solution:** Single source of truth:
- Core concepts in SKILL.md
- Details in references
- Never duplicate
