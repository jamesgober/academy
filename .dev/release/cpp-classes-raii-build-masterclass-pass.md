# C++ Classes, RAII, And Build Masterclass Pass

Date: 2026-05-29

## Summary

Expanded the C++ track from a skeletal overview into a more beginner-friendly
learning path for classes, ownership, RAII, smart pointers, debugging, testing,
and a guided capstone project.

## Course Areas Improved

- Rebuilt Chapter 01 first-program, compile/run, and checkpoint lessons so a
  brand-new learner sees the source-file-to-executable workflow, terminal folder
  checks, strict compiler flags, error reading, common beginner mistakes, and a
  guided first practice loop.
- Rebuilt Chapter 03 classes and objects around invariants, constructors,
  `const`, `static`, composition, inheritance, virtual dispatch, and a task model
  checkpoint.
- Rebuilt Chapter 04 memory and RAII around stack/heap lifetime, references,
  smart pointers, leaks, dangling pointers, use-after-free, double delete,
  sanitizers, and a safe inventory checkpoint.
- Rebuilt Chapter 05 build/test/debug content around strict compiler flags,
  multi-file builds, no-framework tests, warning triage, sanitizer triage, and a
  complete inventory capstone.
- Expanded C++ reference pages for classes/OOP, memory safety/RAII, and commands
  so learners can jump from lessons to lookup-style summaries.
- Rebuilt the C++ types and strings reference from a tiny list into a practical
  lookup page covering type choice, `std::string`, input, characters, numeric
  conversion, initialization, and risk notices.
- Rebuilt the C++ conditionals/loops and functions/parameters references with
  practical patterns, parameter-style decision guidance, grouped return examples,
  loop/search patterns, overload notes, and risk notices.
- Rebuilt the C++ errors/warnings/sanitizers reference with compiler diagnostic
  triage, strict warning flags, common beginner compiler problems, ASan/UBSan
  builds, sanitizer report vocabulary, RAII fix direction, and risk notices.
- Rebuilt Chapter 02 core basics after the main pass so the track no longer
  jumps from thin syntax notes into advanced ownership material. The expanded
  lessons now cover variables, initialization, strings, input validation,
  function parameter choices, guard clauses, branching patterns, vectors, range
  loops, indexing, accumulation, and common beginner mistakes.
- Rebuilt the Chapter 02 checkpoint into a guided score-analyzer program that
  integrates vectors, loops, functions, `const` references, numeric conversion,
  guard clauses, grading logic, and stretch exercises.

## Validation

- C++ markdown broken-link scan: clean
- C++ odd-code-fence scan: clean
- C++ Mermaid block scan: clean
- C++ stale nav/chrome/mojibake scan: clean

## Local Compile Note

A representative capstone compile was attempted with the local LLVM toolchain,
but the installed Clang version is older than the Visual Studio STL on PATH
expects. The failure was a local toolchain version mismatch before semantic
compilation, not a markdown validation failure.
