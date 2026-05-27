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
# Sanitizers and Memory-Issue Triage

Use sanitizers during development to catch hidden runtime memory bugs.

```bash
g++ -std=c++20 -g -O1 -fsanitize=address -fno-omit-frame-pointer main.cpp -o app
```

Common findings:
- use-after-free
- heap-buffer-overflow
- leaks

## Example report fragment

```text
==12345==ERROR: AddressSanitizer: heap-use-after-free on address 0x...
```

How to read it:
- bug class: `heap-use-after-free`
- address: failing memory location
- stack trace below: where invalid access occurred and where free happened

## Root-cause workflow

1. Identify owner of allocation.
2. Identify where ownership ended (`delete`, smart pointer reset, scope end).
3. Find stale pointer/reference still in use.
4. Refactor ownership/lifetime boundary.
5. Rerun sanitizer and strict build.

## Prevention patterns

- prefer RAII wrappers
- prefer `std::unique_ptr` for exclusive ownership
- avoid raw owning pointers in APIs
- avoid manual `new`/`delete` in application logic when possible

Triage flow:
1. reproduce
2. inspect report stack
3. fix ownership/lifetime cause
4. rerun sanitizer
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Debugging, Errors, and Warning Navigation](./03-debugging-errors-and-warning-navigation.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 05 Capstone Project →](./05-chapter-05-capstone-project.md) |

</div>