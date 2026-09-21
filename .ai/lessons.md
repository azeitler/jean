# Lessons

## Preserve alignment when simplifying controls

- When a user asks to simplify a footer control to an icon, keep its existing side alignment unless the user explicitly asks to center it.
- In a sidebar footer, keep a single action in the normal left-side position.

## Use stable, owner-aware resource identities

- Values, query keys, command arguments, and persisted references must include a stable resource ID and its owner when IDs are not globally unique.
- Register ownership at adapter boundaries so path-only commands and remote assets route through the correct server.
- Keep ownership visible in multi-source UI and test duplicate display names.
- Do not persist temporary session context under a broader worktree or project identity.

## Keep multi-server behavior at the correct boundary

- Native desktop can aggregate enabled remote servers, but Web Access must use only its serving origin.
- Treat remote profiles as parallel adapters. Do not replace the native app's local core with a global backend switch.
- A server can remain connected and selectable while excluded from aggregate dashboard results.
- Gate native-only connections, aggregation, routing, caches, and ownership labels with `isNativeApp()`.

## Make state transitions atomic and observable

- Update durable storage and the active query cache before closing setup UI, or stale cache data can reopen it.
- Subscribe Zustand components to the data that controls rendering, not to stable getter functions.
- Guard Zustand mutations against no-op updates so unchanged values do not notify all subscribers.
- Test timing-sensitive close, reopen, refresh, and restoration paths, not only the persistence call.

## Verify behavior at system boundaries

- Read backend selection and ownership logic before relying on a command name or frontend callback.
- Test the real boundary contract: command arguments, CLI values, generated configuration, serialized data, and restored history.
- Verify every responsive UI branch and each supported runtime: native desktop, Web Access, and mobile where applicable.
- Keep long-running jobs backend-owned when they must survive client disconnects.

## Normalize external backend behavior end to end

- Classify a backend by its current documented transport and capabilities before integration. Do not keep old transport assumptions after a product migration.
- Normalize exact tool names and parameter shapes at the display boundary, with readable fallbacks for known native tools.
- Add live-stream and persisted-history parsers together, and route history by the per-run backend before generic fallback logic.
- Resolve default, custom, and empty prompts consistently on every backend; backend mode instructions must augment the shared prompt.
- A capability is complete only when preferences, every send path, persistence, rendering, cancellation, and tests all support it.

## Verify external tools from authoritative sources

- Check the current official site, migration guide, installer, release manifest, and binary help before designing an integration.
- Do not use an old registry entry or similarly named third-party package as proof of the supported product path.
- Check Jean-managed binary locations before declaring a tool unavailable; distinguish “not on PATH” from “not installed.”
- Verify installed skills through each harness's real discovery path. File presence alone does not prove discovery or invocation.

## Make setup actions produce a usable result

- If a feature needs dependent configuration, make its primary install action complete the safe required steps.
- Do not add a second setup action when Jean can configure the dependency automatically.
- Update default agent guidance when installation alone does not expose the intended workflow.

## Keep user-facing output explicit

- Discovery results must show a visible state for every result type, such as open, closed, or merged.
- Background actions should use one toast lifecycle: loading, then success or error with the same ID.
- Show persistent context such as resource ownership where the user needs it to understand later actions.

## Treat fresh-session creation as a backend contract

- Confirm that a “start” command explicitly creates a fresh session when the UI promises one.
- Wait for the returned session ID, select that exact session, and then open its view.
- Put one-shot investigation context on the created session so it cannot leak into later sessions.
- Test backend session selection and restoration, not only that the UI callback runs.

## Design cross-platform affordances explicitly

- Hide and disable keyboard-only actions outside native desktop unless they are useful in that runtime.
- Do not assume Unicode modifier glyphs render in browser fonts; use explicit labels such as `Ctrl` in Web Access.
- Test click or tap behavior separately from native keyboard shortcuts.

## Keep task tracking proportional

- Reset `.ai/todo.md` for each task and keep its checklist proportional to the work.
- For small fixes, record a short plan, verification, and result instead of a duplicate implementation report.
- Explain that `.ai/todo.md` is internal task tracking when a user asks why it changes.

## Separate implementation limits from external limits

- For production-readiness reviews, classify every gap as either implementable in Jean or unavailable in the external backend.
- Do not mark work complete while implementable items remain.
- Report verification limits separately from implementation limits.

## Reproduce the exact visible state before selecting a cache fix

- Distinguish a stale canvas or list status from a stale status inside the open session.
- Trace the exact component and every state source that can keep a running indicator visible.
- Add a regression test for the user-visible open-session state, not only a nearby cache with similar data.
- When UI rendering defers a resource ID, defer its owner and routing context as one value. Never combine a previous server resource ID with the new server's worktree or path.
- If a switched chat shows an incomplete tool or running state, do not conclude that stale query routing is the full cause. Compare WebSocket event decoration and replay with the persisted session response before selecting the fix.

## Test completion as one lifecycle, including selection

- A completion fix must verify the open-session indicator and the active session ID together.
- Cache invalidation can affect both status data and automatic selection effects; test that completion never changes the user's current session.

- When fixing UI flicker, removing duplicate requests is not sufficient. Verify the actual first-paint and transition behavior. If the user reports that one header field appears late, test the rendered visibility gate, not only the data source wiring.

- When a remote attachment works for the backend but not in the UI, verify both upload routing and client preview URL ownership. A remote filesystem path is not directly loadable by the native webview; render it through the owning server file URL.

## Rank palette and list entries by likely target, not by legacy order

- When a new entity type joins an existing list, do not default to appending it below the old entries only to protect existing muscle memory.
- Rank by which entry the user most probably wants. In a session-centric app, the last session beats the last project.
- Surface the ordering trade-off as an explicit choice before implementing, but recommend the likely-target order.

## Confirm the UI element exists before you plan a change to it

- A user who says "the chat shows a timestamp" can be describing something else.
  In the chat thread the only time-like text was the turn runtime (`02:25`), not a
  clock time.
- Grep for the concrete formatting call (`toLocaleTimeString`, `timeStyle`,
  `getHours`) before you plan an edit to a described element. If it does not exist,
  say so and give the user numbered options instead of guessing.
- A duration in `mm:ss` reads like a clock time. Expect this confusion again.
- When you add a value next to an existing one, keep each in its own element with
  its own render condition, so the new value cannot swallow the old one. Keep the
  test that asserts the old value visible - it is the regression guard.

## Send GitHub writes to the fork, never to upstream

- `gh` resolves this repository to the fork parent `coollabsio/jean`, not to
  `origin` (`azeitler/jean`). A bare `gh issue create` therefore posts upstream.
- Always pass `--repo azeitler/jean` on write commands: issues, discussions,
  pull requests, comments, labels, releases.
- Only target `coollabsio/jean` when the user asks for it in that message.
- A 404 on a write command against a repository you did not name is the signal
  that `gh` picked the wrong repository. Check `git remote -v` before you retry.
- See `CLAUDE.local.md` for the standing rule.

## A green local gate does not prove a green CI clippy

- CI installs the latest stable Rust (`dtolnay/rust-toolchain@stable`). The local
  `stable` can be many versions behind, and every new clippy adds lints. The
  v0.1.73-z.4 build failed on `clippy::unnecessary_sort_by` after `check:all`
  passed locally on 1.93 while CI ran 1.98.
- Before a push that cuts a release, run clippy with CI's version. Install it
  beside the default so nothing else changes:
  `rustup toolchain install <ver> --profile minimal -c clippy`, then
  `cargo +<ver> clippy ... -- -D warnings` for jean-core, src-server and
  src-tauri. The failed CI log names the version in its clippy help URL.

## Verify bulk GitHub writes; `gh` can drop them without an error

- `gh issue close N --comment ...` in a fast loop closed all 12 issues but posted
  only 3 of the 12 comments. The command exited 0 each time, so nothing showed
  the loss.
- After any bulk write, read the result back through the API (for example
  `gh api repos/<repo>/issues/N/comments`) and count. Space the writes out
  (a few seconds each) and check each one as you go.
- Never probe with a real comment on a real issue. If a probe is unavoidable,
  delete it straight away (`gh api -X DELETE repos/<repo>/issues/comments/<id>`).

## A click that should land on a view must win over auto-navigation

- Making the sidebar project row call `selectProject()` was not enough to show
  the project page. `ProjectCanvasView` reopens the last session on mount when
  `restore_last_session` is on (the default), and a canvas that is already
  mounted keeps its open session modal. Both made the click look broken for
  "some projects" only.
- Before you wire a click to a view, grep the target for mount effects that
  navigate away (`restore_*`, `autoOpen`, `lastOpened*`) and check the case
  where the target is already on screen. Check the user's own preferences
  file too — defaults hide the bug in tests.
- For a request that must reach a view which may mount later, use a one-shot
  UI-store flag the view consumes (like `pendingSidebarRevealId`), not a DOM
  event.

## A new view must fit its chrome, not only render its own content

- When you add a top-level view, check every surrounding surface in that state:
  side panels, title-bar toggles, sticky rows. The Home view first shipped with
  the Files panel open beside it, showing only "Select a project or worktree to
  browse files" - dead space. Gate such a panel on the view, and disable its
  toggle there, so the toggle never shows "pressed" over nothing.
- Hide it at render time. Do not write the persisted visibility preference, or
  the panel stays closed when the user goes back to a project.
- Align a new sidebar row with its visual neighbours, measured in the browser.
  Copying one row's classes is not enough when the tree already has two insets:
  root projects put their glyph at 8px, folders and section headers at 12px.
  Home copied the project row and so sat 4px left of the headers right under it.
- Dashboard content belongs in columns that answer to the view's own width
  (container queries), because the sidebar takes a variable share of the
  window. Narrow columns need two-line rows; one-line rows with a `shrink-0`
  context squeeze the name to a few characters.
- Never turn a query failure into an empty list. An empty feed renders
  "Nothing yet", which is false, and it caches as a success, so TanStack Query
  never retries. Rethrow and render a distinct error with a Retry control.

## When a shared helper gains a side effect, audit every caller

- `navigateToSession` gained a sidebar reveal for CMD+K (#9). The Pinned and
  Starred rows, which sit inside the sidebar, inherited it: a shortcut click
  expanded the original workspace and scrolled the tree away (#18).
- Before adding a side effect to a shared helper, grep every caller and ask
  whether each one wants it. Where the context differs, add an explicit
  option with the old behaviour as the default, and pass it at the callers
  that differ.
- A shortcut row that repeats an item elsewhere in the same view must not
  navigate the view to the original.

## "Active worktree" in the store is not the worktree on screen

- Relative image paths in chat resolved against
  `useChatStore.activeWorktreePath`. Tests passed, but in the app the images
  showed broken: a session opened from the project canvas renders in
  `SessionChatModal`, and the canvas clears the store's active worktree. The
  modal passes its own `worktreePath` prop to `ChatWindow` instead.
- Before code reads the active worktree from the store, check both chat
  surfaces: the full `ChatWindow` (store) and the canvas modal (props). The
  canvas modal is the default way to open a session.
- Pass the on-screen worktree down (props or a React context such as
  `LocalPathRootContext`), and keep the store only as a fallback. Test with
  the store value set to `null`.

## `bun run fix:all` rewrites the whole repository

- `fix:all` runs `eslint . --fix` and `prettier --write .`, both across every
  file. The repository is not Prettier-clean (`check:all` has no
  `format:check` step), so one run reformatted about 155 unrelated files and
  buried a 14-file change.
- Format only the files the change touches:
  `bunx prettier --write <paths>` and `bunx eslint --fix <paths>`.
- Use `bun run check:all` to verify. It never writes.
- If a whole-repo write already happened, list the intended files, then
  `git diff --name-only | grep -vxFf keep.txt` and `git checkout --` the rest.
  Never `git reset --hard`. Untracked files cannot be restored this way, so
  check their timestamps before assuming they survived.

## `emit_to` a window label does nothing on its own (Tauri v2)

- A JS listener registered with `listen(event, cb)` and no options registers
  `EventTarget::Any`, and `match_any_or_filter` short-circuits every emit
  filter. So switching Rust from `emit` to `emit_to(webview_window(label), …)`
  still delivers the event to every window.
- Both halves are needed: emit to the label **and** register the listener with
  `{ target: label }`. See `listenMenu` in `src/lib/transport.ts` and
  `install_menu_events` in `src-tauri/src/lib.rs`.

## A window built at runtime ignores `tauri.conf.json`

- `app.windows[0]` only describes the window Tauri creates at startup. A
  `WebviewWindowBuilder` has to repeat `minInnerSize`, `titleBarStyle`,
  `hiddenTitle` and the drag-drop handler, or the new window behaves
  differently from `main`.
- The builder method for `"dragDropEnabled": false` is
  `disable_drag_drop_handler()`, not `drag_drop_enabled(false)`.
- Reach the caller's window with a `window: tauri::Window` command parameter
  instead of `app.get_webview_window("main")`. Tauri injects it.

## `localStorage` is stubbed in the test setup

- `src/test/setup.ts` replaces `window.localStorage` with `getItem: vi.fn(() =>
null)`. Writing with `setItem` and reading it back returns `null`, so a test
  that needs real storage has to drive the mock with its own `Map`.
- Not every test has a full `window`. `vi.stubGlobal('window', { open: ... })`
  in `src/lib/platform.test.ts` has no `location`, so module-init code must use
  `window.location?.search`.

## A short follow-up like "can we try this with X?" is not a licence to guess

- A pronoun with no antecedent in the visible transcript means the context is
  gone, not that I should reconstruct the intent from the working tree. Reading
  git status plus the newest task file gives a plausible task, not the task.
- Ask which "this" is meant. One question costs a turn; a wrong guess costs a
  whole implementation, a real CLI run, and a revert.
- Untracked work has no safety net. `git checkout`/`restore` cannot undo an edit
  to a `??` file, so the only copy of the original is whatever I read earlier in
  the session. Read the whole file before editing it, and prefer reversing each
  edit precisely over rewriting the file from memory.
- `len(str)` counts characters, `wc -c` counts bytes. Do not conclude that a
  revert lost data because the two disagree on a file holding non-ASCII text.

## A file path I put in chat resolves against the worktree root, not the folder I am working in

- `openChatLink()` has no notion of a "current directory". A bare
  `01-plain.html` in an answer becomes `<worktree>/01-plain.html`. When I create
  files in a subfolder and then list them, I must write the path from the
  worktree root: `scratch/browser-fixtures/01-plain.html`.
- A markdown table of bare file names therefore produces a table of dead links,
  and the user reads that as a product bug.
- Inline code with a space in it is never autolinked (a command and a name with
  a space read the same), so a name with a space needs an explicit markdown
  link: `[04 report.html](<scratch/x/04 report.html>)`.

## Do not guess what a web view does with a file type — probe it

- A 40-line Swift `WKWebView` script with a `WKNavigationDelegate` answers
  "does this render, what MIME type, does the load finish" in one run, for
  every extension at once. `swift script.swift path...` needs no project.
- It found two things reading the code never would have: a movie reports the
  navigation as **failed** (`WebKitErrorDomain 204`) although it plays, and a
  `file://` text document is decoded as **Latin-1**, so UTF-8 Markdown shows
  mojibake. Both changed the design.
- Check the control case in the same run. The `.xhtml` fixture showed its
  em-dash correctly, which proved the mojibake was WebKit's decoding and not my
  probe's output encoding.

## An end-to-end test that skips Jean's stream parser proves nothing about the UI

- The Claude dialog harness passed a "real chain" test (real CLI, real shim, real socket) and still failed in the app: the plan was never presented. The test drove the CLI directly, so Jean's own stream consumer in `chat/claude.rs` never ran — and that consumer SIGKILLed the CLI on `ExitPlanMode`'s first, empty `input_json_delta`.
- Before claiming a chat UI flow works, replay a **real** CLI stream (with the same flags Jean passes, including `--include-partial-messages`) through the consumer logic. `scripts/claude-dialog-rig/replay_kill_paths.py` does this for the blocking-tool paths.
- Legacy code paths written while a feature was unreachable were never exercised. Re-read every handler that names the restored tools (`grep '"ExitPlanMode"'`) before shipping a restoration.
- CLI streaming behaviour drifts between versions: 2.1.231 streamed `ExitPlanMode`'s plan as deltas, 2.1.273 streams an empty input and injects `plan`/`planFilePath` into the final assistant message. Test against the version users run.
- When reading a stream-json log, find the `system/init` message by type — do not assume it is line 1. A `rate_limit_event` can come first and silently turn a present tool into an "absent" one.

## Complete a task by its name, not its number

- `bun run task:complete 1` moved a different `task-1-…` file: several tasks can share a priority number, and the script takes the first match. Always pass the unique name (`bun run task:complete restore-claude-dialog-tool-harness`) and check `git status docs/` afterwards.

## Diff a shared file before staging it whole

- `git add CHANGELOG.md` swept another session's uncommitted keychain entry into my commit, although its code was not committed. Caught it before the push only because a later grep looked for it.
- Before staging a file I edited, run `git diff <file>` and check every hunk is mine. `CHANGELOG.md` and `.ai/todo.md` are the usual suspects: several sessions append to them.
- `git add -p` is unavailable here. To commit part of a file, write the version I want, stage it, then restore the other hunks to the working tree (see the rebuild of `483fb431`).
- More than one session commits in this working tree. Check `git log <last-known>..HEAD` before committing, and confirm another commit did not take my hunks.

## Look at the screen, not only the tests

- The phone project modal passed 41 unit tests and 6 e2e tests, and still hid the corner FloatingDock: the new layer sat at `z-30` over the dock's `z-10`. Only a screenshot showed it.
- For layout work, capture the real screens (a throwaway Playwright spec on the e2e mock harness works) and compare them with what the user had before. Then add a guard for what the screenshot caught.
- The e2e mock preferences use `zoom_level: 1.0`, but the field is a percentage (50–200), so the harness renders at 50%. Override `zoom_level` / `mobile_zoom_level` to 100 for true-size screenshots.
