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

- **tech-debt** — Scans codebase across 6 debt categories, scores with `(Impact + Risk) × (6 − Effort)`, outputs prioritized remediation plan.
- **testing** — Runs `go test ./... -v`, analyzes failures with root cause + fix suggestions, flags slow/flaky tests.
- **update-claude-md** — Syncs this file with current project state. Hard limit: 200 lines.

## Subagent Model Routing

- **Haiku** — Quick lookups, file searches, simple exploration, read-only tasks
- **Sonnet** — General purpose coding, writing code, debugging, reviews
- **Opus** — Complex architecture decisions, tricky bugs, multi-file refactors
