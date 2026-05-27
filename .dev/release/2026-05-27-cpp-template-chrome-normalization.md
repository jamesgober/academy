# 2026-05-27 C++ Template Chrome Normalization

## Summary
Standardized C++ lesson pages to match tutorial template navigation chrome and preserved prior content improvements.

## Changes
- Normalized all C++ lesson files under `languages/cpp/course/**` (excluding chapter READMEs) to include template-style top header block and head navigation.
- Normalized all C++ lesson files to include a single template-style footer table with Previous/Up/Next links.
- Rebuilt chapter-local previous/next routing based on lesson order to restore navigation continuity.
- Removed duplicated and malformed footer fragments left by prior inconsistent edits.
- Preserved compiler source links, install commands, and visual model content in C/C++ install lessons.

## Validation
- Checked all C++ lesson files for required head/foot nav markers and nav table (`NAV_ISSUE_COUNT=0`).
- Checked C and C++ docs for empty visual model Mermaid blocks (`EMPTY_VISUAL_COUNT=0`).
- Ran diagnostics on `languages/cpp` and `languages/c`; no errors found.
