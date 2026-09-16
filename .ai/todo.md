# Investigate issue model selector in Web Access

- [x] Trace the investigate-issue dialog and model selector conditions.
- [x] Compare native desktop, Web Access, and mobile behavior.
- [x] Review existing regression coverage for Web Access.
- [x] Run focused tests for both investigate entry points.
- [x] Record the review result and verification evidence.

## Review

- The current source shows the backend/model selector in Web Access, native desktop, and mobile.
- `NewWorktreeModal` renders `DesktopBackendModelPicker` for the Issues and PRs tabs without an `isNativeApp()` condition.
- `MagicModal` renders the "Choose backend + model" controls without a platform condition.
- Existing tests explicitly cover native desktop, Web Access, and mobile model selection.
- Focused verification passed: 2 test files, 22 tests.
- No Run environment was available for browser verification.
- No production code change was required. A deployed Web Access client that lacks the selector is likely serving an older frontend bundle or cached page.
