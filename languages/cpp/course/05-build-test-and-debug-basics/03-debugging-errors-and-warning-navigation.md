<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C++](../../README.md) · [Chapter 05](./README.md)

</div>

---
# Debugging, Errors, and Warning Navigation

Debugging quality in C++ depends on reading output precisely and fixing root causes.

## Example error

```text
file.cpp:18:9: error: no matching function for call to 'foo'
```

Read:
- file and line
- severity
- operation that failed

## Typical root-cause pattern for this error

`no matching function` usually means one of:
- wrong argument types
- missing overload
- const/non-const mismatch
- missing include/namespace qualification

## Example warning

```text
warning: comparison of integer expressions of different signedness
```

Warnings should be investigated, not ignored.

## Warning triage example

```text
warning: comparison of integer expressions of different signedness
```

Likely cause:
- comparing `int` with `std::size_t`

Common safe approach:
- align both values to compatible type with clear conversion strategy

## Error and warning navigation workflow

1. Read the first emitted error.
2. Open file and line from that message.
3. Confirm function signature or type expectation.
4. Fix minimal cause.
5. Rebuild and reassess remaining output.

## Debug order

1. reproduce issue
2. isolate smallest failing function
3. inspect values/lifetimes
4. rerun strict build

## Useful build mode for diagnostics

```bash
g++ -std=c++20 -g -O0 -Wall -Wextra -Wpedantic file.cpp -o app
```

`-g` helps debugger visibility; `-O0` keeps behavior closer to source while debugging.
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Tests and Assertions in C++](./02-tests-and-assertions-in-cpp.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Sanitizers and Memory-Issue Triage →](./04-sanitizers-and-memory-issue-triage.md) |

</div>