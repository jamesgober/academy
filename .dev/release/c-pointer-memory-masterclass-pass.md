# C Pointer And Memory Masterclass Pass

## Summary

Started the Rust-level perfection pass for the C track by rebuilding the pointer
and memory chapter into a beginner-friendly sequence with concrete mental
models, safe patterns, warnings, and a guided capstone.

## Concrete Changes

- Expanded memory-address coverage with value/name/address separation,
  `&variable`, `%p`, `(void *)`, address instability, and safety warnings.
- Expanded pointer fundamentals with pointer declarations, pointer types,
  `NULL`, uninitialized pointer risks, `const int *`, and readable declaration
  guidance.
- Expanded dereferencing with read/write examples, declaration-vs-expression
  `*`, null checks, undefined behavior, `const`, and debugging prompts.
- Expanded function pointer arguments with pass-by-value, pass-by-address,
  output parameters, swap examples, array parameters, explicit counts, and API
  design checklists.
- Expanded common pointer mistakes with uninitialized pointers, null
  dereference, pointer/value confusion, missing `&`, returning stack addresses,
  out-of-bounds access, pointer arithmetic, wrong `sizeof`, and ownership
  prompts.
- Expanded dynamic memory coverage with stack/heap mental model, `malloc`,
  `calloc`, allocation failure, `free`, `sizeof *ptr`, ownership comments,
  overflow notes, and cleanup paths.
- Expanded lifetime bug coverage with leaks, dangling pointers, use-after-free,
  double free, ownership vocabulary, one-exit cleanup patterns, and sanitizer
  guidance.
- Replaced the Chapter 03 checkpoint with a guided heap-backed sensor buffer
  project using allocation, pointer parameters, output parameters, cleanup, and
  sanitizer-friendly design.
- Expanded Chapter 04 array and string lessons with explicit indexes,
  count/capacity, array parameters, `sizeof` limits, null terminators,
  `strlen`, string literals, `snprintf`, `fgets`, newline removal, truncation
  checks, destination-buffer APIs, and safe struct/array layout patterns.

## Validation

```powershell
BROKEN_LINK_COUNT=0
ODD_FENCE_COUNT=0
MERMAID_BLOCK_COUNT=0
STALE_NAV_OR_MOJIBAKE=0

CourseFiles    : 36
Lessons        : 30
ReferencePages : 6
CourseWords    : 14308
ReferenceWords : 561
TotalWords     : 14869
```
