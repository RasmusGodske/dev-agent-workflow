# Dev Agent Workflow

Agent-based development workflows for Laravel/Vue projects with Linear integration.

## Plugins

### 🎭 [project-roles](./project-roles)
Role-based workflows for Laravel/Vue projects with Linear integration.

**Features:**
- 5 specialized roles (Tech Lead, Backend/Frontend/Fullstack Engineer, Agent Engineer)
- 3 Linear commands (start project, review project, work on issue)
- 5 generic skills (backend-developer, frontend-developer, laravel-data-writer, php-test-writer, linear-issue-writer)

[📖 Full Documentation](./project-roles/README.md)

### 📋 [rules-boilerplate](./rules-boilerplate)
Bootstrap `.claude/rules/` structure with starter templates.

**Features:**
- `/setup-rules` command to create directory structure
- Backend, frontend, and data class convention templates
- Based on Laravel/Vue best practices
- Works seamlessly with project-roles plugin

[📖 Full Documentation](./rules-boilerplate/README.md)

## Installation

### Quick Install (Both Plugins)

```bash
# Add marketplace
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow

# Install both plugins
/plugin install project-roles@RasmusGodske
/plugin install rules-boilerplate@RasmusGodske

# Restart Claude Code
```

### Individual Installation

```bash
# Just project-roles
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow
/plugin install project-roles@RasmusGodske

# Just rules-boilerplate
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow
/plugin install rules-boilerplate@RasmusGodske
```

## Quick Start

### New Project Setup

```bash
# 1. Install plugins
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow
/plugin install project-roles@RasmusGodske
/plugin install rules-boilerplate@RasmusGodske

# Restart Claude Code

# 2. Bootstrap rules
/setup-rules

# 3. Customize rules for your project
# Edit files in .claude/rules/

# 4. Start working with roles
/roles/backend-engineer
```

### Existing Project

```bash
# Install plugins
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow
/plugin install project-roles@RasmusGodske

# If you don't have .claude/rules/ yet:
/plugin install rules-boilerplate@RasmusGodske
/setup-rules
```

## Workflow Example

```bash
# 1. Activate tech lead role
/roles/techlead

# 2. Start planning a feature
/linear/start-project

# 3. Switch to implementation role
/roles/backend-engineer

# 4. Work on an issue
/linear/work-on-issue PRO-123

# 5. Review project progress
/roles/techlead
/linear/review-project my-project
```

## Team Setup (Auto-Install)

Add to your project's `.claude/settings.json`:

```json
{
  "plugins": {
    "marketplaces": [
      {
        "source": "https://github.com/RasmusGodske/dev-agent-workflow"
      }
    ],
    "installed": [
      {
        "name": "project-roles",
        "marketplace": "RasmusGodske"
      },
      {
        "name": "rules-boilerplate",
        "marketplace": "RasmusGodske"
      }
    ]
  }
}
```

When team members trust the repository folder, plugins install automatically.

## Requirements

- Claude Code v1.0.0 or higher
- Linear MCP server configured (for Linear commands)
- Laravel/Vue project (optional, but designed for it)

## Documentation

- [project-roles Documentation](./project-roles/README.md)
- [project-roles Quick Start](./project-roles/QUICKSTART.md)
- [project-roles Testing Guide](./project-roles/TESTING.md)
- [rules-boilerplate Documentation](./rules-boilerplate/README.md)

## License

MIT License - See individual plugin LICENSE files

## Author

**RasmusGodske**
- GitHub: [@RasmusGodske](https://github.com/RasmusGodske)

## Support

- Issues: [GitHub Issues](https://github.com/RasmusGodske/dev-agent-workflow/issues)
- Discussions: [GitHub Discussions](https://github.com/RasmusGodske/dev-agent-workflow/discussions)

## Changelog

### v1.0.0 (Initial Release)
- **project-roles**: Role-based workflows with Linear integration
- **rules-boilerplate**: Convention template bootstrapping
