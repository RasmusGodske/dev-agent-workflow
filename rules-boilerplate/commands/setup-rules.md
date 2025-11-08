---
description: Clone Laravel/Vue convention rules into .claude/rules/
---

# Setup Rules Command

Clone the Laravel/Vue convention rules repository into your project's `.claude/rules/` directory.

## What This Command Does

This command will:
1. Check if `.claude/rules/` already exists (won't overwrite)
2. Clone the rules repository: `https://github.com/RasmusGodske/laravel-vue-rules`
3. Set up as git submodule (recommended) or regular clone
4. Provide guidance on customizing and updating rules

## What Gets Cloned

```
.claude/rules/                         # Cloned from laravel-vue-rules repo
├── backend/
│   └── form-data-classes.md
├── frontend/
│   ├── vue-conventions.md
│   └── component-composition.md
├── dataclasses/
│   └── laravel-data.md
└── README.md
```

## Workflow

### Step 1: Check Existing Rules

First, check if `.claude/rules/` already exists:

Use Bash: `ls -la .claude/rules/ 2>/dev/null || echo "No rules directory found"`

If the directory exists:
- Ask: "I see you already have a `.claude/rules/` directory. Would you like me to:
  1. Skip setup (keep existing rules)
  2. Backup existing and clone fresh rules
  3. Cancel"

Wait for user choice. If user wants to skip or cancel, stop here.

### Step 2: Ask About Git Submodule

Ask user: "Would you like to set up the rules as a git submodule (recommended) or a regular clone?

**Git Submodule (Recommended):**
- ✅ Easy to update (git pull in submodule)
- ✅ Can track specific version
- ✅ Clear separation from your code
- ⚠️ Requires git repository

**Regular Clone:**
- ✅ Simpler setup
- ✅ No git dependencies
- ❌ Harder to update
- ❌ Becomes part of your project"

Wait for user choice.

### Step 3: Clone the Rules

#### Option A: Git Submodule (Recommended)

If user chose submodule:

```bash
# Ensure .claude directory exists
mkdir -p .claude

# Check if project is a git repo
if [ -d .git ]; then
    git submodule add https://github.com/RasmusGodske/laravel-vue-rules .claude/rules
    git submodule update --init --recursive
    echo "✅ Added as git submodule"
else
    echo "⚠️ Not a git repository. Using regular clone instead."
    git clone https://github.com/RasmusGodske/laravel-vue-rules .claude/rules
fi
```

#### Option B: Regular Clone

If user chose regular clone:

```bash
# Ensure .claude directory exists
mkdir -p .claude

# Clone the rules
git clone https://github.com/RasmusGodske/laravel-vue-rules .claude/rules

# Remove git history to make it part of your project
cd .claude/rules
rm -rf .git
cd ../..

echo "✅ Cloned rules (non-submodule)"
```

### Step 4: Confirm and Guide

After cloning:

**Show summary:**
```
✅ Cloned laravel-vue-rules into .claude/rules/

Files included:
- backend/form-data-classes.md
- frontend/vue-conventions.md
- frontend/component-composition.md
- dataclasses/laravel-data.md
- README.md

Next steps:
1. Review each file and customize for your project
2. Add project-specific patterns and examples
3. Remove sections that don't apply to your stack

Updating rules (if using submodule):
cd .claude/rules
git pull origin main

Forking for your team:
1. Fork https://github.com/RasmusGodske/laravel-vue-rules
2. Customize rules in your fork
3. Update submodule URL:
   git submodule set-url .claude/rules https://github.com/your-org/laravel-vue-rules

These rules will be automatically loaded by project-roles plugin when you activate roles like /roles/backend-engineer or /roles/frontend-engineer.
```

**Provide customization guidance:**
- "These are starter templates based on common Laravel/Vue best practices"
- "You should customize them to match your team's specific patterns"
- "Add real examples from your codebase"
- "Consider forking the repo for your organization to maintain custom rules"

## Important Notes

- **Never overwrite existing rules without explicit confirmation**
- **Templates are starting points** - customize for your team
- **Git submodule recommended** - easier to update and track changes
- **Fork for team use** - maintain your own version with team conventions

## Integration with project-roles Plugin

These rules work seamlessly with the `project-roles` plugin:

- When you run `/roles/backend-engineer`, it loads `backend/` rules
- When you run `/roles/frontend-engineer`, it loads `frontend/` rules
- When you run `/roles/fullstack-engineer`, it loads both

The rules become the "source of truth" for your project conventions.

## Updating Rules

### If Using Git Submodule

```bash
cd .claude/rules
git pull origin main
cd ../..
git add .claude/rules
git commit -m "Update convention rules"
```

### If Using Regular Clone

You'll need to manually pull changes or re-clone:

```bash
cd .claude/rules
git pull https://github.com/RasmusGodske/laravel-vue-rules main
```

---

Execute this command to clone convention rules into your project.
