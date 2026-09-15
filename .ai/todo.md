# Manual remote server reconnect

- [x] Add failing tests for manual reconnect and refresh-button placement.
- [x] Add one remote reconnect action at the connection-manager boundary.
- [x] Show a refresh icon beside remote server names in sidebar sections and the title header.
- [x] Run focused tests and `bun run check:all`.
- [x] Record the result and test steps.

## Review

- Added one shared refresh icon control for remote server labels in grouped sidebar sections and the title header.
- A click resets capability negotiation and forces only that remote WebSocket to reconnect. Local and other remote connections are unchanged.
- Focused reconnect and placement tests pass (44 tests). TypeScript, ESLint, Rust formatting, Rust clippy, and all 2,386 frontend tests passed through `bun run check:all`.
- The final Rust test stage remains blocked by an unrelated existing `Project` test initializer in `jean-core/src/projects/commands.rs` that is missing `sentry_base_url`.
- Jean reported no active run environment, so live UI verification was not available.

## How to test

- Open the native desktop app with at least one remote server and select **All servers**.
- In the sidebar, select the refresh icon beside a remote server heading. Confirm its projects reconnect without changing another server.
- Select a project from that remote server. In the header, select the refresh icon beside its server name and confirm the remote reconnects.
- Take the remote offline, select either refresh icon, start the remote again, and confirm its connection and project data recover.
