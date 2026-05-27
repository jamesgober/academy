<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [JavaScript](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Your First JavaScript Program

> Build a tiny program and explain every moving part.

**You will learn:**
- File structure of a minimal JavaScript program
- `console.log` usage and output expectations
- How runtime errors appear for beginner mistakes

**Before this page, you should know:** how to run JavaScript in browser console or an HTML page.

---

## Minimal example

```javascript
const user = "Avery";
const tasks = 3;

console.log(`Hello, ${user}. You have ${tasks} tasks.`);
```

Run it by loading `index.html` in your browser and opening Developer Tools Console.

Expected output:

```text
Hello, Avery. You have 3 tasks.
```

## Common beginner mistakes

- Missing quote in string literal
- Missing closing parenthesis
- Misspelled variable names

Each will show a stack trace or syntax error with file and line location.

---

## Recap

- JavaScript files can run directly in browser pages.
- Template literals make string output clear.
- Syntax errors include line references; read them before editing randomly.

## Try it yourself

Add one more variable and print it in the same template literal.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Setting Up a JavaScript Practice Environment](./01-setting-up-a-javascript-practice-environment.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [JavaScript Files, Modules, and Execution Flow →](./03-javascript-files-modules-and-execution-flow.md) |

</div>
