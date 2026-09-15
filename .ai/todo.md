# Open Remote Files in the Configured Editor

- [x] Trace the file modal editor-open route and identify the local dispatch.
- [x] Add a failing regression test for a remote-owned file.
- [x] Route explicit remote file ownership through the remote editor mapping.
- [x] Keep Open in Editor visible for remote-owned files.
- [x] Run focused tests and `bun run check:all`.
- [x] Record review and verification results.

## Review

- Open in Editor remains visible for remote-owned files.
- The modal now sends explicit server ownership to the transport layer.
- Native remote files use the same SSH editor mapping as remote worktrees.
- The 34 focused modal and transport tests passed.
- TypeScript passed in `bun run check:all`; the full check then stopped on unrelated concurrent JSX corruption in dashboard and worktree files.
