# Consolidate context injection actions

## Match sidebar separator colors

- [x] Add a failing regression test for both desktop sidebar separators.
- [x] Use the subdued project-sidebar divider color for both separators.
- [x] Run focused tests and quality checks.
- [x] Record the result and manual test steps.

- [x] Add failing desktop and mobile menu tests.
- [x] Keep one context injection action and label it "Inject Context".
- [x] Run focused tests and quality checks.
- [x] Record review and manual test steps.

## Native/mobile parity follow-up

- [x] Add failing parity tests for all native Magic options.
- [x] Add missing Link PR and Advisory actions to mobile Web Access.
- [x] Match the native Release action labels.
- [x] Run focused tests and quality checks.

### Follow-up review

- Native and mobile Web Access now expose the same 20 Magic actions.
- Link PR uses the existing native dialog logic through a focused UI event.
- Advisory follows the existing mobile investigation event flow and is disabled without loaded advisory context.

## Review

- The left-sidebar and file-browser resize separators now use `border/40`, matching the subdued project-sidebar divider.
- The focused separator test, ESLint, and TypeScript typecheck pass.
- Manual check: open Jean on desktop and compare the vertical sidebar divider with the horizontal divider above the project list.
- Desktop Magic now shows one "Inject Context" action with the J shortcut.
- The mobile Magic menu uses the same label, icon, shortcut, and existing context-picker handler.
- Focused component tests, ESLint, TypeScript typecheck, and diff checks pass.
- Manual check: open Magic on desktop and mobile, then select Inject Context and confirm that the context picker opens on the Sessions list.
