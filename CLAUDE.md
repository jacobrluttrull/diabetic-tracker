# Diabetic Tracker

Personal desktop app for managing diabetes day-to-day.

## What It Does

- Track two insulin types (basal and bolus)
- Log carb intake per meal
- Configure insulin-to-carb ratios (can vary by meal period)
- Dosage calculator: carbs / ratio + blood glucose correction
- Time-stamped entries for everything
- Desktop notification reminders for insulin schedules

## Stack

- **Language:** Go 1.25
- **GUI:** Fyne v2 (desktop GUI toolkit)
- **Storage:** SQLite or Fyne Preferences API
- **Platform:** Windows desktop

## Project Structure

```
diabetic-tracker/
├── cmd/main.go                  # App entry point
├── internal/
│   ├── models/                  # Data structs (insulin, log entry, settings)
│   ├── storage/                 # Persistence layer (SQLite or Fyne prefs)
│   ├── ui/                      # UI screens (dashboard, log, calculator, history, settings)
│   └── notifications/           # Reminder scheduling + desktop notifications
├── go.mod
└── go.sum
```

## Build & Run

```
go run ./cmd/
```

## Testing

```
go test ./... -v
```

## Custom Skills

Located in `.claude/skills/`:

- **tech-debt** — Scans the codebase for technical debt across 6 categories (code, architecture, test, dependency, documentation, infrastructure). Scores each issue using `(Impact + Risk) × (6 − Effort)` and produces a prioritized remediation plan.
- **testing** — Runs the test suite, analyzes failures with root cause explanations, flags slow/flaky/skipped tests, and suggests specific fixes.
- **update-claude-md** — Syncs CLAUDE.md with current project state. Hard limit: 200 lines max.

## Subagent Model Routing

- **Haiku** — Quick lookups, file searches, simple exploration, read-only tasks
- **Sonnet** — General purpose coding, writing code, debugging, reviews
- **Opus** — Complex architecture decisions, tricky bugs, multi-file refactors
