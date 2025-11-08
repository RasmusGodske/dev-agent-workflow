# Review Linear Project

Get a comprehensive briefing on a Linear project's status, progress, and remaining work.

## Usage

`/linear/review-project <project-name-or-id>`

## How Your Role Affects the Review

Your active role determines your focus:
- **Tech Lead**: Architecture, project health, technical decisions, task breakdown quality
- **Backend Engineer**: Backend implementation quality, patterns used, technical debt
- **Frontend Engineer**: Frontend implementation, component quality, UX decisions
- **Fullstack Engineer**: Integration between layers, end-to-end coherence
- **No role active**: General overview of project status

## Workflow

### 1. Load Complete Project Context

Gather all available information:

**Project Details:**
- Use `mcp__linear-server__get_project` to fetch project information
- Read the full project description and documentation
- **Extract branch information** from the description (look for "## Branch" section)

**All Issues (Complete History):**
- Use `mcp__linear-server__list_issues` with:
  - Project filter
  - `includeArchived: true`
  - NO status filter (get everything: Done, In Progress, To Do, Canceled, etc.)
  - Sort by `createdAt` to understand chronological order

**Issue Comments (Implementation Details):**
- Use `mcp__linear-server__list_comments` for issues marked as "Done"
- Look for implementation summaries, concerns, and technical decisions
- Identify patterns in the implementation approach

**Related Documentation:**
- Use `mcp__linear-server__list_documents` filtered by project
- Read key documents that provide additional context

### 2. Analyze the Project

Synthesize all information to understand:
- **Overall goal and purpose** of the feature
- **Technical approach** and architecture decisions
- **Progress made** - what's been completed and how
- **Current state** - what's in progress, blocked, or waiting
- **Remaining work** - what still needs to be done
- **Recent activity** - what was worked on most recently

### 3. Provide Comprehensive Briefing

Present your findings in this structured format:

```markdown
# Project Review: [Project Name]

**🌿 Branch:** `[branch-name]` (or "Not specified" if missing)

## Overview
[2-3 paragraphs explaining what this feature is, why it exists, and what problem it solves. Include the technical approach and key architectural decisions.]

## Current Status
**Progress:** [X] of [Y] issues complete ([Z]%)
**State:** [e.g., "Active development" / "Blocked" / "Nearly complete" / "Stalled"]
**Last activity:** [Date and brief description of most recent work]

## What Has Been Done ✅

[List completed work with brief descriptions. Group related items if it makes sense.]

1. **[Completed Feature/Task]**
   - Implementation: [Brief technical summary from comments]
   - Files involved: [Key files mentioned]
   - Notes: [Any concerns or decisions mentioned in implementation]

2. **[Next completed item]**
   - ...

## What Needs To Be Done 📋

[List remaining work in priority/logical order]

1. **[Task name]** (Status: [To Do/In Progress/Blocked])
   - Purpose: [What this accomplishes]
   - Depends on: [Any dependencies, if mentioned]

2. **[Next task]**
   - ...

## Last Thing We Worked On 🔄

**Issue:** [Issue title and ID]
**Status:** [Current status]
**What was done:**
[Detailed explanation based on comments and status]

**What's next:**
[Logical next step based on project state]

## Technical Considerations

[Highlight any important technical details, concerns raised in comments, patterns established, or decisions made that someone continuing this work should know about. Focus on aspects relevant to your role.]

## Recommendation

[Your honest assessment: Is this project on track? Are there concerns? What should be prioritized next? Provide perspective based on your role's focus.]
```

### 4. Offer Next Steps

After presenting the review, ask:

> "Would you like me to:
> - Work on a specific issue? (Use `/linear/work-on-issue [issue-id]`)
> - Dive deeper into a specific implementation?
> - Update the project plan based on what I found?"

## Guidelines

- **Be thorough** - read all issues and comments to understand the full picture
- **Be honest** - if something seems incomplete or concerning, mention it
- **Provide context** - explain technical decisions and patterns
- **Show chronology** - help understand the development timeline
- **Identify blockers** - call out anything that might prevent progress
- **Be concise** - provide details but keep it readable
- **Think critically** - analyze what's been done, not just list it
- **Apply your role's lens** - focus on aspects relevant to your expertise
- **Branch handling** - if no branch is specified, note "Branch: Not specified" and suggest updating the project description

## Example Opening

Good opening for your review:
```
I've reviewed the Linear project "[Name]" including all 15 issues (8 completed, 3 in progress, 4 pending) and their implementation comments. Here's a comprehensive briefing on where this project stands:
```

## Notes

- This is a **read-only, analytical command** - no code changes
- Focus on understanding and communicating project state
- Use implementation comments to understand what was actually built, not just planned
- Identify the "story" of the project - how it evolved and where it's heading
- Your active role affects which aspects you emphasize in your analysis
