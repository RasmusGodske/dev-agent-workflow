# Linear Issue Writer Skill

## Core Principles

### 1. Anyone Should Pick It Up
Write issues so any team member can understand and execute without asking the original author.

### 2. Capture "Why" Over "What"
Context and rationale are irreplaceable. Implementation details can be figured out; context cannot be reconstructed.

### 3. Make Risks Explicit
Call out breaking changes, migrations, and pitfalls upfront.

---

## ⚠️ What NOT to Include in Issues

### Don't Teach Code Conventions

Issues are for breaking down FEATURES, not teaching developers how to code.

❌ **WRONG - Teaching conventions:**
> "Create a Data class. Remember to:
> - Import all classes at the top
> - Use named arguments
> - Add PHPDoc type annotations
> - Follow the backend-developer skill patterns"

✅ **CORRECT - Feature-focused:**
> "Create `ObjectDefinitionStatisticsData` to structure the response with `totalDataObjects` and `dataObjectsWithErrors` fields. Add `#[TypeScript()]` for frontend type generation."

### Assume Developer Competence

Developers on your team:
- ✅ Already know project code conventions
- ✅ Have access to skills (backend-developer, php-test-writer, etc.)
- ✅ Can read existing code for patterns

**Your job:** Break down the FEATURE into tasks, not teach programming basics.

### Code Examples Should Illustrate the Feature

Use code examples to show:
- ✅ What the feature looks like when complete
- ✅ How components interact
- ✅ Complex logic or algorithms

Don't use code examples to:
- ❌ Show how to import classes
- ❌ Demonstrate basic PHP syntax
- ❌ Repeat convention checklists

### Example: Good vs Bad Issue

**❌ BAD ISSUE - Too much convention detail:**
```markdown
## Backend: Add show() method

Create the show() method in ObjectDefinitionController.

**Important conventions to follow:**
- Import all classes at the top (no \Fully\Qualified\Names)
- Use named arguments where parameters aren't obvious
- Add type hints and return types
- Include PHPDoc with Collection types

**Example:**
```php
use App\Models\ObjectDefinition; // Import at top!

public function show(ObjectDefinition $objectDefinition): Response // Type hints!
{
    // Use named arguments!
    return Inertia::render(
        component: 'App/ObjectDefinitions/Show',
        props: ['objectDefinition' => $objectDefinition]
    );
}
```
```

**✅ GOOD ISSUE - Feature-focused:**
```markdown
## Backend: Add show() method

Create `show()` method in `ObjectDefinitionController` that returns:
- ObjectDefinition with columns
- Statistics (total DataObjects, error count)
- Failed operations count
- Current reprocess operation (if active)
- Usage overview (CommissionPlans/Triggers grouped)

Render to Inertia component: `App/ObjectDefinitions/Show`

**Files to modify:**
- `app/Http/Controllers/App/ObjectDefinitionController.php`

**Dependencies:** RAS-23 (Data classes)
```

The good version focuses on WHAT to build, not HOW to write PHP.

---

## Parent Issue: Key Sections

### 1. Problem Statement
**What's wrong and why it matters.**

```markdown
## Problem Statement
- Current system uses loose strings → hard to refactor
- No type safety → errors caught at runtime
- Duplicated logic between frontend/backend

Why This Matters: Wastes dev time, risky to change, tech debt growing.
```

### 2. Solution Design
**Approach and key design decisions.**

```markdown
## Solution Design
Key Changes: Replace strings with enums, create factories, unify data structures

Design Decision: Use factories instead of builders
Why: Single source of truth, prevents drift
Alternative: Separate builders (rejected - can drift out of sync)
Trade-off: Slightly more code, but safer long-term
```

### 3. Breaking Changes
**Call out impacts explicitly.**

```markdown
## ⚠️ Breaking Changes
- API method `oldMethod()` removed → use `newMethod()` instead
- Data structure changed: `data.field` → `nested.data.field`

Migration: Run `artisan migrate:update-structure`
Rollback: Keep old code deprecated for 1 release
```

### 4. Success Criteria
**Define "done" with testable criteria.**

```markdown
## Success Criteria
- [ ] All old usages replaced
- [ ] Tests pass (unit + integration)
- [ ] No regressions in key workflows
- [ ] Documentation updated
```

### 5. Code Example
**Show before/after to visualize improvement.**

```markdown
## Code Example

Before:
$config = ['field' => ['type' => 'string']];  // Loose strings

After:
$config = new FieldConfig(type: FieldType::STRING);  // Type-safe
```

---

## Subtask: Key Elements

Keep subtasks concise and actionable:

```markdown
## Goal
Create UserFactory with validation logic

## Tasks
- [ ] Create `app/Factories/UserFactory.php`
- [ ] Implement validation rules
- [ ] Add tests with edge cases

## Dependencies
- TASK-123: BaseFactory must exist first

## Acceptance Criteria
- [ ] All tests pass
- [ ] Follows coding conventions
- [ ] PHPDoc complete
```

---

## Issue Quality Checklist

Before publishing an issue, scan it and ask:

### ✅ Feature-Focused Questions:
- [ ] Does it describe WHAT to build?
- [ ] Are tasks broken down into logical units?
- [ ] Is the scope clear (what's in/out)?
- [ ] Are dependencies identified?

### ❌ Red Flags (Remove These):
- [ ] Does it teach basic code conventions? (imports, type hints, etc.)
- [ ] Does it include convention checklists from skills?
- [ ] Are code examples showing syntax instead of features?
- [ ] Is it repeating information from backend-developer/php-test-writer skills?

**Rule of thumb:** If you're copy-pasting from skill files, you're probably over-explaining.

---

## Quick Checks

Before publishing an issue, ask:

1. **Can someone else understand this without asking me?**
2. **Are breaking changes and risks explicit?**
3. **Is there a rollback/migration plan?**
4. **Can we measure success?**
5. **Do we explain WHY, not just WHAT?**

---

## Common Mistakes

### ❌ Too Vague
"Update the system" → What system? Why? How to verify?

### ✅ Specific
"Replace string-based config with type-safe enums to prevent runtime errors"

---

### ❌ Missing Context
"Refactor Factory" → Why? What problem does it solve?

### ✅ Includes Context
"Convert static methods to instance methods to enable dependency injection and testing"

---

### ❌ Hidden Breaking Changes
"Update API" → Will this break existing code?

### ✅ Explicit Impact
"⚠️ BREAKING: API signature changed. Migration: Update 15 call sites in controllers."

---

### ❌ Over-Explaining Conventions

**Problem:** Issues that lecture developers on code style instead of describing features.

**Bad - Teaching conventions:**
> "Create the show() method. Make sure to:
> - Import all classes at the top (no \Fully\Qualified\Names)
> - Use named arguments for clarity
> - Add PHPDoc with Collection types
> - Follow all backend-developer patterns"

**Good - Describes the feature:**
> "Create `show()` method that returns ObjectDefinition details, statistics, and usage overview to the Show.vue component."

**Why it matters:** Developers already know conventions. Issues should focus on WHAT to build, not HOW to code.
