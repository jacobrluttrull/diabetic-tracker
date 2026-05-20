---
name: testing
description: Run tests, analyze failures, and report on test health. Use this skill when the user says "run tests", "check tests", "why is this test failing", "test results", "what broke", "run the suite", or asks about test coverage, flaky tests, or test failures. Also trigger when the user wants to understand test output or debug a failing test.
---

# Testing — Run & Analyze

Run the project's test suite, break down what passed and failed, explain why things broke, and suggest concrete fixes.

## Step 1: Detect the Test Runner

Figure out what kind of project this is and how tests run. Check for these in order:

| Indicator | Runner | Command |
|---|---|---|
| `go.mod` | Go test | `go test ./...` |
| `package.json` with test script | npm/yarn | `npm test` or `yarn test` |
| `pytest.ini`, `pyproject.toml`, `setup.cfg` with pytest config | pytest | `pytest` |
| `requirements.txt` + `test_*.py` files | pytest or unittest | `pytest` or `python -m unittest discover` |
| `Cargo.toml` | Rust | `cargo test` |
| `Makefile` with test target | Make | `make test` |

If unclear, check the README or CLAUDE.md for test instructions. If still unsure, ask.

## Step 2: Run the Tests

Run the full suite with verbose output so you can see individual test names and results:

- **Go:** `go test ./... -v`
- **Python:** `pytest -v`
- **Node:** `npm test -- --verbose` (or whatever the project uses)
- **Rust:** `cargo test -- --nocapture`

If the user asked about a specific file or package, scope the run to just that area instead of the whole suite.

## Step 3: Analyze the Results

### For passing suites
Report a quick summary:
- Total tests run, all green
- How long the suite took
- If any tests are suspiciously slow (>2s for unit tests), flag them
- Note any skipped tests and why

### For failures
For each failing test:

1. **What failed:** Test name and file location
2. **The error:** The actual error message or assertion that didn't hold
3. **Why it likely failed:** Read the test code and the code it's testing. Trace the logic — don't just restate the error message. Common causes:
   - Recent code change broke an assumption the test relied on
   - Test expects old behavior that was intentionally changed
   - Nil/null pointer from missing setup or changed data shape
   - Race condition or timing issue
   - Missing dependency or environment setup
4. **Suggested fix:** Point to the specific line(s) to change — either in the test or the source code, depending on which is actually wrong

### For flaky tests (pass sometimes, fail sometimes)
If the user mentions flakiness or you see intermittent patterns:
- Look for `time.Sleep`, hardcoded timeouts, or time-dependent assertions
- Check for shared state between tests (global variables, shared files, database state)
- Look for goroutine/async races without proper synchronization

## Step 4: Report

Keep it scannable:

```markdown
# Test Results — [project or package name]

**Status:** ✅ All passing | ❌ X failures | ⚠️ X skipped
**Tests run:** [count]
**Duration:** [time]

## Failures

### [TestName] — `path/to/file_test.go:42`
**Error:** [the actual error message]
**Cause:** [your analysis of why]
**Fix:** [what to change and where]

## Warnings
- [slow tests, skipped tests, flaky patterns]
```

## Important Notes

- Always run the tests before analyzing — don't guess at results from reading test code alone
- If tests need a database, docker, or external service to run, tell the user rather than silently failing
- When the fix is in the source code (not the test), say so clearly — a passing test isn't always the goal if the test caught a real bug
- If there are no tests at all, say that plainly and suggest where to start
