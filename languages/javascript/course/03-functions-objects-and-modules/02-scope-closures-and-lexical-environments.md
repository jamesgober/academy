<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# Scope, Closures, and Lexical Environments

> Closures are a core JavaScript power feature and a frequent source of confusion.

**You will learn:**
- block scope with `let` and `const`
- lexical scope rules
- closure behavior in practical patterns

**Before this page, you should know:** function basics.

---

## Scope basics

```javascript
let outer = "outside";

function show() {
  const inner = "inside";
  console.log(outer, inner);
}
```

`inner` is not accessible outside `show`.

## Closure example

```javascript
function createCounter() {
  let count = 0;
  return function increment() {
    count += 1;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

The returned function keeps access to `count` after `createCounter` finishes.

## Common closure bug

Avoid loop closure surprises by using `let` instead of `var` for loop variables.

---

## Recap

- Scope controls visibility.
- Closures preserve lexical environment.
- `let`/`const` reduce historical `var` scope bugs.

## Try it yourself

Build a `createMultiplier(factor)` closure and test it with two factors.

---

[**Next ->** Objects, Classes, and Prototypes](./03-object-patterns-and-prototypes.md)  
[**<- Previous** Functions and Parameters in Depth](./01-functions-and-parameters-in-depth.md)
