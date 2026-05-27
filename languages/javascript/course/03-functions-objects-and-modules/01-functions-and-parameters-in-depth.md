<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# Functions and Parameters in Depth

> Function design determines how easy your code is to test, reuse, and debug.

**You will learn:**
- function declarations, expressions, and arrow functions
- default, rest, and destructured parameters
- return strategy and side-effect boundaries

**Before this page, you should know:** arrays and objects.

---

## Function forms

```javascript
function add(a, b) {
  return a + b;
}

const multiply = function (a, b) {
  return a * b;
};

const subtract = (a, b) => a - b;
```

## Parameter patterns

```javascript
function log(message, level = "info") {
  console.log(`[${level}] ${message}`);
}

function sum(...values) {
  return values.reduce((acc, n) => acc + n, 0);
}

function printUser({ name, age }) {
  console.log(`${name} (${age})`);
}
```

## Return guidance

Prefer returning computed values instead of mutating external state where possible.

---

## Recap

- JavaScript has multiple valid function forms.
- Parameter features reduce boilerplate when used clearly.
- Return values generally scale better than hidden side effects.

## Try it yourself

Write one function with default parameters and one with rest parameters.

---

[**Next ->** Scope, Closures, and Lexical Environments](./02-scope-closures-and-lexical-environments.md)  
[**<- Previous** Chapter Functions, Objects, and Modules](./README.md)
