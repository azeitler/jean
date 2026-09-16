# Task: Fix background investigation startup

## Plan
- [x] Trace background investigation from UI action through server routing and prompt dispatch.
- [x] Add a failing regression test for multi-instance routing and initial prompt delivery.
- [x] Implement the smallest root-cause fix.
- [x] Run focused tests and `bun run check:all`.
- [x] Record the result and test steps.

## Review
- Root cause: multi-instance routing did not scope the nested worktree in
  `worktree:created` events. The optimistic scoped worktree stayed `pending`, so
  the background hook sometimes never reached prompt dispatch. The behavior
  depended on which transport delivered the event. The investigation command
  result also returned raw session/worktree IDs, which split prompt and stream
  state from the scoped session.
- Fix: make remote worktree lifecycle events owner-aware, including nested
  worktrees, decorate both IDs in the investigation command result, and wake
  the investigation hook directly when the current project's cached worktree
  changes from pending to ready.
- Regression: routing tests failed with raw worktree/session IDs before each
  fix and now cover both the readiness event and command response boundaries.
- Verification: focused Vitest suites pass. `bun run check:all`
  passed typecheck, lint, formatting, Clippy, and all 2,388 frontend tests. Its
  Rust-test phase stopped on an unrelated existing `Project` test initializer
  that is missing `sentry_base_url` in `jean-core/src/projects/commands.rs`.
