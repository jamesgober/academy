# Functions and Closures Cheat Sheet

## Function forms

```javascript
function fn() {}
const fnExpr = function () {};
const arrow = () => {};
```

## Hoisting behavior

- function declarations are hoisted with definitions
- function expressions/arrow functions follow variable initialization timing

```javascript
declared(); // works

function declared() {}

// expr(); // ReferenceError if called before initialization
const expr = () => {};
```

## Parameter patterns

```javascript
function log(msg = "info") {}
function sum(...values) {}
function showUser({ name }) {}
```

## Callback and higher-order function pattern

```javascript
function withTiming(label, fn) {
  const start = performance.now();
  const result = fn();
  console.log(label, performance.now() - start);
  return result;
}
```

## Closure example

```javascript
function createCounter() {
  let count = 0;
  return () => ++count;
}
```

## `this` binding quick map

- regular function: `this` depends on call site
- arrow function: `this` is lexical (inherited from outer scope)

```javascript
const obj = {
  value: 10,
  regular() { return this.value; },
  arrow: () => this.value
};
```

Use arrow functions for callbacks, not object methods relying on dynamic `this`.

## Async function pattern

```javascript
async function load() {
  const response = await fetch("/api/data");
  if (!response.ok) throw new Error("failed");
  return response.json();
}
```

## Guidance

- Use declarations for main APIs.
- Use arrow functions for short callbacks.
- Use closures intentionally for encapsulated state.
- Keep function responsibilities narrow and names explicit.

---

[← JavaScript Reference](./README.md)
