---
description: Bootstrap documentation structure and conventions
---

# Setup Documentation Structure

You are setting up a research-driven documentation system for this project.

## Prerequisites Check

First, check if documentation conventions are in place:

```
Read: .claude/rules/documentation/README.md
```

**If the file exists:** Proceed with setup.

**If the file does NOT exist:**
- Tell the user: "Documentation conventions not found. Please run `/setup-rules` first to install the rule templates."
- Stop here and do not proceed

The documentation conventions in `.claude/rules/documentation/README.md` are the single source of truth for:
- Documentation structure
- File naming conventions
- Templates
- Style guidelines
- INDEX.md maintenance

## Your Task

Create the documentation directory structure:

### 1. Create Documentation Directories

```
docs/
├── README.md
├── INDEX.md
├── domains/
├── layers/
│   ├── backend/
│   └── frontend/
└── _research/
    ├── lacking/
    │   ├── pending/
    │   ├── in-progress/
    │   └── processed/
    └── summaries/
```

Use Bash to create all directories:
```bash
mkdir -p docs/domains docs/layers/backend docs/layers/frontend docs/_research/lacking/{pending,in-progress,processed} docs/_research/summaries
```

### 2. Create docs/README.md

```markdown
# Documentation

This directory contains the project's documentation, organized using a research-driven approach.

## Structure

- **domains/** - Business domains and features (authentication, ecommerce, etc.)
- **layers/** - Technical implementation layers (backend, frontend, database)
- **INDEX.md** - Master index for quick lookup
- **_research/** - Research artifacts and gap reports (internal use)

## How to Use This Documentation

### Finding Documentation

**By Business Domain:**
- Check `domains/` for user-facing features and business logic
- Example: `domains/authentication/` for login, registration, JWT

**By Technical Layer:**
- Check `layers/backend/` for services, models, controllers
- Check `layers/frontend/` for components, composables, views
- Example: `layers/backend/services/auth-service.md`

**By Search:**
- Start with `INDEX.md` for quick lookup
- Use grep/search across `docs/` directory

### For Developers

When you need context:
1. Check `INDEX.md` first
2. Navigate to relevant domain or layer
3. If missing, the research agent will document the gap

### For Claude Code

When gaps are found:
1. Research agent creates reports in `_research/lacking/pending/`
2. Run `/docs/process-documentation-reports` to generate missing docs
3. All documentation follows conventions in `.claude/rules/documentation/README.md`

## Documentation Conventions

All conventions are defined in:
```
.claude/rules/documentation/README.md
```

This includes:
- Structure and organization
- File-to-doc mapping
- Templates for all documentation types
- Style guidelines
- When to create documentation
```

### 3. Create docs/INDEX.md

Get current timestamp and create INDEX.md:

```markdown
# Documentation Index

Last updated: {YYYY-MM-DD HH:MM:SS}

This index provides quick links to all documentation. Update this file whenever you create new documentation.

## Domains

*No domains documented yet. Add your first domain in `docs/domains/`.*

## Backend

*No backend documentation yet. Add documentation in `docs/layers/backend/`.*

## Frontend

*No frontend documentation yet. Add documentation in `docs/layers/frontend/`.*

---
*This index is automatically maintained by the documentation system.*
*See `.claude/rules/documentation/README.md` for conventions.*
```

### 4. Check for Existing Content

Before creating files, check if they already exist:
- If `docs/` already exists with content, preserve it
- Only create files that don't exist
- Warn the user if docs/ already has content

### 5. Explain What Was Created

After creating all files and directories, tell the user:

```
✅ Documentation structure created successfully!

Created:
- docs/ directory with README.md and INDEX.md
- docs/domains/ for business domain documentation
- docs/layers/backend/ and docs/layers/frontend/ for technical documentation
- docs/_research/ for research agent artifacts

Documentation conventions loaded from:
  .claude/rules/documentation/README.md

Next steps:
1. Read `.claude/rules/documentation/README.md` to understand the structure
2. Create your first domain: `docs/domains/{domain-name}/README.md`
3. Use the research agent to identify documentation gaps automatically
4. Run `/docs/process-documentation-reports` to generate missing documentation

The research-driven documentation workflow is now ready to use!
```

## Important Notes

- **Do NOT create `.claude/rules/documentation/README.md`** - that's handled by `/setup-rules`
- Create all directories even if empty (for consistency)
- Use current timestamp in INDEX.md
- Preserve any existing `docs/` content - this is additive
- If `docs/` already exists, warn the user and ask before proceeding
- All templates and conventions are in `.claude/rules/documentation/README.md`

## Error Handling

**If `.claude/rules/documentation/README.md` is missing:**
- Stop immediately
- Tell user to run `/setup-rules` first
- Explain that conventions must be in place before creating docs structure

**If `docs/` already exists:**
- Ask user: "docs/ directory already exists. Merge with existing content? [yes/no]"
- If yes: Create missing files only, preserve existing
- If no: Stop and tell user to manually handle it

**If individual files exist:**
- Skip those files
- Create only missing files
- Tell user which files were skipped
