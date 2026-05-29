# Rust Masterclass Refactor

## Summary

Started the Rust track refactor from skeleton-style overview pages toward a
beginner-friendly masterclass with deeper explanations, real project structure,
module/export coverage, Cargo configuration examples, and practical references.

## Concrete Changes

- Expanded first Cargo project lesson with Cargo mental model, generated file
  explanations, `Cargo.toml` fields, dependency setup, expected output, and
  beginner troubleshooting.
- Expanded Rust project structure lesson with binary/library crates, tests,
  examples, `src/lib.rs`, `src/main.rs`, `target/`, and a growth path from one
  file to reusable code.
- Rebuilt modules and visibility lesson to cover `mod`, `pub`, `use`,
  `pub use`, crate paths, private fields, cross-file modules, re-exports, and
  common compiler errors.
- Replaced the Chapter 05 project placeholder with a guided inventory CLI
  project including full file layout, library API, binary entry point,
  integration tests, examples, quality commands, and extension ideas.
- Expanded Rust reference index and added references for Cargo manifests,
  module/export syntax, types/strings/collections, functions/generics/traits,
  errors/warnings/debugging, and ecosystem lookup.
- Updated authoring templates to prefer left-aligned slash breadcrumbs and simple
  previous/next footer links instead of navigation tables.
- Replaced Rust Mermaid visual models with plain text diagrams so diagrams render
  in Markdown viewers that do not support Mermaid.
- Updated Rust course footers so the next link appears above the previous link,
  with bold labels and arrows.
- Added Rust prelude coverage to the modules/visibility lesson and reference.
- Added Chapter 06, Practical Rust Mastery, with full lessons for function
  signature design, ownership-aware parameters, strings, collections, traits,
  generics, iterators, closures, files, CLI input, environment variables,
  macros, attributes, documentation, Cargo features, polish, and a final Study
  Log CLI capstone.
- Added a practical Rust patterns reference for CLI, parsing, files, iterators,
  worker threads, macros, attributes, and quality commands.
- Deepened Chapter 02 from Rust core summaries into full beginner lessons on
  variables, ownership, borrowing, lifetimes, structs, enums, pattern matching,
  `Option`, `Result`, and a garage intake checkpoint project.
- Deepened Chapter 03 into a real testing workshop covering unit tests,
  integration tests, doc tests, edge cases, ignored tests, panic tests,
  property/fuzz/snapshot testing tradeoffs, CI, failure debugging, flaky tests,
  and a tested Study Log library capstone.
- Deepened Chapter 04 into a practical concurrency and async chapter covering
  data races, `Send`, `Sync`, `thread::spawn`, `JoinHandle`, `Arc<Mutex<T>>`,
  channels, async futures, Tokio runtime basics, `.await`, timeouts,
  cancellation, retries, and a threaded job dispatcher capstone.
- Deepened Chapter 05 around package/crate/module structure, `Cargo.toml`,
  library APIs, binary entry points, multiple binaries, side-effect boundaries,
  module cleanliness, re-exports, and refactoring messy Rust codebases.

## Validation

Validation run:

```powershell
BROKEN_LINK_COUNT=0
ODD_FENCE_COUNT=0
MERMAID_BLOCK_COUNT=0

Lessons        : 36
CourseFiles    : 42
CourseWords    : 36221
AvgLessonWords : 1006
ReferencePages : 13
ReferenceWords : 4363
AvgRefWords    : 336
TotalWords     : 40584
```
