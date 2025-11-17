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

### Step 3: Analyze Why It Wasn't Clear

Be honest about gaps in the current plugin documentation. Consider:

- **Missing entirely?** - The rule/instruction doesn't exist at all
- **Too buried/vague?** - Present but easy to overlook or unclear
- **Not emphatic enough?** - Mentioned but not stressed as critical/required
- **Contradictory?** - Different parts of the documentation conflict
- **No examples?** - Abstract instructions without concrete demonstrations
- **Abstract language?** - Vague terms instead of specific, actionable steps
- **No visual patterns?** - Nothing to "scan for" or check against
- **Missing negative examples?** - No "DON'T do this" warnings

### Step 4: Identify the Source

Which plugin files should have prevented this mistake?

Use the repository structure shown above to identify the likely files. Based on what component was involved:

- **If a role command:** `project-roles/commands/roles/[role-name].md` (e.g., `backend-engineer.md`)
- **If a Linear command:** `project-roles/commands/linear/[command-name].md` (e.g., `work-on-issue.md`)
- **If a skill activated:** `project-roles/skills/[skill-name]/SKILL.md` (e.g., `backend-developer/SKILL.md`)
- **If an agent should have been used:** `project-roles/agents/[agent-name].md` (e.g., `backend-reviewer.md`)

**Be honest if you're not certain.** It's okay to say "likely `backend-engineer.md` based on the role that was active" rather than claiming certainty you don't have.

### Step 5: Propose Specific Improvements

**IMPORTANT: Focus on the SINGLE MOST IMPACTFUL change.**

Don't propose 3-5 improvements. Identify the ONE change that would have prevented this specific mistake. You can mention other potential improvements briefly, but provide detailed markdown for only ONE primary change.

For the primary improvement, specify:

**Location:** `project-roles/path/to/file.md`

**Change:** Show the actual markdown/text to add or modify (keep it focused - 50-150 words)

**IMPORTANT - Sanitize your examples:**
- Use generic task names ("task-1", "the task", "feature implementation")
- NOT company-specific IDs (RAS-60, PROJ-123, etc.)
- Use generic code examples without proprietary business logic

```markdown
## CRITICAL: [Requirement Name]

**BEFORE [action], you MUST:**
1. [Specific step]
2. [Specific step]

❌ WRONG: [Show what not to do - use GENERIC examples]
✅ RIGHT: [Show what to do - use GENERIC examples]
```

**Why:** Explain how this prevents the mistake from happening again (2-3 sentences)

**Types of improvements to consider:**
- More explicit language with examples
- Visual patterns ("Scan for X in your code")
- Prominent negative examples (show WRONG way first)
- Add to checklists
- Add emphasis (bold, ALL CAPS, emojis for critical points)
- Repetition at key decision points
- Concrete before/after code examples
- Hard checkpoints that block forward progress

### Step 6: Sanitize for Public GitHub Issue

**CRITICAL:** Remove all sensitive information before creating the issue.

**What to REMOVE:**
- **Issue tracker IDs** - Linear, Jira, GitHub issue numbers (RAS-60, PROJ-123, #456 → "the task", "the feature")
- **Company or project names** - Replace with "the company", "the project"
- **Specific file paths** - User's project paths (`/home/company/app/Models/User.php` → "a Laravel model file")
- **Business logic details** - Replace with generic equivalents
- **Customer/user data** - Names, identifiers, sensitive data
- **Internal URLs** - Infrastructure details, environment info
- **Proprietary code** - Replace with generic examples
- **Team member names** - Remove or use "the developer"
- **Database/table names** - Replace with generic equivalents ("users table" → "a database table")

**What to KEEP:**
- Plugin file references (`project-roles/skills/backend-developer/SKILL.md`)
- Generic pattern descriptions ("implementing a CRUD endpoint")
- Abstract code examples (generic Laravel/Vue patterns)
- Technology stack mentions (Laravel, Vue, PHP, TypeScript, etc.)
- The type of task being performed (backend, frontend, testing, etc.)

**Sanitization examples:**

❌ Before: "After completing RAS-60 implementation..."
✅ After: "After completing the implementation..."

❌ Before: "Should we move to RAS-61?"
✅ After: "Should we move to the next task?"

❌ Before: "When implementing the UserSubscriptionService for Acme Corp's billing system..."
✅ After: "When implementing a service class with database interactions..."

❌ Before: "File at /home/acme/app/Services/Billing/UserSubscriptionService.php"
✅ After: "A Laravel service class"

❌ Before: "The customer_stripe_id field in our payments table"
✅ After: "A foreign key field in a database table"

**VALIDATION - Before finalizing, scan your draft for:**
- Issue IDs with patterns like: XXX-123, PROJ-456, #789, ticket-123
- Company names in file paths or class names
- Specific database/table/field names
- Any references to Linear, Jira, or other project management tools with task IDs

### Step 7: Draft the GitHub Issue

Create a sanitized issue following this structure. **Keep it concise** - aim for under 1000 words total.

**Include what you experienced** - this is valuable context about the current state of the documentation.

Use this exact markdown template:

```markdown
## What Convention/Pattern Was Missed?

[2-3 sentences: What should have been followed but wasn't]

## What I Saw When Invoked

[2-4 sentences: What instructions/checklist you received when the command/skill was invoked. If you don't remember seeing anything about this requirement, say that clearly.]

## Task Context (Generic)

[1-2 sentences: Generic description of the task type - no company specifics]

## Why Wasn't It Clear?

[2-4 sentences: Analysis of the documentation gap based on what you experienced]

## Which Documentation Files Need Updates

- `project-roles/path/to/file.md`

## Proposed Improvement

**File:** `project-roles/path/to/file.md`

**Change:**
```markdown
[Your proposed markdown text to add/modify - keep focused, 50-150 words]
```

**Why:** [2-3 sentences: How this prevents future mistakes]

## Additional Context

[1-2 sentences: Any other relevant observations, or note if this happens frequently]

---

**Component:** [Role Command / Linear Command / Skill / Agent] - [specific name]
**Plugin:** project-roles
```

**Length Guidelines:**
- What I Saw When Invoked: 2-4 sentences
- Proposed Improvement: 50-150 words of markdown
- Other sections: 1-4 sentences each
- Target: 500-1000 words total

### Step 8: Show Draft for Approval

Display the complete sanitized issue to the user with clear formatting:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DRAFT GITHUB ISSUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title: [Convention] [Brief description]

[Full formatted markdown body from Step 6 here]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Then ask:

**IMPORTANT: Please review this draft carefully for any sensitive information.**

Would you like me to:
1. **Create GitHub issue** - Post to RasmusGodske/dev-agent-workflow using gh CLI
2. **Save to file** - Save the draft locally for manual posting later
3. **Cancel** - Don't create the issue

### Step 9: Create GitHub Issue (if approved)

If the user approves option 1, use the GitHub CLI to create the issue directly:

```bash
gh issue create \
  --repo RasmusGodske/dev-agent-workflow \
  --title "[Convention] [your title]" \
  --body "$(cat <<'EOF'
[Full markdown body from Step 7]
EOF
)" \
  --label "claude-code-feedback"
```

**Important notes:**
- Use a heredoc (`cat <<'EOF' ... EOF`) to properly handle the multi-line body
- The single quotes around `'EOF'` prevent variable expansion
- This ensures markdown formatting is preserved

Then return the issue URL to the user:

```
✅ Issue created successfully!

📍 View it here: https://github.com/RasmusGodske/dev-agent-workflow/issues/[number]

Thank you for helping improve the plugin! This feedback will help make the documentation clearer for everyone.
```

#### Alternative: Save to File (Option 2)

If user chose option 2, save the issue to a local file:

```bash
cat > convention-improvement-draft.md <<'EOF'
# Title: [Convention] [title]

[Full markdown body]
EOF
```

Then tell the user:

```
✅ Draft saved to: convention-improvement-draft.md

You can review this file and manually create the issue at:
https://github.com/RasmusGodske/dev-agent-workflow/issues/new

Thank you for helping improve the plugin!
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