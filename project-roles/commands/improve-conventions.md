---
description: Analyze why a convention wasn't followed and create a GitHub issue to improve plugin documentation
argument-hint: [what convention was missed or why something wasn't followed]
---

# Improve Conventions Command

The user has noticed that a convention or pattern wasn't followed in their project and wants to help improve the plugin to prevent future mistakes.

## What the User Observed

$ARGUMENTS

## Important Context: You Are In a User's Project

**You cannot see the plugin source code right now.** The plugin files are installed in Claude Code's plugin directory, not in this project.

However, you can still create a valuable improvement issue based on:
1. What you experienced when commands/skills were invoked (the expanded prompts you saw)
2. What the user is telling you went wrong
3. Your understanding of how the plugin should work

## Plugin Repository Structure

The plugin you're helping improve lives at: **https://github.com/RasmusGodske/dev-agent-workflow**

It has this structure:

```
dev-agent-workflow/
├── project-roles/                    # Main plugin
│   ├── .claude-plugin/
│   │   └── plugin.json               # Plugin metadata
│   ├── commands/
│   │   ├── roles/                    # Role commands
│   │   │   ├── techlead.md
│   │   │   ├── backend-engineer.md
│   │   │   ├── frontend-engineer.md
│   │   │   ├── fullstack-engineer.md
│   │   │   └── agent-engineer.md
│   │   └── linear/                   # Linear workflow commands
│   │       ├── start-project.md
│   │       ├── work-on-issue.md
│   │       └── review-project.md
│   ├── skills/                       # Auto-activating skills
│   │   ├── backend-developer/SKILL.md
│   │   ├── frontend-developer/SKILL.md
│   │   ├── laravel-data-writer/SKILL.md
│   │   ├── php-test-writer/SKILL.md
│   │   └── linear-issue-writer/SKILL.md
│   └── agents/                       # Subagents with separate context
│       ├── backend-reviewer.md
│       └── frontend-reviewer.md
│
└── rules-boilerplate/                # Convention templates plugin
    └── commands/
        └── setup-rules.md
```

**How the plugin works:**
- **Commands** (`/roles/backend-engineer`) - Expand prompts in the main conversation
- **Skills** - Claude auto-activates based on task context
- **Agents** - Subagents with separate context that roles/skills can delegate to

## Your Task

Provide a structured analysis and create a GitHub issue to improve the plugin documentation.

### Step 1: Recall What You Experienced

First, think about what you saw when the relevant command/skill was invoked:
- If a role command was used (`/roles/backend-engineer`), what instructions did you receive?
- If a skill activated, what did the skill prompt tell you to do?
- What checklist or workflow steps were provided?

**Be specific about what you remember seeing.** This is valuable data about what the current documentation says.

### Step 2: Acknowledge What Was Missed

Based on what the user observed and what you remember from the expanded prompts, clearly state what convention/pattern should have been followed but wasn't.

Be specific:
- Which role/skill/agent was involved?
- What should have happened?
- What actually happened instead?
- What did you see in the expanded prompt about this requirement (if anything)?

### Step 2: Analyze Why It Wasn't Clear

Be honest about gaps in the current plugin documentation. Consider:

- **Missing entirely?** - The rule/instruction doesn't exist at all
- **Too buried/vague?** - Present but easy to overlook or unclear
- **Not emphatic enough?** - Mentioned but not stressed as critical/required
- **Contradictory?** - Different parts of the documentation conflict
- **No examples?** - Abstract instructions without concrete demonstrations
- **Abstract language?** - Vague terms instead of specific, actionable steps
- **No visual patterns?** - Nothing to "scan for" or check against
- **Missing negative examples?** - No "DON'T do this" warnings

### Step 3: Identify the Source

Which plugin files should have prevented this mistake?

Use the repository structure shown above to identify the likely files. Based on what component was involved:

- **If a role command:** `project-roles/commands/roles/[role-name].md` (e.g., `backend-engineer.md`)
- **If a Linear command:** `project-roles/commands/linear/[command-name].md` (e.g., `work-on-issue.md`)
- **If a skill activated:** `project-roles/skills/[skill-name]/SKILL.md` (e.g., `backend-developer/SKILL.md`)
- **If an agent should have been used:** `project-roles/agents/[agent-name].md` (e.g., `backend-reviewer.md`)

**Be honest if you're not certain.** It's okay to say "likely `backend-engineer.md` based on the role that was active" rather than claiming certainty you don't have.

### Step 4: Propose Specific Improvements

For each file that needs changes, propose improvements based on:
- What you remember seeing (or NOT seeing) in the expanded prompt
- What the user told you went wrong
- What would have prevented this mistake

Specify:

**Location:** `project-roles/path/to/file.md`

**Change:** Show the actual markdown/text to add or modify
```markdown
## CRITICAL: [Requirement Name]

**BEFORE [action], you MUST:**
1. [Specific step]
2. [Specific step]
3. [Specific step]

❌ WRONG: [Show what not to do]
✅ RIGHT: [Show what to do]
```

**Why:** Explain how this prevents the mistake from happening again

**Types of improvements to consider:**
- More explicit language with examples
- Visual patterns ("Scan for X in your code")
- Prominent negative examples (show WRONG way first)
- Add to checklists
- Add emphasis (bold, ALL CAPS, emojis for critical points)
- Repetition in multiple relevant places
- Concrete before/after code examples
- Step-by-step walkthroughs

### Step 5: Sanitize for Public GitHub Issue

**CRITICAL:** Remove all sensitive information before creating the issue.

**What to REMOVE:**
- Company or project names (replace with "the company", "the project")
- Specific file paths from the user's project (`/home/company/secret/app/Models/User.php` → "a Laravel model file")
- Business logic details (replace with generic equivalents)
- Customer/user data, names, or identifiers
- Internal URLs, infrastructure details, or environment info
- Proprietary code snippets (replace with generic examples)
- Team member names

**What to KEEP:**
- Plugin file references (`project-roles/skills/backend-developer/SKILL.md`)
- Generic pattern descriptions ("implementing a CRUD endpoint")
- Abstract code examples (generic Laravel/Vue patterns)
- Technology stack mentions (Laravel, Vue, PHP, TypeScript, etc.)
- The type of task being performed (backend, frontend, testing, etc.)

**Sanitization examples:**

❌ Before: "When implementing the UserSubscriptionService for Acme Corp's billing system..."
✅ After: "When implementing a service class with database interactions..."

❌ Before: "File at /home/acme/app/Services/Billing/UserSubscriptionService.php"
✅ After: "A Laravel service class"

❌ Before: "The customer_stripe_id field in our payments table"
✅ After: "A foreign key field in a database table"

### Step 6: Draft the GitHub Issue

Create a sanitized issue following this structure. **Include what you experienced** - this is valuable context about the current state of the documentation.

```markdown
**Title:** [Convention] [Brief description of what was missed]

**Which plugin?** project-roles

**Component Type:** [Role Command / Linear Command / Skill / Agent / Documentation]

**Specific Component:** [Name of the role/skill/agent]

**What Convention/Pattern Was Missed?**
[Clear description of what should have been followed but wasn't]

**What I Saw When Invoked:**
[What instructions/checklist/workflow you received when the command/skill was invoked. Be specific - this shows the current state of documentation. If you don't remember seeing anything about the requirement, say that clearly.]

**Task Context (Generic):**
[Generic description of the task type - no company specifics]

**Why Wasn't It Clear?**
[Analysis of the documentation gap based on what you experienced]

**Which Documentation Files Need Updates?**
- `project-roles/path/to/file.md` (specify if uncertain: "likely this file based on X")
- `project-roles/path/to/another/file.md`

**Proposed Improvements:**

**File:** `project-roles/path/to/file.md`
**Change:**
```markdown
[Exact text to add/modify - be specific about placement and formatting]
```
**Why:** [How this prevents future mistakes]

[Repeat for each file]

**Additional Context:**
[Any other relevant observations about frequency, similar issues, etc.]
```

### Step 7: Show Draft for Approval

Display the complete sanitized issue to the user with clear formatting:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DRAFT GITHUB ISSUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title: [Convention] [Brief description]

Which plugin? project-roles

Component Type: [Role Command / Linear Command / Skill / Agent]

Specific Component: [name]

What Convention/Pattern Was Missed?
[description]

What I Saw When Invoked:
[what you experienced]

Task Context (Generic):
[generic task description]

Why Wasn't It Clear?
[analysis]

Which Documentation Files Need Updates?
[file paths]

Proposed Improvements:
[specific changes with examples]

Additional Context:
[other observations]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Then ask:

**IMPORTANT: Please review this draft carefully for any sensitive information.**

Would you like me to:
1. **Open pre-filled form in browser** - Opens GitHub with all fields populated for final review
2. **Show URL only** - Display the URL so you can open it manually
3. **Cancel** - Don't create the issue

### Step 8: Open Pre-Filled GitHub Issue Form (if approved)

GitHub's YAML issue forms can be pre-populated using URL query parameters. Construct a URL and open it in the browser for final review and submission.

#### Build the URL

Use Python to construct the URL with proper encoding:

```python
import urllib.parse

# Map your sanitized data to form field IDs
params = {
    'template': 'convention-improvement.yml',
    'title': '[Convention] [your title]',
    'plugin': 'project-roles',
    'component-type': '[component type value]',
    'component-name': '[component name]',
    'what-was-missed': '[sanitized text]',
    'what-was-seen': '[what you experienced]',
    'task-context': '[generic context]',
    'why-unclear': '[analysis]',
    'affected-files': '[file paths]',
    'proposed-improvements': '[improvements with markdown]',
    'additional-context': '[observations]'
}

base_url = 'https://github.com/RasmusGodske/dev-agent-workflow/issues/new'
query_string = urllib.parse.urlencode(params)
full_url = f'{base_url}?{query_string}'
```

#### Open in Browser

If user chose option 1, open the URL in the default browser:

```bash
xdg-open "[URL]"  # Linux
# or
open "[URL]"      # macOS
# or
start "[URL]"     # Windows
```

Then tell the user:

```
✅ Opening pre-filled issue form in your browser...

The GitHub issue form is now open with all fields pre-populated.

Please:
1. Review all fields one final time for sensitive information
2. Make any last edits if needed
3. Click "Submit new issue" when ready

The issue will be created in: RasmusGodske/dev-agent-workflow

Thank you for helping improve the plugin!
```

#### Show URL Only

If user chose option 2, display the URL:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pre-filled issue form URL:

[full URL here]

Copy and paste this URL into your browser to review and submit.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Key Principles

1. **Be Honest** - Acknowledge documentation gaps without being defensive
2. **Be Specific** - Propose exact text changes, not vague suggestions
3. **Be Practical** - Focus on changes that actually prevent mistakes
4. **Be Actionable** - Ready to create the issue immediately
5. **Protect Privacy** - Aggressively sanitize any company-specific information

## Remember

This is a **feedback loop** for improving the plugin based on real mistakes in real projects. The goal is to make conventions so clear and prominent that they're hard to miss.

Your analysis should be thorough, your sanitization should be paranoid, and your proposed improvements should be concrete and implementable.