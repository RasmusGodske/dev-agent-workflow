---
name: documentation-writer
description: Skill for writing and updating codebase documentation. Use when creating or editing markdown documentation files in the docs/ directory, README files, or any documentation-related content. Also activates when maintaining the documentation index.
---

# Documentation Writer Skill

You are writing or updating project documentation. This skill ensures you follow project conventions and maintain consistency.

## When This Skill Activates

This skill activates when you are:
- Creating new documentation files in `docs/`
- Editing existing documentation in `docs/`
- Updating `docs/INDEX.md`
- Working on domain, feature, or layer documentation
- Updating documentation as part of code changes

## Load Documentation Conventions

**Before writing any documentation**, load the project's conventions:

```
Use Glob to find: .claude/rules/documentation/**/*.md
Read each file found
```

These files define:
- Documentation structure (domains, layers, features)
- File-to-doc mapping conventions
- Templates for different documentation types
- Writing style guidelines
- When to create documentation
- INDEX.md maintenance rules

## Follow the Conventions

All documentation practices are defined in `.claude/rules/documentation/`. Your job is to:

1. **Load the conventions first**
2. **Follow the structure** defined there (domains, layers, placement)
3. **Use the templates** provided for consistency
4. **Maintain INDEX.md** as specified in conventions
5. **Follow style guidelines** for clarity and completeness

## Critical Reminders

- **Always update `docs/INDEX.md`** when creating new documentation
- **Check INDEX.md first** before creating docs (might already exist)
- **Use lowercase-with-hyphens** for file names
- **Include code references** with line numbers: `path/to/file.php:123`
- **Link generously** between related documentation

## Integration with Other Skills

This skill works alongside:
- **backend-developer** - When backend code changes need doc updates
- **frontend-developer** - When frontend code changes need doc updates
- **research-agent** - Provides context for documentation gaps
- **process-documentation-reports** - Uses this skill when generating docs

## Quality Gate

This skill is the "quality gate" ensuring all documentation, whether created manually or from research reports, meets project standards defined in `.claude/rules/documentation/`.
