# Disable Local Editor Opens for Remote Files

- [x] Trace the file modal editor-open route and identify the local dispatch.
- [x] Add a failing regression test for a remote-owned file.
- [x] Hide the local external-editor action for remote-owned files.
- [x] Run focused tests and `bun run check:all`.
- [x] Record review and verification results.

## Review

- Remote-owned files keep Jean's inline Edit action but no longer show the local Open in Editor action.
- Local-owned files still show Open in Editor when the runtime supports it.
- The focused component suite passed: 3 tests.
- TypeScript, ESLint, Rust formatting, Rust Clippy, and all 2,363 frontend tests passed in `bun run check:all`.
- Rust tests did not start because of an unrelated existing missing `sentry_base_url` field in `jean-core/src/projects/commands.rs:15299`.
