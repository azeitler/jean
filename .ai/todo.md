# Task: Fix Web Access background-investigation crash

- [x] Trace the crash to the background-investigation cache listener.
- [x] Limit the listener to real query data updates.
- [x] Keep remote worktree and investigation IDs owner-aware.
- [x] Show expandable, copyable crash details in production.
- [x] Run focused verification.

## Review

- The query cache emits observer lifecycle notifications as well as data
  updates. Treating all events as readiness changes could cause a render loop
  in Web Access.
- The listener now reacts only to `updated` events for worktree queries.
- The app-level crash screen now exposes the actual error and stack trace so a
  Web Access user can copy the failure details.

- Verification: TypeScript, ESLint, diff checks, and 28 focused tests passed.
