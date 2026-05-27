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

# Testing Fundamentals

> Tests convert assumptions into executable checks.

**You will learn:**
- test design basics for JavaScript logic
- assert patterns for equality and failures
- framework-agnostic test workflow

**Before this page, you should know:** function and module basics.

---

## Minimal test pattern

```javascript
function assertEqual(actual, expected, message) {
  if (actual !== expected) {
    throw new Error(`${message} | expected=${expected}, actual=${actual}`);
  }
}

function sum(a, b) {
  return a + b;
}

assertEqual(sum(2, 3), 5, "sum adds two numbers");
```

Run this in your preferred JS execution environment and verify no error is thrown.

If assertion fails, the thrown error is your test failure signal.

## Practical test habits

- one clear behavior per test
- descriptive test names
- include edge-case inputs

---

## Recap

- JavaScript tests are assertions over behavior, independent of framework choice.
- Clear failure messages make debugging faster.
- Test naming quality matters for maintainability.

## Try it yourself

Write tests for one function with both normal and edge cases.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Debugging Workflow and Tooling →](./02-debugging-workflow-and-tooling.md) |

</div>
