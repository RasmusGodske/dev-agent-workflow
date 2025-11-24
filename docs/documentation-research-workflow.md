# Documentation Research Workflow

This document explains the research-driven documentation system provided by the `project-roles` plugin.

---

## Overview

The documentation research workflow automatically identifies and addresses documentation gaps in your codebase through a two-phase system:

1. **Research Phase** - Automated context gathering and gap detection
2. **Generation Phase** - User-approved documentation creation

---

## How It Works

### The Complete Workflow

```
Developer working
      ↓
Primary Claude needs context about "JWT validation"
      ↓
Primary Claude invokes research-agent subagent
      ↓
Research Agent (separate context):
  - Loads conventions from .claude/rules/documentation/
  - Searches existing docs (docs/INDEX.md, docs/domains/, docs/layers/)
  - Searches codebase (app/Services/, resources/js/, etc.)
  - Creates detailed report → docs/_research/lacking/pending/
  - Returns: "Report at X, key findings: Y, Z"
      ↓
Primary Claude reads key findings and continues work
      ↓
[Later, when developer has time]
Developer: /docs/process-documentation-reports
      ↓
Documentation Agent:
  - Finds pending reports
  - Loads ALL documentation conventions
  - For each report:
    → Verifies findings
    → Creates plan.md
    → Shows plan to developer
    → Gets approval
    → Generates documentation using templates
    → Updates docs/INDEX.md
    → Creates resolution.md
    → Moves report to processed/
      ↓
Documentation is now complete and discoverable
```

---

## Components

### 1. Research Agent

**File:** `project-roles/agents/research-agent.md`

**What it does:**
- Invoked automatically when primary Claude needs context
- Operates in a separate context (subagent)
- Loads minimal conventions (`structure.md`, `file-mapping.md`)
- Searches systematically for documentation and code
- Creates observational reports (not prescriptive)

**Output:**
- **Research report:** `docs/_research/lacking/pending/{timestamp}_{slug}/report.md`

**Report format:**
```markdown
---
topic: Authentication JWT Validation
priority: high|medium|low
created: 2025-11-20T14:30:22Z
requested_for: Implement password reset feature
---

# Research Report: Authentication JWT Validation

## What Was Requested
Why this research was needed

## Where I Looked
Complete list of files checked

## What I Found
Existing docs and code implementation

## Things I'm Unsure About
Honest gaps in understanding

## What Could Be Improved
Observational notes about documentation gaps (NOT code improvements)
```

### 2. Process Documentation Reports Command

**File:** `project-roles/commands/docs/process-documentation-reports.md`

**What it does:**
- User-invoked: `/docs/process-documentation-reports`
- Finds pending reports in `docs/_research/lacking/pending/`
- Loads ALL documentation conventions
- Processes reports sequentially with user approval
- Generates high-quality documentation
- Manages report lifecycle

**Report lifecycle:**
```
pending/
  {timestamp}_{slug}/
    └── report.md
        ↓
in-progress/
  {timestamp}_{slug}/
    ├── report.md
    └── plan.md          (generated: what will be done)
        ↓
processed/
  {timestamp}_{slug}/
    ├── report.md
    ├── plan.md
    └── resolution.md    (generated: what was done)
```

### 3. Documentation Writer Skill

**File:** `project-roles/skills/documentation-writer/SKILL.md`

**What it does:**
- Auto-activates when creating/editing documentation
- Loads all documentation conventions
- Ensures quality and consistency
- Acts as quality gate for all documentation work

**Used by:**
- `/docs/process-documentation-reports` command
- Manual documentation work
- Documentation updates during development

### 4. Setup Commands

**Files:**
- `docs-boilerplate/commands/setup-docs.md` - Creates `docs/` structure
- `rules-boilerplate/commands/setup-rules.md` - Installs conventions

**What they do:**
- Set up documentation directory structure
- Install documentation conventions to `.claude/rules/documentation/`
- Create initial files (README.md, INDEX.md)

---

## Directory Structure

### In User Projects

```
user-project/
├── .claude/
│   └── rules/
│       └── documentation/          # Conventions (from laravel-vue-rules)
│           ├── README.md            # Overview and navigation
│           ├── structure.md         # How docs are organized
│           ├── file-mapping.md      # Code-to-doc mapping
│           ├── templates.md         # Documentation templates
│           ├── writing-style.md     # Style guidelines
│           ├── index-maintenance.md # INDEX.md rules
│           └── common-mistakes.md   # Validation checklist
│
└── docs/                           # Documentation (created by setup-docs)
    ├── README.md                    # Overview
    ├── INDEX.md                     # Master index
    ├── domains/                     # Business domains
    │   └── authentication/
    │       ├── README.md
    │       └── features/
    │           └── login.md
    ├── layers/                      # Technical layers
    │   ├── backend/
    │   │   └── services/
    │   │       └── auth-service.md
    │   └── frontend/
    └── _research/                   # Research artifacts (plugin-managed)
        └── lacking/
            ├── pending/             # Reports waiting to be processed
            ├── in-progress/         # Reports currently being worked on
            └── processed/           # Completed reports with resolutions
```

### In This Repository

```
dev-agent-workflow/
├── project-roles/                   # Plugin: Role-based workflows + docs system
│   ├── agents/
│   │   └── research-agent.md        # Research subagent
│   ├── commands/
│   │   └── docs/
│   │       └── process-documentation-reports.md
│   └── skills/
│       └── documentation-writer/
│           └── SKILL.md
│
├── docs-boilerplate/                # Plugin: Bootstrap docs/ structure
│   └── commands/
│       └── setup-docs.md
│
└── docs/                            # Plugin documentation (this directory)
    └── documentation-research-workflow.md  (this file)
```

---

## Conventions vs Implementation

### Conventions (laravel-vue-rules)

**Location:** `.claude/rules/documentation/`

**Purpose:** Define HOW to write documentation

**Files:**
- `structure.md` - Organization (domains, layers)
- `file-mapping.md` - Where to place docs
- `templates.md` - What to write
- `writing-style.md` - How to write
- `index-maintenance.md` - INDEX.md rules
- `common-mistakes.md` - Validation

**Who uses it:** Both research agent and documentation writers

### Implementation (project-roles)

**Location:** `project-roles/agents/` and `project-roles/commands/`

**Purpose:** Provide TOOLS to generate documentation

**Files:**
- `research-agent.md` - How to research
- `process-documentation-reports.md` - How to process reports
- `documentation-writer/SKILL.md` - Quality gate

**Who uses it:** Claude Code when invoked

---

## User Workflow

### Setup (One Time)

```bash
# Install plugins
/plugin marketplace add https://github.com/RasmusGodske/dev-agent-workflow
/plugin install project-roles@dev-agent-workflow
/plugin install docs-boilerplate@dev-agent-workflow
/plugin install rules-boilerplate@dev-agent-workflow

# Setup conventions and structure
/setup-rules        # Installs .claude/rules/ (includes documentation/)
/setup-docs         # Creates docs/ structure
```

### During Development (Automatic)

```
Developer: "I need to implement password reset"

Claude (internally): "I need context about JWT validation"
  → Invokes research-agent
  → Research agent creates report
  → Returns key findings to Claude

Claude: "Based on the JWT validation flow [context], here's how to implement password reset..."

[Report stored in docs/_research/lacking/pending/ for later processing]
```

### Documentation Maintenance (Periodic)

```bash
# Review and process accumulated gap reports
/docs/process-documentation-reports

# Claude shows:
Found 3 pending documentation reports:
1. 2025-11-20_143022_auth-jwt-validation - Authentication JWT Validation
2. 2025-11-20_150135_payment-flow - Payment Processing Flow
3. 2025-11-20_163408_user-profile - User Profile Management

Process these reports? [yes/no]

# For each report:
# → Shows plan
# → Gets approval
# → Generates docs
# → Updates INDEX.md
```

---

## Benefits

### For Developers

- **Less context explanation** - Research happens automatically
- **Visible gaps** - Know what's undocumented
- **Non-blocking** - Research doesn't slow down development
- **Quality documentation** - Follows consistent conventions

### For Projects

- **Self-documenting** - Gaps are identified automatically
- **Always current** - Documentation generated from actual code
- **Scalable** - Works for small and large projects
- **Maintainable** - Single source of truth for conventions

### For Claude Code

- **Focused context** - Research agent has clean, separate context
- **Better information** - Primary Claude gets exactly what it needs
- **Quality assurance** - Documentation follows project standards

---

## Configuration

### Gitignore Options

**Option 1: Commit research artifacts**
```
# Keep _research/ in git
```
- Pro: Historical record of documentation evolution
- Con: Clutters git history

**Option 2: Ignore research artifacts**
```gitignore
# .gitignore
docs/_research/
```
- Pro: Clean git history
- Con: Lose documentation improvement tracking

### Customizing Conventions

1. Fork https://github.com/RasmusGodske/laravel-vue-rules
2. Modify `documentation/*.md` files
3. Update your `.claude/rules/` to point to your fork

---

## Best Practices

### When to Process Reports

**Good times:**
- End of feature development
- Before code review
- After significant architectural changes
- Weekly documentation maintenance

**Avoid:**
- Processing during active coding (unless blocking)
- Batching too many reports at once

### Quality Over Quantity

- Process 2-3 reports thoroughly > rushing through 10
- Review plans carefully before approving
- Ensure high-quality documentation output

### Regular Maintenance

- Process pending reports weekly
- Review and archive old processed reports (optional)
- Update conventions as project evolves
- Keep INDEX.md current

---

## Troubleshooting

### Research Agent Not Finding Docs

**Problem:** Research agent reports docs don't exist, but they do

**Causes:**
- Docs not in expected locations per `file-mapping.md`
- Docs not in INDEX.md
- File naming doesn't follow conventions

**Solutions:**
- Review `.claude/rules/documentation/file-mapping.md`
- Ensure INDEX.md is up to date
- Check file naming (lowercase-with-hyphens)

### Generated Documentation is Low Quality

**Problem:** Documentation doesn't meet standards

**Causes:**
- Research report lacked detail
- Conventions are vague or incomplete
- Templates need improvement

**Solutions:**
- Enhance convention files in `.claude/rules/documentation/`
- Manually improve generated docs as examples
- Update templates for future documentation

### Too Many Pending Reports

**Problem:** Overwhelming number of reports

**Causes:**
- Active development creating many gaps
- Haven't processed reports recently

**Solutions:**
- Prioritize high-priority reports first
- Process in manageable batches
- Consider if all gaps warrant documentation

---

## Related Documentation

- **Project Conventions:** `.claude/rules/documentation/README.md` (in user projects)
- **Research Agent:** `project-roles/agents/research-agent.md`
- **Process Command:** `project-roles/commands/docs/process-documentation-reports.md`
- **Documentation Writer Skill:** `project-roles/skills/documentation-writer/SKILL.md`

---

## Summary

The documentation research workflow provides:

1. **Automatic context gathering** via research-agent subagent
2. **Gap detection** through systematic searching
3. **Structured reports** for later processing
4. **User-approved generation** of high-quality documentation
5. **Consistent quality** through convention-driven templates

This system ensures documentation grows organically with your codebase while maintaining high quality and consistency.
