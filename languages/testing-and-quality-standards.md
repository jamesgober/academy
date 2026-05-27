<div align="center">

[Home](../README.md) · [Languages](./README.md)

</div>

---

# Language Testing and Quality Standards

> Every language track must include a testing section and a CI-aligned quality workflow.

## Why this standard exists

Learners should not only write code. They should verify behavior, prevent
regressions, and ship changes safely.

## Required in every language track

1. A dedicated testing lesson or chapter that covers:
   - test types for the language
   - how to run tests locally
   - how to interpret failing tests
2. A quality workflow section that covers:
   - formatting
   - linting/static analysis
   - tests in CI
3. A release-readiness checklist:
   - all required checks green
   - changelog updated
   - version increment policy applied

## Minimum cross-track structure

Each track should contain:
- one beginner testing lesson in early chapters
- one CI/testing integration lesson before release topics
- one reference page for common testing commands and troubleshooting

## Example expectations for Rust

- `cargo fmt --check`
- `cargo clippy --all-targets --all-features -- -D warnings`
- `cargo test --all-features`

> [!IMPORTANT]
> Language-specific details vary, but quality principles do not.

## Related platform resources

- [GitHub Masterclass](../platforms/github/)
- [CI Workflow Setup Checklist](../platforms/github/reference/ci-workflow-setup-checklist.md)

---

<div align="center">

[← Languages](./README.md) · [Home](../README.md)

</div>
