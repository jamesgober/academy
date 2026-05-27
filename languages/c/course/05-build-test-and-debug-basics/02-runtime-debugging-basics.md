<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Runtime Debugging Basics

> Build output is not enough; runtime inspection is how you verify what really happens.

**You will learn:**
- Why runtime debugging matters
- Basic debugger mindset
- How to isolate crash and corruption symptoms

**Before this page, you should know:** [Compiler Warnings and Strict Build Flags](./01-compiler-warnings-and-strict-build-flags.md)

---

## Debugger mindset

A debugger helps answer:
- what line is running now?
- what values are present now?
- where did control flow diverge from expectation?

## Beginner workflow

1. Reproduce the bug.
2. Narrow to a small function.
3. Inspect values near failure point.
4. Verify pointer and buffer assumptions.

> [!TIP]
> In C, "I think this pointer is valid" is not enough. Verify assumptions directly.

---

## Recap

- Runtime debugging verifies real behavior.
- Small-scope investigation beats random edits.
- Pointer and buffer assumptions should be tested, not guessed.

## Try it yourself

Introduce one small bug, then describe how you would isolate it in four steps.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Compiler Warnings and Strict Build Flags](./01-compiler-warnings-and-strict-build-flags.md) | [Chapter 05](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Address Sanitizer and Leak Detection →](./03-address-sanitizer-and-leak-detection.md) |

</div>
