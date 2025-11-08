# Engineering Roles Plugin for Claude Code

A comprehensive role-based engineering workflow system for Laravel/Vue projects with Linear integration. This plugin provides specialized roles, commands, and skills to streamline your development workflow.

## Features

### 🎭 Role-Based Engineering Workflows
- **Tech Lead** - Architecture planning and feature breakdown
- **Backend Engineer** - PHP/Laravel implementation
- **Frontend Engineer** - Vue.js/TypeScript UI development
- **Fullstack Engineer** - End-to-end feature implementation
- **Agent Engineer** - Meta-level agent system understanding

### 📋 Linear Project Management
- **Start Project** - Plan and create Linear projects with detailed breakdown
- **Review Project** - Get comprehensive project status briefings
- **Work on Issue** - Load and implement specific Linear issues with role-aware guidance

### 🛠️ Specialized Skills
- **backend-developer** - Load backend conventions and patterns
- **frontend-developer** - Load frontend conventions and patterns
- **laravel-data-writer** - Create Spatie Laravel Data classes
- **php-test-writer** - Write comprehensive PHP tests
- **linear-issue-writer** - Create detailed, actionable Linear issues

## Prerequisites

- Claude Code v1.0.0 or higher
- Linear MCP server configured (for Linear commands)
- Project with `.claude/rules/` directory structure:
  - `.claude/rules/backend/` - Backend conventions
  - `.claude/rules/frontend/` - Frontend conventions
  - `.claude/rules/dataclasses/` - Data class patterns

## Installation

### Quick Install

```bash
# Add the marketplace
/plugin marketplace add https://github.com/RasmusGodske/claude-project-roles

# Install the plugin
/plugin install project-roles@RasmusGodske
```

### Local Development Testing

1. Clone this repository
2. Create a test marketplace structure:
```bash
mkdir test-marketplace
cd test-marketplace
mkdir .claude-plugin

# Create marketplace.json
cat > .claude-plugin/marketplace.json << 'EOF'
{
  "name": "test-marketplace",
  "owner": { "name": "Test User" },
  "plugins": [{
    "name": "project-roles",
    "source": "./path/to/claude-project-roles",
    "description": "Engineering roles plugin under development"
  }]
}
EOF
```

3. Add the marketplace to Claude Code:
```bash
/plugin marketplace add ./test-marketplace
```

4. Install the plugin:
```bash
/plugin install project-roles@test-marketplace
```

## Usage

### Activating Roles

Roles set your context and load relevant conventions:

```bash
# Activate a role
/roles/techlead          # For architecture and planning
/roles/backend-engineer  # For backend implementation
/roles/frontend-engineer # For frontend implementation
/roles/fullstack-engineer # For full-stack work
/roles/agent-engineer    # For agent system work
```

### Linear Commands

#### Start a New Project
Plan and create a Linear project with comprehensive breakdown:

```bash
/linear/start-project
```

The command will:
1. Understand your feature requirements
2. Identify the Git branch
3. Create a Linear project with detailed documentation
4. Break down work into implementable issues
5. Ask if you want to start implementing immediately

#### Review Project Status
Get a comprehensive briefing on project status:

```bash
/linear/review-project <project-name-or-id>
```

Provides:
- Project overview and context
- Current status and progress
- Completed work summary
- Remaining tasks
- Last activity
- Technical considerations
- Recommendations

#### Work on an Issue
Load and implement a specific Linear issue:

```bash
/linear/work-on-issue <issue-id>
```

The command will:
1. Load issue and project context
2. Verify role matches the task type
3. Check if you're on the correct branch
4. Update issue to "In Progress"
5. Guide implementation based on your active role
6. Update issue to "Done" with detailed comment

### Using Skills

Skills are automatically activated by Claude based on context, but you can also invoke them explicitly:

- **backend-developer** - Loads all backend conventions before implementation
- **frontend-developer** - Loads all frontend conventions before implementation
- **laravel-data-writer** - Guides Data class creation
- **php-test-writer** - Guides test creation
- **linear-issue-writer** - Helps create detailed, feature-focused issues

## Project Structure Requirements

For this plugin to work effectively, your project should have:

```
your-project/
├── .claude/
│   ├── rules/
│   │   ├── backend/           # Backend conventions (*.md files)
│   │   ├── frontend/          # Frontend conventions (*.md files)
│   │   └── dataclasses/       # Data class patterns (*.md files)
│   └── settings.json
└── ... (your code)
```

### Example Rules Structure

```bash
# Backend conventions
.claude/rules/backend/
├── controller-conventions.md
├── database-conventions.md
├── php-conventions.md
└── testing-conventions.md

# Frontend conventions
.claude/rules/frontend/
├── vue-conventions.md
└── component-composition.md

# Data class conventions
.claude/rules/dataclasses/
└── laravel-data.md
```

## Workflow Examples

### Planning a New Feature

```bash
# 1. Activate tech lead role
/roles/techlead

# 2. Start the project
/linear/start-project

# 3. Follow prompts to create project and issues
# 4. Start implementing
/roles/backend-engineer
/linear/work-on-issue PRO-123
```

### Implementing a Feature

```bash
# 1. Activate appropriate role
/roles/backend-engineer

# 2. Work on the issue
/linear/work-on-issue PRO-456

# 3. Claude will:
#    - Load backend conventions
#    - Verify role matches task
#    - Update Linear issue status
#    - Guide implementation
#    - Run tests
#    - Mark issue as Done
```

### Reviewing Project Status

```bash
# As tech lead
/roles/techlead
/linear/review-project dashboard-redesign

# Get comprehensive briefing on:
# - What's done
# - What's remaining
# - Current progress
# - Recommendations
```

## Role Behaviors

### Tech Lead
- **Focus**: Architecture, planning, task breakdown
- **Loads**: Relevant rules based on work type
- **Linear Integration**: Creates projects and issues
- **Approach**: Thinks architecturally, plans for others

### Backend Engineer
- **Focus**: Backend implementation quality
- **Loads**: All backend and data class rules
- **Linear Integration**: Updates issue status and comments
- **Approach**: Test-driven, convention-focused

### Frontend Engineer
- **Focus**: UI/UX implementation
- **Loads**: All frontend rules
- **Linear Integration**: Updates issue status and comments
- **Approach**: Component-driven, user-first

### Fullstack Engineer
- **Focus**: End-to-end implementation
- **Loads**: Both backend and frontend rules
- **Linear Integration**: Updates issue status with both layers
- **Approach**: System thinking, clean integration

### Agent Engineer
- **Focus**: Understanding and improving the agent system
- **Loads**: Meta-level understanding
- **Approach**: Architectural, organizational, clear communication

## Configuration

### Team Setup

To automatically install this plugin for your team, add to `.claude/settings.json`:

```json
{
  "plugins": {
    "marketplaces": [
      {
        "source": "https://github.com/RasmusGodske/claude-project-roles"
      }
    ],
    "installed": [
      {
        "name": "project-roles",
        "marketplace": "RasmusGodske"
      }
    ]
  }
}
```

### Linear MCP Server

Ensure Linear MCP server is configured in your `.mcp.json`:

```json
{
  "mcpServers": {
    "linear-server": {
      "command": "npx",
      "args": ["-y", "@linear/mcp-server-linear"],
      "env": {
        "LINEAR_API_KEY": "your-api-key"
      }
    }
  }
}
```

## Best Practices

1. **Always activate a role first** - Ensures proper context and convention loading
2. **Use Linear commands for trackable work** - Keeps team aligned on progress
3. **Let Claude detect role mismatches** - Trust the warnings when your role doesn't match the task
4. **Keep rules updated** - The plugin relies on your `.claude/rules/` being current
5. **Review project status regularly** - Use `/linear/review-project` to stay aligned

## Troubleshooting

### Commands Not Appearing
- Restart Claude Code after plugin installation
- Run `/help` to verify commands are loaded

### Skills Not Working
- Ensure `.claude/rules/` directories exist
- Check that rule files are markdown (*.md)

### Linear Commands Failing
- Verify Linear MCP server is configured
- Check LINEAR_API_KEY is set correctly
- Ensure you have access to the Linear workspace

### Role Mismatch Warnings
- This is intentional! Claude is helping you stay aligned
- Consider switching roles or continuing if you know what you're doing

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Test your changes with a local marketplace
4. Submit a pull request

## License

MIT License - See LICENSE file for details

## Author

**RasmusGodske**
- GitHub: [@RasmusGodske](https://github.com/RasmusGodske)

## Support

- Issues: [GitHub Issues](https://github.com/RasmusGodske/claude-project-roles/issues)
- Documentation: [GitHub Repository](https://github.com/RasmusGodske/claude-project-roles)

## Changelog

### v1.0.0 (Initial Release)
- Role-based engineering workflows (5 roles)
- Linear project management commands (3 commands)
- Specialized skills (5 skills)
- Comprehensive documentation
- Laravel/Vue project support
