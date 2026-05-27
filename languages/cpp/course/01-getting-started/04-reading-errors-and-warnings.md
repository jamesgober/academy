<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C++](../../README.md) · [Chapter 01](./README.md)

</div>

---
# Reading Errors and Warnings

Errors and warnings are navigation signals, not random terminal noise.

## Difference first

- **error**: build stops, no executable produced
- **warning**: build may continue, but code may still be risky or wrong

## Example compile error

```text
main.cpp:7:15: error: expected ';' before 'return'
```

How to read it:
- file and line: `main.cpp:7`
- severity: `error`
- message: missing semicolon before `return`

## Example warning

```text
warning: comparison between signed and unsigned integer expressions
```

Warnings often indicate correctness or safety risks even when build succeeds.

## How to track errors quickly

1. Start at the first error in output.
2. Open the exact file and line.
3. Fix syntax/type issue at that location.
4. Rebuild.
5. Repeat until clean.

Many later errors disappear after fixing the first real root cause.

## Example decoding workflow

```text
main.cpp:7:15: error: expected ';' before 'return'
```

- file: `main.cpp`
- line: `7`
- column: `15`
- what failed: parser expected semicolon before `return`

Likely fix: add missing `;` on previous statement.

## Warning follow-up workflow

```text
warning: comparison between signed and unsigned integer expressions
```

Likely cause:
- comparing `int` to `size_t`

Common fix:
- use compatible types
- add explicit, safe conversion where appropriate

## Triage flow

1. reproduce error
2. jump to file/line
3. fix smallest cause first
4. recompile with strict flags

## Suggested strict build flags

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -Werror main.cpp -o app
```

Treat warnings as errors while learning. It builds discipline early.
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Compiling and Running Step by Step](./03-compiling-and-running-step-by-step.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 01 Checkpoint →](./05-chapter-01-checkpoint.md) |

</div>