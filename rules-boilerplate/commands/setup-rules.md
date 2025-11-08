---
description: Bootstrap .claude/rules/ structure with starter templates for Laravel/Vue projects
---

# Setup Rules Command

Create the `.claude/rules/` directory structure with starter convention templates for your Laravel/Vue project.

## What This Command Does

This command will:
1. Check if `.claude/rules/` already exists (won't overwrite existing rules)
2. Create the directory structure: `backend/`, `frontend/`, `dataclasses/`
3. Generate starter rule templates based on Laravel/Vue best practices
4. Provide guidance on customizing rules for your project

## Directory Structure Created

```
.claude/
└── rules/
    ├── backend/
    │   └── form-data-classes.md
    ├── frontend/
    │   ├── vue-conventions.md
    │   └── component-composition.md
    └── dataclasses/
        └── laravel-data.md
```

## Workflow

### Step 1: Check Existing Rules

First, check if `.claude/rules/` already exists:

Use Bash: `ls -la .claude/rules/ 2>/dev/null || echo "No rules directory found"`

If the directory exists with files:
- Ask: "I see you already have a `.claude/rules/` directory. Would you like me to:
  1. Skip setup (keep existing rules)
  2. Show you what templates I would create (for reference)
  3. Create templates with different names (e.g., `-template` suffix) so you can compare"

Wait for user choice.

If no rules exist or user wants to proceed, continue to Step 2.

### Step 2: Create Directory Structure

Create the directories:

```bash
mkdir -p .claude/rules/{backend,frontend,dataclasses}
```

### Step 3: Generate Backend Rules

Copy the template file from this plugin to the project:

#### .claude/rules/backend/form-data-classes.md

Use the template from `templates/backend/form-data-classes.md` file in this plugin.

This file contains conventions for:
- When to create Data classes vs using arrays
- Form Request validation patterns
- Data class structure and usage
- Converting form data to domain objects

### Step 4: Generate Frontend Rules

#### .claude/rules/frontend/vue-conventions.md

Use template with:
- Composition API patterns
- TypeScript usage
- Component structure
- Props and emits
- defineModel() usage

#### .claude/rules/frontend/component-composition.md

Use template with:
- When to split components
- Props vs composables
- Component reusability
- Naming conventions

### Step 5: Generate Data Class Rules

#### .claude/rules/dataclasses/laravel-data.md

Use template with:
- Spatie Laravel Data patterns
- Constructor property promotion
- Validation annotations
- Collection usage
- TypeScript export

### Step 6: Confirm and Guide

After creating files:

**Show summary:**
```
✅ Created .claude/rules/ structure with 4 starter templates:

Backend (1 file):
- form-data-classes.md

Frontend (2 files):
- vue-conventions.md
- component-composition.md

Data Classes (1 file):
- laravel-data.md

Next steps:
1. Review each file and customize for your project
2. Add project-specific patterns and examples
3. Remove sections that don't apply to your stack
4. Share with your team for feedback

These rules will be automatically loaded by the engineering-roles plugin when you activate roles like /roles/backend-engineer or /roles/frontend-engineer.
```

**Provide customization guidance:**
- "These are starter templates based on common Laravel/Vue best practices"
- "You should customize them to match your team's specific patterns"
- "Add real examples from your codebase"
- "Remove or modify sections that don't fit your project"
- "Consider adding rules for: [suggest based on project type]"

## Important Notes

- **Never overwrite existing rules without explicit confirmation**
- **Templates are starting points** - teams should customize
- **Examples should be generic** - real project examples are better
- **Keep rules concise** - focus on "what" and "why", not "how to code"

## Integration with Engineering Roles Plugin

These rules work seamlessly with the `engineering-roles` plugin:

- When you run `/roles/backend-engineer`, it loads `backend/` rules
- When you run `/roles/frontend-engineer`, it loads `frontend/` rules
- When you run `/roles/fullstack-engineer`, it loads both

The rules become the "source of truth" for your project conventions.

## Template Philosophy

Templates follow these principles:
1. **Opinionated but flexible** - Strong defaults, easy to modify
2. **Example-driven** - Show good vs bad patterns
3. **Context over commands** - Explain "why", not just "what"
4. **Project-agnostic** - Generic examples (Report, User, Product)
5. **Modern Laravel/Vue** - Current best practices (Laravel 10+, Vue 3)

---

Execute this command to bootstrap your project's convention rules.
