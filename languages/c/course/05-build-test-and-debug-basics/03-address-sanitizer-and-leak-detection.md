<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Address Sanitizer and Leak Detection

> Sanitizers are practical tools for catching memory errors you cannot reliably see by inspection alone.

**You will learn:**
- What AddressSanitizer is
- How to compile with sanitizer support
- How sanitizer output helps detect leaks and invalid memory use

**Before this page, you should know:** [Runtime Debugging Basics](./02-runtime-debugging-basics.md)

---

## Example compile with sanitizer

```bash
gcc -g -O1 -fsanitize=address -fno-omit-frame-pointer main.c -o app
```

Then run `./app` and inspect sanitizer output.

## What it can catch

- use-after-free
- out-of-bounds access
- some leaks
- double free patterns

## How to respond

- read the reported stack trace
- identify allocation and failure points
- fix ownership/lifetime logic
- rerun until clean

> [!IMPORTANT]
> Sanitizers find symptoms quickly, but you still need good ownership design to prevent recurrence.

---

## Recap

- Sanitizers are essential for C memory debugging.
- Build with sanitizer flags during development.
- Use reports to fix root causes, not just local crashes.

## Try it yourself

Create a tiny leak intentionally, run with sanitizer, and explain the report in plain language.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Runtime Debugging Basics](./02-runtime-debugging-basics.md) | [Chapter 05](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Memory-Issue Triage Workflow →](./04-memory-issue-triage-workflow.md) |

</div>
