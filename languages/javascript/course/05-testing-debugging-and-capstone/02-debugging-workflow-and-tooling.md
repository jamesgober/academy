<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [JavaScript](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Debugging Workflow and Tooling

> Debugging is a process: reproduce, isolate, fix, verify.

**You will learn:**
- practical bug triage flow
- debugger versus logging tradeoffs
- post-fix regression prevention

**Before this page, you should know:** reading runtime errors.

---

## Triage workflow

1. reproduce consistently
2. capture exact input and output mismatch
3. isolate smallest failing unit
4. inspect state in debugger/logs
5. apply minimal fix
6. add test to prevent regression

## Tooling basics

- `console.log` for quick state checks
- browser or editor debugger breakpoints for step-through inspection
- VS Code breakpoints for variable/state inspection

---

## Recap

- Reliable reproduction is step zero.
- Use debugger when control flow is unclear.
- Add tests after bug fixes.

## Try it yourself

Take one prior exercise, introduce a bug intentionally, and resolve it using the triage steps.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Testing Fundamentals](./01-testing-fundamentals.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Linting, Formatting, and Quality Gates →](./03-linting-formatting-and-quality-gates.md) |

</div>
