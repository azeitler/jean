# Task: Add clone provider selector

- [x] Review the existing select component and clone form tests
- [x] Add failing tests for GitHub, GitLab, and Custom URL handling
- [x] Add the provider selector and URL construction
- [x] Run focused and full frontend verification
- [x] Review the diff and record results

## Review

- GitHub is the default provider and accepts `owner/repository`.
- GitLab accepts group and nested-group repository paths.
- Custom keeps the previous unrestricted full-URL behavior.
- TypeScript, focused ESLint checks, 12 focused tests, and all 2,381 frontend tests passed.
