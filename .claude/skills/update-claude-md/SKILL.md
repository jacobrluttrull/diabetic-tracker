---
name: update-claude-md
description: Update the project's CLAUDE.md file to reflect current state. Use this skill when new features are added, project structure changes, skills are created or modified, dependencies change, build commands change, or anything else that should be documented in CLAUDE.md. Also trigger when the user says "update claude.md", "sync the docs", or "the readme is stale".
---

# Update CLAUDE.md

Keep the project's CLAUDE.md accurate and concise as the project evolves.

## When to Update

- New packages or files added to the project structure
- Skills created, modified, or removed from `.claude/skills/`
- Dependencies added or removed (new entries in go.mod, etc.)
- Build or run commands change
- New features implemented that change what the app does
- Testing approach changes

## How to Update

1. **Read the current CLAUDE.md** to understand what's already documented
2. **Scan what changed** — look at the actual codebase, not assumptions. Check:
   - Project structure (new directories, renamed files)
   - go.mod for dependency changes
   - `.claude/skills/` for skill changes
   - New or changed functionality in source files
3. **Edit only what's relevant** — don't rewrite sections that haven't changed
4. **Keep it under 200 lines** — this is a hard limit. CLAUDE.md is loaded into context every conversation, so bloat costs real money and attention. If approaching the limit:
   - Cut verbose descriptions down to one-liners
   - Remove obvious information (don't document what the code already says clearly)
   - Collapse similar items into grouped bullet points
   - Move detailed reference material into separate docs if needed and link to them

## What Belongs in CLAUDE.md

- Project name and one-line description
- What the app does (feature list, brief)
- Stack and key dependencies
- Project structure (tree view of main directories)
- How to build and run
- How to test
- Custom skills (name + one-line description each)
- Any conventions or patterns the project follows

## What Does NOT Belong in CLAUDE.md

- Full API documentation — put in separate docs
- Step-by-step tutorials — that's README territory
- Implementation details that change frequently
- Anything a developer can figure out by reading the code directly

## Offloading to Reference Files

When content is useful for context but too detailed for CLAUDE.md, move it to a reference file and link to it. This keeps CLAUDE.md lean while still making information discoverable.

**Where to put reference files:** `docs/` directory in project root.

**How to link from CLAUDE.md:**
```markdown
See [docs/api-models.md](docs/api-models.md) for full struct definitions.
```

**What to offload:**
- Detailed struct/model field descriptions → `docs/models.md`
- Fyne widget usage patterns and examples → `docs/fyne-patterns.md`
- Dosage calculation logic and formulas → `docs/calculations.md`
- Database schema details → `docs/schema.md`
- Dependency notes (why X was chosen over Y) → `docs/dependencies.md`
- Conventions and style guide details → `docs/conventions.md`

**What stays in CLAUDE.md:** A one-liner summary + the link. Example:
```markdown
## Data Models
Insulin, LogEntry, and Settings structs. See [docs/models.md](docs/models.md) for details.
```

When creating or updating CLAUDE.md, always check if a section is growing past its line budget. If it is, extract the detail into a `docs/` file and replace the section with a summary + link.

## Line Budget Guide

| Section | Target Lines |
|---|---|
| Header + description | ~5 |
| Features | ~10 |
| Stack | ~8 |
| Project structure | ~15-20 |
| Build & run | ~5 |
| Testing | ~5 |
| Skills | ~3 per skill |
| Conventions | ~10-15 |
| **Buffer** | ~20 |
