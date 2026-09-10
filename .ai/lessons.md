# Lessons

## Keep task tracking proportional

- The project instructions require work to be tracked in `.ai/todo.md`, but do not expand it with excessive implementation detail.
- For small follow-up fixes, add only a short checklist and result instead of a long duplicate report.
- Explain that `.ai/todo.md` is internal task tracking when the user asks why it changes.

## Normalize backend tool vocabularies at the display boundary

- Do not assume similar tools share names or parameter casing across backends.
- Capture real stream events, map exact backend names and keys to Jean's common renderer contract, and keep an explicit readable fallback for known native tools.
- Test both live-looking tool inputs and persisted inputs so reload does not reintroduce unhandled labels.

## Wire backend capability semantics end to end

- A backend flag is not supported until the frontend selects the correct setting type and every send path forwards it.
- For persistent-only MCP backends, add discovery and installers, but do not show a per-session switch that the CLI cannot honor.
- Test the exact CLI value set and the generated persistent config, not only the low-level command builder.

## Do not confuse incomplete work with upstream limits

- When the user asks for production readiness, classify each gap as either implementable in Jean or unavailable in the external backend.
- Do not call work complete while implementable checklist items remain.
- Do not use an upstream limitation to excuse adjacent Jean work that is still possible.
- State verification limits separately from implementation limits.

## Verify product migrations against the current official CLI

- Do not treat an old package registry entry as proof that a product is still the supported user path.
- Check the official product site, migration guide, installer, release manifest, and downloaded binary help before designing an integration.
- Do not rename an integration while keeping the old transport assumptions. Reclassify the backend from its current documented protocol.
- Do not install similarly named third-party packages. For Antigravity, use Google's official native installer and release manifest, not the unrelated npm package.

## Register live and history parsers together

- A new streaming backend needs two parser paths: the live response parser and the run-log reconstruction parser.
- Route persisted runs by the per-run backend or model prefix before using a generic fallback parser.
- Test history reload with the backend's real NDJSON format. Live streaming success does not prove that the response survives a query refresh or app reload.

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
