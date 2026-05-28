# Functions and Closures Cheat Sheet

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for function forms, parameter patterns, callbacks, closures, pure functions, and `this`.

Course lessons:

- [Functions and Parameters in Depth](../course/03-functions-objects-and-modules/01-functions-and-parameters-in-depth.md)
- [Scope, Closures, and Lexical Environments](../course/03-functions-objects-and-modules/02-scope-closures-and-lexical-environments.md)

## Function Forms

| Form | Example | Use |
|---|---|---|
| declaration | `function save() {}` | named top-level behavior |
| expression | `const save = function () {};` | assign function to variable |
| arrow | `const save = () => {};` | short callbacks and lexical `this` |
| async | `async function load() {}` | promise-based async work |

## Parameter Patterns

| Pattern | Example | Use |
|---|---|---|
| default | `function log(msg, level = "info") {}` | optional value |
| rest | `function sum(...values) {}` | any number of values |
| destructured object | `function render({ title }) {}` | named options/data |
| callback | `items.map((item) => item.name)` | later/custom behavior |

## Pure Versus Side-Effect Functions

Pure:

```javascript
function countDone(tasks) {
  return tasks.filter((task) => task.done).length;
}
```

Side effect:

```javascript
function renderMessage(message) {
  document.querySelector("#message").textContent = message;
}
```

Keep pure logic separate from DOM, storage, network, and console effects.

## Closure Pattern

```javascript
function createValidator(minLength) {
  return function validate(value) {
    return value.trim().length >= minLength;
  };
}
```

The returned function remembers `minLength`.

## `this` Rules

| Function kind | `this` behavior |
|---|---|
| regular function call | depends on call site |
| object method | receiver before dot |
| arrow function | inherited from outer scope |
| class method | instance when called as `instance.method()` |

Avoid arrow methods when the method needs the object as `this`.

## Risk Notes

| Pattern | Risk |
|---|---|
| too many positional params | hard to call correctly |
| side effects inside data functions | hard to test |
| empty callbacks/catches | hides behavior |
| arrow function as object method | wrong `this` |
| closure over large data | can keep memory alive |

## Cross References

- [Arrays and Objects Patterns](./arrays-and-objects-patterns.md)
- [DOM and Events Patterns](./dom-and-events-patterns.md)
- [Async and Promise Patterns](./async-and-promise-patterns.md)
