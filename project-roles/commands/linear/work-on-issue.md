# Work on Linear Issue

Load and implement a specific Linear issue.

## Usage

`/linear/work-on-issue <issue-id>`

Example: `/linear/work-on-issue PRO-123`

## Prerequisites

**Recommended:** Activate an appropriate role first:
- `/roles/backend-engineer` - For backend issues
- `/roles/frontend-engineer` - For frontend issues
- `/roles/fullstack-engineer` - For full-stack issues
- `/roles/techlead` - For planning/architecture issues

If no role is active, you'll work as a general engineer.

## Workflow

### 1. Load Issue Context

**Fetch the Issue:**
- Use `mcp__linear-server__get_issue` with the issue ID
- Read the complete issue description and acceptance criteria
- Note the current status and any existing comments

**Load Project Context (if available):**
- If the issue belongs to a project, use `mcp__linear-server__get_project`
- Read the project description to understand:
  - **Branch** - Which branch this work belongs to
  - **Purpose** - Why this feature exists
  - **Technical Approach** - Overall architecture
  - **Dependencies** - Related systems

**Analyze Issue Type:**
- Determine if this is primarily a backend, frontend, or fullstack task by reading:
  - Issue title and description
  - Technical guidance in the description
  - File paths or components mentioned
  - Labels/tags on the issue

**Verify Role Match:**
- Check if your current role matches the issue type:
  - **Backend Engineer** → Backend task ✓
  - **Frontend Engineer** → Frontend task ✓
  - **Fullstack Engineer** → Any task ✓
  - **Tech Lead** → Any task ✓ (focus on architecture)
  - **No role** → Any task ✓ (general approach)

- If there's a mismatch, warn the user:
  > "⚠️ **Role Mismatch Detected**
  >
  > I'm currently acting as a **[current-role]**, but this issue appears to be primarily **[issue-type]** work based on:
  > - [Reason 1, e.g., "Issue mentions Vue components"]
  > - [Reason 2, e.g., "File paths reference frontend directory"]
  >
  > **Recommendation:** Activate `/roles/[suggested-role]` first for better context and conventions.
  >
  > Would you like me to:
  > - **Proceed anyway** with my current [role] perspective?
  > - **Wait** while you activate the correct role?"

- Wait for user confirmation before proceeding with a mismatched role

**Verify Branch:**
- Get current branch: `git branch --show-current`
- If project specifies a branch and you're on a different one:
  - Warn: "⚠️ This issue is for branch `[project-branch]`, but you're on `[current-branch]`. Would you like me to switch branches or continue on the current branch?"
  - Wait for user decision

### 2. Update Issue to "In Progress"

- Use `mcp__linear-server__update_issue` to set status to "In Progress"
- This signals to the team that work has started

### 3. Implement the Issue

Work on the issue based on your active role:

**Backend Engineer:**
- Follow backend conventions from loaded rules
- Create: Models, Data classes, Services, Controllers, Form Requests, Tests
- Ensure proper validation, testing, and pattern adherence
- Focus on code quality and maintainability

**Frontend Engineer:**
- Follow frontend conventions from loaded rules
- Create: Vue components, composables, TypeScript types
- Check for existing reusable components first
- Ensure proper UX and accessibility
- Focus on component quality and user experience

**Fullstack Engineer:**
- Implement both backend and frontend
- Ensure clean integration between layers
- Design APIs that serve frontend needs
- Test end-to-end functionality

**Tech Lead:**
- Focus on architectural correctness
- Set patterns for others to follow
- Document technical decisions
- Think about long-term maintainability

### 4. Test Your Implementation

**Backend:**
- Write comprehensive tests
- Run tests: `./vendor/bin/sail php artisan test --filter=YourTest`
- Ensure all tests pass

**Frontend:**
- Test manually in the browser
- Check responsive design
- Verify user interactions work correctly

### 5. Update Issue to "Done"

When implementation is complete:

**Update Status:**
- Use `mcp__linear-server__update_issue` to set status to "Done"

**Add Implementation Comment:**
- Use `mcp__linear-server__create_comment` with detailed summary

**Comment Format:**
```markdown
✅ Implementation complete

**Summary:**
[1-3 sentences describing what was implemented and how it works]

**Changes:**
- `app/Services/ReportService.php:45` - Brief description of change
- `app/Data/CreateReportData.php:1` - Brief description of change
- `tests/Feature/ReportExportTest.php:1` - Brief description of change

**Uncertainties/Concerns:**
[Only if applicable - implementation decisions you're unsure about or potential issues]
⚠️ Token refresh interval is set to 15 minutes. May need adjustment for production.

**Testing:**
✅ All tests passing (8 new tests added)
[or]
✅ Tested manually in browser - all interactions working correctly
```

## Notes

- Always update Linear at start (In Progress) and finish (Done)
- Include file references with line numbers: `path/to/file.php:123`
- Be specific about uncertainties so reviewers know what to check
- Keep comments concise but informative
- If you discover additional work needed, ask if you should create a new Linear issue
- Follow conventions from your active role's loaded rules

## Example Sessions

### Example 1: Role Matches Task
```
User: /roles/backend-engineer
[Backend engineer role activated, rules loaded]

User: /linear/work-on-issue PRO-456

You: Loading issue PRO-456: "Add export functionality to reports"

This is a backend task (mentions Services, Controllers, API endpoints).
You're currently acting as Backend Engineer ✓

Branch: feature/report-export (you're on it ✓)

I'll update the issue to "In Progress" and begin implementation.

[Updates Linear, implements, tests]

You: Issue PRO-456 complete! Created ReportExportService, added API endpoint, wrote tests.
Marking as Done in Linear.
```

### Example 2: Role Mismatch (Warning Given)
```
User: /roles/backend-engineer
[Backend engineer role activated]

User: /linear/work-on-issue PRO-789

You: Loading issue PRO-789: "Create export dialog component"

⚠️ **Role Mismatch Detected**

I'm currently acting as a **Backend Engineer**, but this issue appears to be primarily **Frontend** work based on:
- Issue title mentions "dialog component"
- Description references Vue components in resources/js/
- Labels include "frontend"

**Recommendation:** Activate `/roles/frontend-engineer` first for better context and conventions.

Would you like me to:
- **Proceed anyway** with my current Backend Engineer perspective?
- **Wait** while you activate the correct role?

User: Wait

You: Understood. Please activate `/roles/frontend-engineer` and then run `/linear/work-on-issue PRO-789` again.
```

### Example 3: Fullstack or Tech Lead (No Warning)
```
User: /roles/fullstack-engineer

User: /linear/work-on-issue PRO-101

You: Loading issue PRO-101: "Create export dialog component"

This is a frontend task. As a Fullstack Engineer, I can handle this ✓

[Proceeds with implementation]
```
