---
name: frontend-developer
description: Skill for implementing frontend features following project conventions. Use when writing Vue components, creating UI, or refactoring frontend code. Loads all frontend rules from .claude/rules/frontend/.
---

# Frontend Developer Skill

Use this skill when working with frontend code to ensure project conventions are followed.

## Loading Conventions

**CRITICAL:** Before implementing any frontend features, read ALL frontend rules:

**Step 1 - Techstack rules (required):**
1. Use Glob to find all files: `.claude/rules/frontend/*.md`
2. Read each file to load conventions

**Step 2 - Project-specific rules (if exists):**
1. Check if `.claude/project-rules/frontend/` directory exists
2. If yes, use Glob to find all files: `.claude/project-rules/frontend/*.md`
3. Read each file to load project-specific patterns

These rules contain all patterns, conventions, and best practices for:
- Vue component structure
- TypeScript usage
- Component composition patterns
- Styling conventions
- State management
- Project-specific patterns (component examples, etc.)
- And more...

## When to Use This Skill

Activate this skill when:
- Implementing frontend features or UI components
- Refactoring frontend code
- You need to verify frontend patterns
- User asks to "follow frontend conventions"
- You're in a different role but need frontend context temporarily

## What This Skill Provides

After loading the rules, you have complete context for:
- How to structure Vue components
- Which patterns to use (and avoid)
- TypeScript conventions
- Inertia.js patterns (if used)
- Component communication patterns
- Styling approaches

## Key Principle

**Rules are the source of truth.** This skill simply loads them and provides context on when to apply them.

The rules define:
- WHAT the patterns are
- HOW to implement them
- WHAT to avoid

This skill provides:
- WHEN to use which patterns
- Context for applying rules in your current task
