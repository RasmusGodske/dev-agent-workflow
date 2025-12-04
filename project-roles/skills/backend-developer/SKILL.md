---
name: backend-developer
description: Skill for PHP/Laravel backend development following project conventions. Use when creating or editing PHP code, models, services, controllers, tests, or any backend logic. Loads all backend rules from .claude/rules/backend/ and .claude/rules/dataclasses/.
---

# Backend Developer Skill

Use this skill when working with backend code to ensure project conventions are followed.

## Loading Conventions

**CRITICAL:** Before implementing any backend features, read ALL backend rules:

**Step 1 - Techstack rules (required):**
1. Use Glob to find all files: `.claude/rules/backend/*.md`
2. Read each file to load conventions
3. Also read: `.claude/rules/dataclasses/laravel-data.md`

**Step 2 - Project-specific rules (if exists):**
1. Check if `.claude/project-rules/backend/` directory exists
2. If yes, use Glob to find all files: `.claude/project-rules/backend/*.md`
3. Read each file to load project-specific patterns

These rules contain all patterns, conventions, and best practices for:
- Controller structure and responsibilities
- Data class creation and usage
- Database and model patterns
- PHP best practices
- Testing conventions
- Project-specific patterns (examples, boilerplate, etc.)
- And more...

## Controller Response Types: Inertia vs JSON

**BEFORE implementing a controller endpoint, determine the response type:**

| Use Case | Response Type | Example |
|----------|--------------|---------|
| Dedicated page with own route | **Inertia** | Settings page, Dashboard, Detail view |
| Reusable dialog/modal | **JSON** | Confirmation dialog, Quick-edit modal |
| API consumed by external clients | **JSON** | Public API, webhook endpoints |

### Decision Rule

**Ask: "Does this endpoint render a full page?"**
- ✅ YES → Use Inertia response with Vue page component
- ❌ NO → Use JSON response

❌ WRONG - Using JSON for a dedicated page:
```php
public function show(): JsonResponse
{
    return response()->json(SettingsData::from($settings));
}
```

✅ RIGHT - Using Inertia for a dedicated page:
```php
public function show(): Response
{
    return Inertia::render('Settings/Show', [
        'settings' => SettingsData::from($settings),
    ]);
}
```

## When to Use This Skill

Activate this skill when:
- Implementing backend features (models, services, controllers)
- Writing tests
- Refactoring backend code
- You need to verify backend patterns
- User asks to "follow backend conventions"
- You're in a different role but need backend context temporarily

## What This Skill Provides

After loading the rules, you have complete context for:
- When to create Data classes vs using arrays
- How to structure controllers and services
- Database and migration patterns
- Testing approaches and factory usage
- PHPDoc conventions
- Type safety patterns

## Controller Response Types: Inertia vs JSON

**BEFORE implementing a controller endpoint, determine the response type:**

| Use Case | Response Type | Example |
|----------|--------------|---------|
| Dedicated page with own route | **Inertia** | Settings page, Dashboard, Detail view |
| Reusable dialog/modal | **JSON** | Confirmation dialog, Quick-edit modal |
| API consumed by external clients | **JSON** | Public API, webhook endpoints |

### Decision Rule

**Ask: "Does this endpoint render a full page?"**
- ✅ YES → Use Inertia response with Vue page component
- ❌ NO → Use JSON response

❌ WRONG - Using JSON for a dedicated page:
```php
public function show(): JsonResponse
{
    return response()->json(SettingsData::from($settings));
}
```

✅ RIGHT - Using Inertia for a dedicated page:
```php
public function show(): Response
{
    return Inertia::render('Settings/Show', [
        'settings' => SettingsData::from($settings),
    ]);
}
```

## Integration with Other Skills

This skill works alongside project-specific skills:

- **`laravel-data-writer`**: For detailed Data class patterns
- **`data-objects`**: For DataObject CRUD operations
- **`object-definitions`**: For ObjectDefinition schema operations
- **`multi-tenancy`**: For tenant isolation patterns
- **`php-test-writer`**: For comprehensive test creation

## Key Principle

**Rules are the source of truth.** This skill simply loads them and provides context on when to apply them.

The rules define:
- WHAT the patterns are
- HOW to implement them
- WHAT to avoid

This skill provides:
- WHEN to use which patterns
- Context for applying rules in your current task
