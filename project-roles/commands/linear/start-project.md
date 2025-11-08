# Start Linear Project

Plan and create a new Linear project for a feature or initiative.

## Prerequisites

**Recommended:** Activate a role first (e.g., `/roles/techlead`) to load relevant codebase rules.

If you haven't loaded rules yet, read the appropriate ones:
- Backend work: `.claude/rules/backend/*.md` and `.claude/rules/dataclasses/laravel-data.md`
- Frontend work: `.claude/rules/frontend/*.md`
- Full-stack work: Both backend and frontend rules

## Workflow

### 1. Understand the Feature

Ensure you thoroughly understand what needs to be built:
- If they provided a description, confirm your understanding
- If not, ask them to describe the feature/task
- Ask clarifying questions about scope, requirements, and acceptance criteria
- Identify any technical constraints, dependencies, or integration points

### 2. Identify Git Branch

Determine which branch this feature will be developed on:
- Use Bash: `git branch --show-current`
- Ask the user: "I see you're on branch `[branch-name]`. Is this the correct branch for this feature, or would you like to specify a different one?"
- Store the branch name to include in the project description

### 3. Create Linear Project

Create a new Linear project:
- Use `mcp__linear-server__create_project` with comprehensive details
- Use a clear, descriptive name for the project
- Include a detailed description in Markdown format:

```markdown
## Branch
`[branch-name]`

## Purpose
[What problem this solves or what value it provides]

## Scope
[What's included and what's explicitly out of scope]

## Technical Approach
[High-level architecture or approach - reference patterns from rules]

## Dependencies
[Any related systems, APIs, or services]
```

- Set the appropriate team (ask user if unclear)
- Set target date if the user provides one

### 4. Break Down into Issues

Use the **linear-issue-writer** skill to create detailed issues:
- Activate the skill: `Skill tool with command: "linear-issue-writer"`
- Break the feature down into logical, implementable tasks
- **Write issues assuming someone else will implement them** - include full context
- Each issue should contain:
  - Clear title describing what needs to be done
  - Detailed description with background context
  - Specific acceptance criteria
  - Technical guidance (files to modify, patterns to follow, services to use)
  - Links to relevant documentation or related code
  - Any gotchas or important considerations
- Order issues logically (dependencies first)
- Assign appropriate labels and priorities
- **Reference codebase conventions** from the rules (e.g., "Create a Data class per form-data-classes.md")

### 5. Ask About Next Steps

After planning is complete, ask the user:

> "I've created the Linear project '[Project Name]' with [X] issues for implementation. What would you like to do next?"
> - Start implementing now (use `/linear/work-on-issue` to begin)
> - Stop here and continue later (you can resume with `/linear/work-on-issue [issue-id]`)
> - Review the plan first (I can walk through the issues)

Provide them with the Linear project ID/URL for reference.

## Notes

- Plan thoroughly - assume handoff to another person/session
- Write issues with full context, not just task names
- Keep the user informed at each step
- Ask for approval before creating multiple Linear issues
- Reference specific codebase patterns in issues
- Good planning makes implementation smoother for anyone
