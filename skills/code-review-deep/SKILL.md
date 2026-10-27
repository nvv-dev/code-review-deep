---
name: code-review-deep
description: Use when asked to review a pull request, a diff or a branch for correctness — bugs, breaking changes, security problems, half-finished features. Reviews only what changed, investigates every suspicion before reporting it, and gives each finding a severity, a location and a verdict. For structure and long-term health, use code-review-maintainability instead.
---

# Deep code review

Find the problems that will hurt users or the next developer. Fewer, verified findings beat a long list of maybes.

## Scope

- Review **only the changed code** and the code it directly affects (callers, callees, contracts, migrations). Pre-existing problems are out of scope unless the change makes them worse; mention at most one, separately.
- Finish your own independent pass **before** reading existing PR comments or the author's description of what they tested. Then compare.

## What to look for

1. **Correctness:** wrong logic, off-by-one, null and empty cases, error paths, concurrency, time zones, money and rounding, encoding.
2. **Breaking changes:** public APIs, database schemas and migrations, serialized formats, config keys, CLI flags, events. Is there a migration path?
3. **Security:** input validation, authorization on every new entry point, injection, secrets in code or logs, unsafe deserialization, data from one tenant reachable by another, untrusted content reaching an LLM prompt or a shell.
4. **Half-activated features:** flags that default to on in production, debug or test switches reachable in release builds, test credentials or test IDs in production paths, endpoints wired but unguarded.
5. **Tests:** do they exercise the new behavior through its public boundary? Were any tests weakened, skipped, or timeouts widened? Report each by name.
6. **Operational impact:** performance on realistic data sizes, N+1 queries, unbounded loops or payloads, missing timeouts, log volume.

## Investigate before reporting

Do not report a suspicion you have not finished checking. Read the caller, run the test, grep the usages. If you cannot settle it, it goes under "needs validation", not "confirmed".

## Findings

Each finding has:

- **Verdict:** `confirmed` (you have evidence), `needs validation` (plausible, evidence missing, say what would settle it) or `rejected` (you checked and it is fine — list these briefly so nobody re-checks them).
- **Severity:** `critical` (data loss, security hole, outage), `high` (wrong behavior users will hit), `medium` (edge case or degraded behavior), `low` (minor, safe to defer).
- **Location:** `path/to/file.ext:line`.
- **What happens:** the concrete failure scenario, with inputs.
- **Fix:** the smallest correct change. Show it; do not rewrite unrelated code.

Point out the error, show the correction, and stop. No suggestions outside the scope of the change.

## Decision

- Any `confirmed` critical or high finding → **request changes**.
- Only medium/low or `needs validation` items → **approve with comments**, listing what to validate.
- Nothing found → **approve**, and say what you checked (the coverage), so the reader knows what "nothing found" means.

## Known failures

_Record real mistakes here with a date and the rule that prevents them._
