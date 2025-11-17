# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **Claude Code plugin marketplace repository** containing two plugins for agent-based development workflows:

1. **project-roles** - Role-based workflows for Laravel/Vue projects with Linear integration
2. **rules-boilerplate** - Bootstrap `.claude/rules/` structure with starter templates

Both plugins work together to provide structured, convention-driven development workflows.

### Important Context Distinction

This repository serves two purposes:

1. **Plugin Development** - Contains plugin source code with `commands/` and `skills/`
2. **Plugin Distribution** - Users install these plugins which add commands and skills to their Claude Code instance

When working in this repository:
- Modifying `project-roles/commands/roles/techlead.md` changes what `/roles/techlead` does
- Modifying `project-roles/skills/backend-developer/SKILL.md` changes when/how that skill activates
- The `agent-engineer` command teaches about USER project structure (not this plugin's structure)
- User projects will have `.claude/rules/` directories; this plugin repository does not

## Repository Structure

```
dev-agent-workflow/
├── project-roles/           # Plugin: Role-based workflows
│   ├── .claude-plugin/      # Plugin metadata
│   │   └── plugin.json
│   ├── commands/
│   │   ├── roles/           # 5 role commands (techlead, backend-engineer, etc.)
│   │   └── linear/          # 3 Linear commands (start-project, review-project, work-on-issue)
│   └── skills/              # 5 specialized skills
│
├── rules-boilerplate/       # Plugin: Convention templates
│   ├── .claude-plugin/      # Plugin metadata
│   │   └── plugin.json
│   └── commands/
│       └── setup-rules.md   # Bootstrap command
│
└── README.md                # Main installation/usage guide
```

## Key Concepts

### Plugin Structure
Each plugin follows Claude Code's plugin format:
- `.claude-plugin/plugin.json` - Plugin metadata (name, version, description, author)
- `commands/` - Slash commands (user-invoked prompts that expand in main conversation)
- `skills/` - Model-invoked capabilities (Claude activates autonomously based on context)
- `agents/` - Custom subagents with separate context windows (this repository has none)

**Important distinction:**
- **Commands** (e.g., `/roles/techlead`) - User types these; they expand prompts in the main conversation
- **Skills** (e.g., `backend-developer`) - Claude invokes these automatically when needed
- **Subagents** (e.g., `code-reviewer`) - Delegated AI assistants with separate context (not used in this repo)

### project-roles Architecture
The plugin implements a role-based workflow system using **commands** and **skills**:

**Commands** (`commands/` directory):
- **Role Commands** (`commands/roles/*.md`) - Set context and mindset in main conversation
  - Examples: `/roles/techlead`, `/roles/backend-engineer`
  - These are NOT subagents - they expand prompts directly in the main thread
  - Load conventions from user's `.claude/rules/` directory
- **Linear Commands** (`commands/linear/*.md`) - Project management workflows
  - Examples: `/linear/start-project`, `/linear/work-on-issue`
  - Use Linear MCP server tools to integrate with Linear
  - Execute in main conversation context

**Skills** (`skills/` directory):
- Claude autonomously activates these based on task context
- Examples: `backend-developer`, `frontend-developer`, `laravel-data-writer`, `php-test-writer`
- Each skill has YAML frontmatter with `name:` and `description:` that tells Claude when to use it
- Skills load conventions from user's `.claude/rules/` directory

**Role Command Pattern:**
When a user invokes `/roles/backend-engineer`:
1. Command expands prompt in main conversation (not a separate context)
2. Prompt instructs Claude to load `.claude/rules/backend/*.md` and `.claude/rules/dataclasses/*.md`
3. Sets "Backend Engineer" mindset and approach for subsequent work
4. All work happens in the main conversation thread

**Linear Integration via MCP:**
Commands use Linear MCP server tools:
- `mcp__linear-server__get_issue` - Fetch issue details
- `mcp__linear-server__get_project` - Fetch project context
- `mcp__linear-server__update_issue` - Update status
- `mcp__linear-server__create_comment` - Add implementation notes

### rules-boilerplate Purpose
Generates starter convention templates that project-roles expects:
- Backend conventions (PHP, Laravel, testing)
- Frontend conventions (Vue 3, TypeScript, components)
- Data class conventions (Spatie Laravel Data)

### User Project Structure
The plugins expect users to have a `.claude/rules/` directory in their projects:

```
user-project/
├── .claude/
│   ├── rules/              # Single source of truth for conventions
│   │   ├── backend/        # Backend conventions loaded by backend role
│   │   ├── frontend/       # Frontend conventions loaded by frontend role
│   │   └── dataclasses/    # Data class patterns
│   └── settings.json       # Plugin installation config
└── ... (user code)
```

**Convention hierarchy** (as taught by the `agent-engineer` command):
1. **Rules** (`.claude/rules/`) - Single source of truth for project conventions
2. **Skills** - Load and apply rules; provide context on when to use patterns
3. **Role Commands** - Load rules and set mindset for extended work
4. **Workflow Commands** - Use rules during planning and implementation

This separation ensures conventions are defined once in `.claude/rules/` and referenced everywhere else.

## Development Commands

### Testing Plugins Locally

Create a test marketplace structure:
```bash
mkdir test-marketplace
cd test-marketplace
mkdir .claude-plugin

cat > .claude-plugin/marketplace.json << 'EOF'
{
  "name": "test-marketplace",
  "owner": { "name": "Test User" },
  "plugins": [{
    "name": "project-roles",
    "source": "/path/to/dev-agent-workflow/project-roles",
    "description": "Testing project-roles"
  }]
}
EOF

# Add marketplace and install
/plugin marketplace add ./test-marketplace
/plugin install project-roles@test-marketplace
```

### Iterating on Plugin Changes

When developing plugins, use the uninstall/reinstall cycle:

```bash
# After making changes to plugin files
/plugin uninstall project-roles@test-marketplace

# Reinstall to test changes
/plugin install project-roles@test-marketplace

# Restart Claude Code if prompted
# Test your changes
```

**Note:** Some changes (like adding new commands) require a Claude Code restart to take effect.

### Validating Plugin Structure

Essential checks when modifying plugins:
- `plugin.json` must have: name, version, description, author
- Command files must be `.md` format with optional YAML frontmatter
- Command frontmatter should include `description:` field (shown in `/help`)
- Skill files must have YAML frontmatter with `name:` and `description:`
- Skills are model-invoked (automatic); commands are user-invoked (manual)
- Subagent files must have YAML frontmatter with `name:`, `description:`, and optional `tools:`/`model:`

## Important Design Patterns

### Role Mismatch Detection
The `/linear/work-on-issue` command (work-on-issue.md:40-64) includes logic to detect when the active role doesn't match the task type:
- Analyzes issue title, description, labels, and file paths to determine task type
- If Backend Engineer role is active but issue is frontend work, warns the user
- Allows Fullstack/Tech Lead roles to work on any task type
- This is implemented as conditional logic in the command prompt, not as a separate agent

### Convention Loading Strategy
Skills use a consistent pattern:
1. Use Glob to find all rule files in relevant directory
2. Read each file to load complete context
3. Apply conventions during implementation

### Linear Issue Workflow
Standard workflow enforced by `/linear/work-on-issue`:
1. Load issue and project context
2. Verify role matches task type
3. Update issue to "In Progress"
4. Implement following role conventions
5. Test implementation
6. Update issue to "Done" with detailed comment including file references

## Plugin Installation URLs

Production installation uses GitHub URLs:
```bash
# project-roles
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow
/plugin install project-roles@RasmusGodske

# rules-boilerplate
/plugin install rules-boilerplate@RasmusGodske
```

## Critical Implementation Details

### File References Format
When updating Linear issues, always use: `path/to/file.php:123` (path with line number)

### Project Context Structure
Linear projects should include:
- **Branch** - Git branch for the work
- **Purpose** - Why the feature exists
- **Technical Approach** - Overall architecture
- **Dependencies** - Related systems

### Skill Activation
Skills have `description:` field that tells Claude when to activate them. Example:
```yaml
---
name: backend-developer
description: Skill for PHP/Laravel backend development. Use when creating or editing PHP code, models, services, controllers, tests, or any backend logic.
---
```

## Target Technology Stack

Both plugins are optimized for:
- **Backend:** PHP 8+, Laravel 10+
- **Frontend:** Vue 3, TypeScript, Composition API
- **Data:** Spatie Laravel Data package
- **Project Management:** Linear (via MCP server)
- **Testing:** PHPUnit, Pest (Laravel testing)

## Extension Points

### Adding New Role Commands
1. Create `project-roles/commands/roles/new-role.md`
2. Add YAML frontmatter with `description:` field
3. Define role focus, mindset, and approach in the prompt body
4. Specify which `.claude/rules/` directories to load from user's project
5. Use Glob and Read to load conventions: "Use Glob to find all files in `.claude/rules/backend/*.md`"

### Adding New Linear Commands
1. Create `project-roles/commands/linear/new-command.md`
2. Add YAML frontmatter with `description:` field
3. Define workflow using Linear MCP server tools (`mcp__linear-server__*`)
4. Follow pattern: fetch context → update status → execute → update status
5. Include error handling and user confirmations

### Adding New Skills
1. Create `project-roles/skills/new-skill/` directory
2. Create `SKILL.md` with YAML frontmatter containing:
   - `name:` - Skill identifier (lowercase-with-hyphens)
   - `description:` - When Claude should activate this skill (be specific!)
3. In prompt body, define:
   - What conventions to load from user's `.claude/rules/`
   - When to activate the skill
   - What the skill provides (context, not implementation details)

### Adding Subagents (Future)
This repository currently has no subagents, but you could add them:
1. Create `project-roles/agents/` directory
2. Create `agent-name.md` with YAML frontmatter:
   - `name:` - Agent identifier
   - `description:` - When to delegate to this agent
   - `tools:` - Optional comma-separated list (inherits all if omitted)
   - `model:` - Optional model alias or 'inherit'
3. Define agent's system prompt in body

## Understanding Plugin vs User Context

When users install these plugins, they experience:

**User types:** `/roles/backend-engineer`
**Claude Code:**
1. Expands the prompt from `project-roles/commands/roles/backend-engineer.md`
2. Executes the expanded prompt in main conversation
3. Prompt instructs Claude to load files from user's `.claude/rules/backend/`
4. Claude adopts "Backend Engineer" mindset for subsequent work

**Claude autonomously decides to use skill:**
**Claude Code:**
1. Detects task matches `backend-developer` skill description
2. Expands the prompt from `project-roles/skills/backend-developer/SKILL.md`
3. Skill prompt instructs Claude to load user's `.claude/rules/backend/` files
4. Claude applies loaded conventions to current task

The key insight: Commands and skills are **prompt templates** that get expanded and executed. They instruct Claude to read from the **user's** `.claude/rules/` directory, which is why the `rules-boilerplate` plugin exists - to create that structure for users.
