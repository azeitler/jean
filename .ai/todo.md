# Task: Scope models and favorites to the active Jean server

- [x] Trace model discovery and favorite persistence for local and remote connections.
- [x] Add a failing regression test for remote-server isolation.
- [x] Implement the smallest owner-aware fix.
- [x] Run focused tests and `bun run check:all`.
- [x] Record verification and review results.

## Review

- Chat windows now derive the Jean server from the scoped worktree ID and provide that owner to preferences and model hooks.
- Backend installation checks, model queries, favorites, fast-mode choices, OpenCode refresh, and PI default-model synchronization use the owning server.
- Query keys include the server ID so cached local model data cannot appear in a remote session.
- Focused frontend tests passed (41 tests), typecheck passed, lint passed, Rust formatting passed, Clippy passed, and all 2,387 frontend tests passed.
- `bun run check:all` reached Rust tests, then stopped on the unrelated existing `Project` test initializer at `src/projects/commands.rs:15299`, which is missing the new `sentry_base_url` field.
