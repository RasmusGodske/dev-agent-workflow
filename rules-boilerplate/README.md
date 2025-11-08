# Rules Boilerplate Plugin for Claude Code

Bootstrap your Laravel/Vue project with starter convention rules. Creates the `.claude/rules/` structure needed by the project-roles plugin.

## Purpose

This plugin helps you quickly set up convention rules for your Laravel/Vue project by:
- Creating the `.claude/rules/` directory structure
- Generating starter rule templates
- Providing guidance on customization

These rules are then automatically loaded by the [project-roles plugin](https://github.com/RasmusGodske/claude-project-roles) when you activate roles like `/roles/backend-engineer`.

## What It Creates

```
.claude/
└── rules/
    ├── backend/
    │   └── form-data-classes.md      # Data class conventions
    ├── frontend/
    │   ├── vue-conventions.md         # Vue.js patterns
    │   └── component-composition.md   # Component patterns
    └── dataclasses/
        └── laravel-data.md            # Spatie Laravel Data
```

## Installation

```bash
# Add marketplace
/plugin marketplace add https://github.com/RasmusGodske/rules-boilerplate

# Install plugin
/plugin install rules-boilerplate@RasmusGodske
```

## Usage

### Quick Start

```bash
# Run the setup command
/setup-rules

# Follow the prompts
# Templates will be created in .claude/rules/
```

The command will:
1. Check if `.claude/rules/` already exists (won't overwrite)
2. Create directory structure
3. Generate starter templates
4. Provide customization guidance

### For Existing Projects

If you already have `.claude/rules/`, the command will ask if you want to:
- Skip setup (keep existing rules)
- View templates (for reference)
- Create templates with different names (for comparison)

## Template Contents

### Backend Rules

**form-data-classes.md**
- When to use Data classes vs arrays
- Form Request validation patterns
- Data class structure conventions
- Type safety practices

### Frontend Rules

**vue-conventions.md**
- Vue 3 Composition API patterns
- TypeScript usage
- Component structure
- Props and emits patterns

**component-composition.md**
- When to split components
- Component reusability
- Props vs composables
- Naming conventions

### Data Class Rules

**laravel-data.md**
- Spatie Laravel Data patterns
- Constructor property promotion
- Validation via annotations
- Collection usage over arrays
- TypeScript export patterns

## Customization

After running `/setup-rules`, you should:

1. **Review each file** - Read through the templates
2. **Add project-specific examples** - Replace generic examples with real ones from your codebase
3. **Remove irrelevant sections** - If you don't use certain patterns, remove them
4. **Add missing conventions** - Include patterns unique to your project
5. **Share with team** - Get feedback and iterate

## Integration with Engineering Roles

These rules work seamlessly with the [project-roles plugin](https://github.com/RasmusGodske/claude-project-roles):

```bash
# Install both plugins
/plugin install rules-boilerplate@RasmusGodske
/plugin install project-roles@RasmusGodske

# Set up rules
/setup-rules

# Activate a role (automatically loads rules)
/roles/backend-engineer  # Loads .claude/rules/backend/*.md
/roles/frontend-engineer # Loads .claude/rules/frontend/*.md
/roles/fullstack-engineer # Loads both
```

## Example Workflow

### New Project Setup

```bash
# 1. Create project
laravel new my-project
cd my-project

# 2. Start Claude Code
claude

# 3. Install plugins
/plugin install rules-boilerplate@RasmusGodske
/plugin install project-roles@RasmusGodske

# 4. Bootstrap rules
/setup-rules

# 5. Customize templates for your project
# (Edit files in .claude/rules/)

# 6. Start working with roles
/roles/backend-engineer
# Now Claude knows your project conventions!
```

### Existing Project

```bash
# If you already have conventions but want to formalize them

# 1. Install plugin
/plugin install rules-boilerplate@RasmusGodske

# 2. Create templates as reference
/setup-rules
# Choose "Create with -template suffix"

# 3. Compare with your existing conventions
# Templates created as:
# - form-data-classes-template.md
# - vue-conventions-template.md
# etc.

# 4. Merge useful patterns into your rules
```

## Benefits

### For New Projects
- ✅ Instant convention structure
- ✅ Laravel/Vue best practices
- ✅ No need to write rules from scratch
- ✅ Ready for project-roles plugin

### For Teams
- ✅ Consistent starting point
- ✅ Shared conventions across projects
- ✅ Easy onboarding for new members
- ✅ Professional standards from day one

### For Solo Developers
- ✅ Personal conventions library
- ✅ Reuse patterns across projects
- ✅ Improve code quality
- ✅ Learn best practices

## Template Philosophy

Templates are designed to be:
- **Opinionated but flexible** - Strong defaults you can change
- **Example-driven** - Show good vs bad patterns
- **Modern** - Laravel 10+, Vue 3, PHP 8+
- **Generic** - Easy to customize for any project
- **Comprehensive** - Cover common scenarios

## Troubleshooting

**Command not showing?**
- Restart Claude Code after installation
- Run `/help` to verify

**Templates too opinionated?**
- They're starting points! Customize freely
- Remove sections that don't fit
- Add your own patterns

**Missing conventions for my stack?**
- Templates focus on Laravel/Vue core
- Add your own files for additional tech
- Pull requests welcome!

## Contributing

Have better template conventions? Contributions welcome!

1. Fork the repository
2. Update template files
3. Test with a fresh project
4. Submit pull request

## License

MIT License - See LICENSE file

## Author

**RasmusGodske**
- GitHub: [@RasmusGodske](https://github.com/RasmusGodske)

## Related Plugins

- [project-roles](https://github.com/RasmusGodske/claude-project-roles) - Role-based workflows that use these rules

## Support

- Issues: [GitHub Issues](https://github.com/RasmusGodske/rules-boilerplate/issues)
- Documentation: This README

## Changelog

### v1.0.0
- Initial release
- 4 starter templates (backend, frontend, dataclasses)
- `/setup-rules` command
- Integration with project-roles plugin
