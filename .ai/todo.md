# Show git diff in the session header

- [x] Inspect the responsive session header and existing tests.
- [x] Add a failing test for diff stats at mobile and desktop widths.
- [x] Show uncommitted and branch diff stats beside the title at all widths.
- [x] Run focused tests and quality checks.
- [x] Record the result and manual test steps.

## Review

- Session modal headers now pass uncommitted and branch diff totals to `GitStatusBadges` at mobile and desktop widths.
- The focused responsive-header test passed. TypeScript typecheck, ESLint, Rust formatting, Rust clippy, and all 2,382 frontend tests passed through `bun run check:all`.
- The final Rust test stage is blocked by an unrelated existing test initializer in `jean-core/src/projects/commands.rs` that is missing `Project.sentry_base_url`.
- No Jean run environment was active, so live responsive browser verification was not available.

## Manual test

- Open Web Access on a mobile-width viewport and open a worktree session.
- Confirm the `+added/-removed` diff totals appear directly after the worktree title.
- Repeat at desktop width and select each total to confirm the correct diff opens.
