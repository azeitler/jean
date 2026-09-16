# Fix Web Access live-session history hydration

- [x] Trace native-to-Web Access session open and streaming data flow.
- [x] Add a failing regression test for loading persisted history before live chunks.
- [x] Implement the smallest root-cause fix.
- [x] Run focused tests and `bun run check:all`.
- [x] Record verification and review results.

## Review

- Root cause: Web Access can receive live chunks before `get_session` finishes.
  `ChatWindow` then saw live state and skipped the persisted running snapshot,
  so output produced before the browser opened was absent.
- Fix: always merge the persisted running snapshot into live stream state. The
  existing merge and replay-deduplication logic keeps overlapping output unique.
- Regression: the new assertion failed with the old early return and passes now.
- Verification: 31 focused tests passed. TypeScript typecheck, ESLint, Rust
  formatting, Clippy, and all 2,390 frontend tests passed through
  `bun run check:all`. The existing Rust test compile error remains:
  `Project` is missing `sentry_base_url` in
  `jean-core/src/projects/commands.rs:15299`.
- Live UI verification was not available because Jean reported no running
  environment for this repository.
