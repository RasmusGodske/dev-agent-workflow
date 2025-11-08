# Testing the Engineering Roles Plugin

This guide explains how to test the plugin locally before publishing.

## Setup Test Environment

### 1. Directory Structure

Your test environment should look like this:

```
project/
├── claude-project-roles/          # The plugin
│   ├── .claude-plugin/plugin.json
│   ├── commands/
│   ├── skills/
│   ├── README.md
│   └── ...
├── test-engineering-marketplace/      # Test marketplace
│   └── .claude-plugin/marketplace.json
└── your-test-project/                 # Project to test with
    └── .claude/
        └── rules/
```

### 2. Create Test Project

If you don't have a test project, create one:

```bash
mkdir test-project
cd test-project
mkdir -p .claude/rules/{backend,frontend,dataclasses}

# Create minimal rules files (see QUICKSTART.md)
```

## Installation for Testing

### Step 1: Add Test Marketplace

From your test project directory:

```bash
# Start Claude Code
claude

# Add the test marketplace
/plugin marketplace add ../test-engineering-marketplace
```

### Step 2: Install Plugin

```bash
# Install the plugin
/plugin install project-roles@test-engineering-marketplace

# Select "Install now"
# Restart Claude Code when prompted
```

### Step 3: Verify Installation

```bash
# Check commands are available
/help

# Should show all project-roles commands:
# /roles/techlead
# /roles/backend-engineer
# /roles/frontend-engineer
# /roles/fullstack-engineer
# /roles/agent-engineer
# /linear/start-project
# /linear/review-project
# /linear/work-on-issue
```

## Test Cases

### Test 1: Role Activation

```bash
# Test backend engineer role
/roles/backend-engineer

# Expected:
# - Claude adopts backend engineer persona
# - Attempts to load .claude/rules/backend/*.md files
# - Explains what conventions were loaded

# Test other roles
/roles/frontend-engineer
/roles/fullstack-engineer
/roles/techlead
/roles/agent-engineer
```

**Pass Criteria:**
- ✅ Role activates without errors
- ✅ Claude attempts to read rules from .claude/rules/
- ✅ Role persona is adopted (check by asking "what's your role?")

### Test 2: Skills Auto-Activation

Skills should be activated automatically based on context:

```bash
# Activate backend engineer
/roles/backend-engineer

# Ask about creating a Data class
"How do I create a new Data class for user registration?"

# Expected:
# - backend-developer skill loads rules
# - laravel-data-writer skill may activate
# - Response references loaded conventions
```

**Pass Criteria:**
- ✅ Skills activate based on context
- ✅ Conventions from rules are referenced
- ✅ Guidance matches skill patterns

### Test 3: Linear Commands (If MCP Configured)

```bash
# Start a project
/roles/techlead
/linear/start-project

# Expected prompts:
# 1. Feature description
# 2. Git branch confirmation
# 3. Project creation confirmation
# 4. Issue breakdown
# 5. Next steps

# Work on an issue
/roles/backend-engineer
/linear/work-on-issue <issue-id>

# Expected:
# - Issue loads
# - Role match verification
# - Branch verification
# - Status updates to "In Progress"
# - Implementation guidance
# - Status updates to "Done" with comment

# Review project
/roles/techlead
/linear/review-project <project-name>

# Expected comprehensive briefing
```

**Pass Criteria:**
- ✅ Commands execute without errors
- ✅ Linear API calls succeed
- ✅ Issue statuses update correctly
- ✅ Comments are created with proper format

### Test 4: Role Mismatch Detection

```bash
# Activate backend engineer
/roles/backend-engineer

# Work on frontend issue
/linear/work-on-issue <frontend-issue-id>

# Expected:
# - Warning about role mismatch
# - Recommendation to switch to frontend-engineer
# - Option to proceed anyway or wait
```

**Pass Criteria:**
- ✅ Mismatch detected correctly
- ✅ Warning displayed clearly
- ✅ User given choices

## Iteration Workflow

When making changes to the plugin:

### 1. Make Changes

Edit files in `claude-project-roles/`:
- Commands in `commands/`
- Skills in `skills/`
- Documentation in root

### 2. Uninstall Current Version

```bash
/plugin uninstall project-roles@test-engineering-marketplace
```

### 3. Reinstall Updated Version

```bash
/plugin install project-roles@test-engineering-marketplace
```

### 4. Restart Claude Code

Required for changes to take effect.

### 5. Test Changes

Run relevant test cases from above.

## Common Issues

### Plugin Not Found
**Symptom:** "Plugin not found" when installing

**Solutions:**
- Check marketplace.json path to plugin is correct
- Verify plugin.json exists in `.claude-plugin/`
- Ensure marketplace was added successfully

### Commands Not Showing
**Symptom:** `/help` doesn't show new commands

**Solutions:**
- Restart Claude Code after installation
- Uninstall and reinstall plugin
- Check command files are in `commands/` directory with `.md` extension

### Skills Not Activating
**Symptom:** Skills don't load conventions

**Solutions:**
- Verify test project has `.claude/rules/` structure
- Check SKILL.md files exist in each skill directory
- Ensure rules files are markdown (*.md)

### Linear Commands Failing
**Symptom:** Linear commands error or fail

**Solutions:**
- Verify Linear MCP server is configured in `.mcp.json`
- Check LINEAR_API_KEY environment variable is set
- Test Linear MCP server with basic queries
- Ensure you have access to Linear workspace

## Validation Checklist

Before considering the plugin ready for release:

- [ ] All 5 roles activate without errors
- [ ] Skills load conventions from `.claude/rules/`
- [ ] Linear commands work (if MCP configured)
- [ ] Role mismatch detection works correctly
- [ ] Documentation is complete and accurate
- [ ] README has clear installation instructions
- [ ] QUICKSTART guide works for new users
- [ ] LICENSE file is present
- [ ] plugin.json has correct metadata
- [ ] No project-specific references remain
- [ ] Code examples are generic

## Performance Testing

### Rule Loading Speed

```bash
# Time how long roles take to load rules
time /roles/backend-engineer

# Should be < 5 seconds with typical rules
```

### Linear Integration

```bash
# Test issue creation speed
/linear/start-project
# Track time to create project + 5 issues
# Should be < 30 seconds
```

## Documentation Testing

Have someone unfamiliar with the plugin:

1. Follow QUICKSTART.md from scratch
2. Install and test basic workflow
3. Note any confusion or missing steps
4. Update documentation based on feedback

## Next Steps

Once all tests pass:

1. Push to GitHub repository
2. Tag release (v1.0.0)
3. Update marketplace.json to point to GitHub
4. Share with team for testing
5. Gather feedback and iterate

## Getting Help

If you encounter issues during testing:

1. Check plugin.json is valid JSON
2. Verify directory structure matches documentation
3. Check Claude Code logs for errors
4. Review marketplace.json configuration
5. Create GitHub issue with:
   - What you tried
   - Expected behavior
   - Actual behavior
   - Error messages

Happy testing! 🧪
