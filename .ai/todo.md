# Disable Steering by Default

- [x] Add regression tests for queue-first defaults.
- [x] Change Rust and TypeScript steering defaults and fallbacks.
- [x] Update related comments and setting fallbacks.
- [x] Run focused tests and quality checks.
- [x] Record review and verification results.

## Review

- Codex, OpenCode, PI, and Grok now queue prompts by default.
- Explicitly enabled steering continues to steer running turns.
- Missing persisted fields deserialize to queue-first defaults; existing explicit values remain unchanged.
- All 2,361 frontend tests, TypeScript, ESLint, Rust formatting, and Rust Clippy passed.
- Rust tests could not start because an unrelated existing `Project` test initializer in `jean-core/src/projects/commands.rs:15299` is missing `sentry_base_url`.
