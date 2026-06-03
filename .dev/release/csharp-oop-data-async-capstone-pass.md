# C# OOP, Data, Async, And Capstone Pass

Date: 2026-05-29

## Summary

Expanded the C# track toward a beginner-friendly masterclass standard across
class design, constructors, interfaces, collections, failure design, LINQ, files,
JSON, async, xUnit testing, and the console order-tracker capstone.

## Course Areas Improved

- Rebuilt Chapter 01 first-program, project-command, and checkpoint pages around
  modern top-level statements, classic `Main`, `.csproj` structure, `bin/` and
  `obj/`, `dotnet new/build/run/clean`, diagnostic IDs, and a deliberate
  break/fix workflow.
- Rebuilt the C# diagnostics lesson around compiler output anatomy, common
  first errors, nullability warnings, triage loops, and deliberate practice.
- Expanded C# command and conditional/loop references with `dotnet` command
  semantics, working-directory notes, quality loops, switch expressions, pattern
  matching, loop choices, LINQ alternatives, and risk notices.
- Rebuilt class/property guidance around fields, properties, validation,
  `private set`, constructors, invariants, object lifecycle, `IDisposable`, and
  `using`.
- Rebuilt interface guidance around contracts, polymorphism, fakes, composition,
  inheritance, and testability.
- Rebuilt collections/data guidance around `List<T>`, dictionaries, sets,
  read-only views, exceptions, `TryParse`, LINQ, files, JSON, DTOs, and
  persistence failure cases.
- Rebuilt async/testing/capstone guidance around `Task`, `async`/`await`,
  cancellation, `Task.WhenAll`, xUnit facts/theories, async tests, fakes, and a
  DTO-backed order-tracker project.
- Expanded records/structs coverage with value equality, `with` expressions,
  record classes, record structs, mutable-struct risks, and type-selection
  decision tables.
- Expanded debugging/logging workflow with stack trace reading, breakpoints,
  structured logging, log levels, bug-to-test workflow, and safety notices.
- Expanded reference pages for OOP/type design, collections/LINQ, and
  errors/debugging with lesson cross-links, examples, and risk notices.
- Rebuilt Chapter 02 fundamentals around variables, numeric type choice,
  `decimal` for money, `var`, nullable values, string handling, `TryParse`,
  method signatures, `out`, `ref`, optional parameters, overloads, `params`,
  loop selection, filtering, accumulation, and command-loop patterns.
- Rebuilt the Chapter 02, 03, and 04 checkpoints as guided integration builds:
  an order-total reporter, an invoice object model, and an inventory JSON loader.
- Expanded the types/strings reference with lesson links, examples, nullable
  guidance, parsing patterns, and risk notices.

## Validation

- C# markdown broken-link scan: clean
- C# odd-code-fence scan: clean
- C# Mermaid block scan: clean
- C# stale nav/chrome/mojibake scan: clean
- Representative C# capstone core smoke test: passed with local .NET SDK
