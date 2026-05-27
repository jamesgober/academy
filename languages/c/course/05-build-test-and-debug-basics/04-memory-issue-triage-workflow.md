<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Memory-Issue Triage Workflow

> Memory bugs become manageable when triaged with a repeatable process.

**You will learn:**
- A practical order for investigating memory problems
- How to classify bug types quickly
- How to verify fixes without guesswork

**Before this page, you should know:** [Address Sanitizer and Leak Detection](./03-address-sanitizer-and-leak-detection.md)

---

## Triage order

1. Reproduce the issue reliably.
2. Capture exact tool output (warning, sanitizer, crash trace).
3. Classify bug type: leak, use-after-free, out-of-bounds, double free, uninitialized use.
4. Find ownership/lifetime mismatch in code.
5. Fix design and code.
6. Rerun strict build + sanitizer.

## Why this works

It keeps investigation structured.
It avoids random edits that hide symptoms but keep root causes.

## Quick classification hints

- leak: missing cleanup path
- use-after-free: pointer outlives allocation
- double free: duplicate ownership or repeated cleanup call
- out-of-bounds: index/length mismatch

---

## Recap

- Triage starts with reproducibility.
- Classification narrows root cause search.
- Verification must include rerunning quality tools.

## Try it yourself

Pick one bug type and write a three-line "symptom -> cause -> fix" summary.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Address Sanitizer and Leak Detection](./03-address-sanitizer-and-leak-detection.md) | [Chapter 05](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Quality Workflow and Release Checklist →](./05-quality-workflow-and-release-checklist.md) |

</div>
