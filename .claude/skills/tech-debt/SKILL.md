---
name: tech-debt
description: Systematically identify, categorize, and prioritize technical debt in a codebase. Use this skill when the user asks about tech debt, code quality issues, wants a codebase audit, mentions "what should we clean up", asks about code smells, refactoring priorities, or wants to understand what's holding the project back. Also trigger when the user mentions outdated dependencies, missing tests, poor abstractions, or wants a remediation plan.
---

# Tech Debt Management

Scan the codebase, identify technical debt across six categories, score each item, and produce a prioritized remediation plan.

## How This Works

You're looking for real, concrete problems — not hypothetical ones. Read the actual code, check the actual dependencies, look at the actual test coverage. Every item you flag should point to a specific file or pattern, not a vague concern.

## Step 1: Scan the Codebase

Go through each debt category below and actively look for issues. Use Grep, Glob, and Read to examine the code — don't guess.

### Categories to Check

| Category | What to Look For |
|---|---|
| **Code debt** | Duplicated logic across files, magic numbers/strings, overly long functions (50+ lines), deeply nested conditionals, poor naming, dead code, TODO/FIXME/HACK comments |
| **Architecture debt** | God files that do too much, circular dependencies, business logic in the wrong layer, tight coupling between packages, missing interfaces where they'd help |
| **Test debt** | Missing test files, low coverage areas, no integration tests, test files that only test the happy path, flaky patterns (sleep-based waits, time-dependent assertions) |
| **Dependency debt** | Outdated packages (check go.mod, package.json, requirements.txt, etc.), unmaintained libraries (no commits in 12+ months), dependencies with known CVEs, pinned to old major versions |
| **Documentation debt** | Missing or outdated README, no inline docs on exported functions/types, missing API docs, stale comments that describe old behavior, no setup/contributing guide |
| **Infrastructure debt** | No CI/CD config, manual deployment steps, missing linter config, no pre-commit hooks, no Dockerfile when one would help, missing .gitignore entries |

### Scanning Approach

1. Start with the project root — check for config files, README, CI configs, Dockerfile, .gitignore
2. Check dependency files for outdated or problematic packages
3. Read through the main source directories, focusing on the largest files first (size often correlates with debt)
4. Look for test directories/files — note what's missing, not just what's there
5. Search for TODO, FIXME, HACK, XXX comments
6. Check for duplicated patterns across files

## Step 2: Score Each Item

For every concrete issue found, assign three scores:

- **Impact** (1–5): How much does this slow down development or cause problems day-to-day?
- **Risk** (1–5): What's the worst that happens if this stays unfixed? Security vuln = 5, mild annoyance = 1
- **Effort** (1–5): How hard is the fix? Quick rename = 1, major refactor = 5

Calculate priority:

```
Priority = (Impact + Risk) × (6 − Effort)
```

This naturally bubbles up high-impact, low-effort fixes — the quick wins you should tackle first.

## Step 3: Produce the Report

### Report Structure

Use this exact format:

```markdown
# Tech Debt Audit — [Project Name]

**Date:** [today's date]
**Files scanned:** [count]
**Issues found:** [count]

## Summary

[2-3 sentences: overall health, biggest concern, top recommendation]

## Priority Queue

| # | Issue | Category | Impact | Risk | Effort | Score | Location |
|---|-------|----------|--------|------|--------|-------|----------|
| 1 | ...   | ...      | ...    | ...  | ...    | ...   | ...      |

## Detailed Findings

### [Category Name]

#### [Issue Title]
- **Location:** `path/to/file.go:42`
- **What:** [concrete description of the problem]
- **Why it matters:** [business/development impact]
- **Suggested fix:** [brief, actionable recommendation]
- **Effort estimate:** [rough time — hours/days]

## Remediation Plan

### Phase 1: Quick Wins (< 1 day each)
- [ ] ...

### Phase 2: Medium Effort (1-3 days each)
- [ ] ...

### Phase 3: Larger Refactors (3+ days)
- [ ] ...
```

## Important Notes

- Only flag things you actually found in the code — no generic advice
- Every issue needs a specific file path or pattern reference
- The remediation plan should be realistic alongside feature work — don't suggest stopping everything to pay down debt
- If the codebase is small or new, it's fine to report "low debt" — don't manufacture problems
- When in doubt about severity, score conservatively — crying wolf on tech debt erodes trust
