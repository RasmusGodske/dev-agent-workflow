# Quick Start Guide

Get up and running with the Engineering Roles plugin in 5 minutes.

## Prerequisites

✅ Claude Code installed
✅ Linear MCP server configured (optional, for Linear commands)
✅ Project with `.claude/rules/` directory structure

## Step 1: Install the Plugin

### Option A: From GitHub (Production)

```bash
# Add the marketplace
/plugin marketplace add https://github.com/RasmusGodske/claude-project-roles

# Install the plugin
/plugin install project-roles@RasmusGodske

# Restart Claude Code when prompted
```

### Option B: Local Testing (Development)

```bash
# Navigate to parent directory
cd /home/vscode/project

# Add test marketplace
/plugin marketplace add ./test-engineering-marketplace

# Install the plugin
/plugin install project-roles@test-engineering-marketplace

# Restart Claude Code when prompted
```

## Step 2: Verify Installation

```bash
# Check available commands
/help

# You should see:
# - /roles/techlead
# - /roles/backend-engineer
# - /roles/frontend-engineer
# - /roles/fullstack-engineer
# - /roles/agent-engineer
# - /linear/start-project
# - /linear/review-project
# - /linear/work-on-issue
```

## Step 3: Set Up Your Project Structure

Ensure your project has this structure:

```
your-project/
├── .claude/
│   └── rules/
│       ├── backend/
│       │   ├── controller-conventions.md
│       │   ├── php-conventions.md
│       │   └── testing-conventions.md
│       ├── frontend/
│       │   └── vue-conventions.md
│       └── dataclasses/
│           └── laravel-data.md
```

**Don't have rules yet?** Create minimal starter rules:

```bash
mkdir -p .claude/rules/{backend,frontend,dataclasses}

# Backend rules
cat > .claude/rules/backend/php-conventions.md << 'EOF'
# PHP Conventions

- Always use type hints and return types
- Use named arguments for clarity
- Import classes at the top of files
- Follow PSR-12 coding standards
EOF

# Frontend rules
cat > .claude/rules/frontend/vue-conventions.md << 'EOF'
# Vue Conventions

- Use Composition API
- TypeScript for all components
- Props should be typed
- Use defineModel() for v-model
EOF

# Data class rules
cat > .claude/rules/dataclasses/laravel-data.md << 'EOF'
# Laravel Data Classes

- Use Spatie Laravel Data
- Constructor property promotion
- Validation via annotations
- Use Collection, not array
EOF
```

## Step 4: Try Your First Command

### Example 1: Activate a Role

```bash
# Become a backend engineer
/roles/backend-engineer

# Claude will load all backend conventions and adopt the role
```

### Example 2: Start a Linear Project

```bash
# Activate tech lead role first
/roles/techlead

# Start planning a project
/linear/start-project

# Follow the prompts to:
# 1. Describe your feature
# 2. Confirm git branch
# 3. Create Linear project
# 4. Break down into issues
```

### Example 3: Work on an Issue

```bash
# Activate appropriate role
/roles/backend-engineer

# Work on a specific issue
/linear/work-on-issue PRO-123

# Claude will:
# - Load issue context
# - Verify role matches
# - Update Linear status
# - Guide implementation
# - Mark issue as done
```

## Common Workflows

### Planning a Feature
```bash
/roles/techlead
/linear/start-project
# Answer prompts
# Issues created automatically
```

### Implementing Backend Work
```bash
/roles/backend-engineer
/linear/work-on-issue PRO-456
# Claude implements following conventions
```

### Reviewing Progress
```bash
/roles/techlead
/linear/review-project my-project
# Get comprehensive status briefing
```

## Tips

1. **Always activate a role first** - Ensures conventions are loaded
2. **Let Claude warn about role mismatches** - If you're a backend engineer working on frontend, Claude will notice
3. **Use Linear commands for team work** - Keeps everyone aligned
4. **Keep rules updated** - The plugin relies on current conventions

## Next Steps

- Read the full [README.md](./README.md) for detailed documentation
- Explore all available roles: `/help`
- Customize rules in `.claude/rules/` for your team
- Set up team auto-installation (see README)

## Troubleshooting

**Commands not showing?**
- Restart Claude Code after installation
- Run `/help` to verify

**Skills not loading conventions?**
- Check `.claude/rules/` exists
- Ensure files are markdown (*.md)

**Linear commands failing?**
- Verify Linear MCP server in `.mcp.json`
- Check LINEAR_API_KEY environment variable

## Getting Help

- Issues: [GitHub Issues](https://github.com/RasmusGodske/claude-project-roles/issues)
- Documentation: [README.md](./README.md)

Happy coding! 🚀
