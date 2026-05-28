<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# Scope, Closures, and Lexical Environments

> Closures explain why JavaScript functions can remember values after the outer function has finished.

**You will learn:**
- block, function, module, and global scope
- what lexical scope means
- how closures work in real UI code
- why `let` fixes old `var` loop bugs
- how closures help with event handlers, validators, and private state

**Before this page, you should know:** [Functions and Parameters in Depth](./01-functions-and-parameters-in-depth.md)

---

## What Scope Means

Scope is the area of code where a name can be used.

```javascript
const appName = "Task Tracker";

function startApp() {
  const status = "ready";
  console.log(appName);
  console.log(status);
}

startApp();
// console.log(status); // ReferenceError
```

`appName` is outside the function, so the function can read it. `status` is
inside the function, so outside code cannot read it.

## Block Scope

`let` and `const` are block-scoped. A block is code inside `{}`.

```javascript
if (true) {
  const message = "inside";
  console.log(message);
}

// console.log(message); // ReferenceError
```

This keeps temporary names from leaking into the rest of the file.

## Module Scope

Each ES module has its own top-level scope.

```javascript
// state.js
let tasks = [];

export function getTasks() {
  return tasks;
}
```

`tasks` is not global. Other modules can only access what you export.

## Lexical Scope

Lexical scope means functions remember where they were written, not where they
are called.

```javascript
function createLogger(prefix) {
  return function log(message) {
    console.log(`${prefix}: ${message}`);
  };
}

const logError = createLogger("ERROR");
logError("Could not save task");
```

The inner `log` function can still use `prefix` because it was created inside
`createLogger`.

## Closure Visual Model

```text
createLogger("ERROR") runs
    |
    |-- prefix = "ERROR"
    |
    `-- returns log()
            |
            `-- log() keeps access to prefix later
```

A closure is a function plus the variables from its lexical environment that it
continues to use.

## UI Example: Event Handler Remembers State

```javascript
function setupCounter(button, output) {
  let count = 0;

  button.addEventListener("click", () => {
    count += 1;
    output.textContent = `Clicked ${count} times`;
  });
}
```

The click handler runs later, after `setupCounter` has finished. It still
remembers `count`.

This is a closure doing useful frontend work.

## UI Example: Create a Validator

```javascript
function createLengthValidator(minLength) {
  return function validate(value) {
    return value.trim().length >= minLength;
  };
}

const isValidTaskTitle = createLengthValidator(3);

console.log(isValidTaskTitle("Hi")); // false
console.log(isValidTaskTitle("DOM")); // true
```

The returned function remembers `minLength`.

## The Old `var` Loop Problem

`var` is function-scoped, not block-scoped.

```javascript
for (var i = 0; i < 3; i += 1) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
```

This logs `3`, `3`, `3` because every callback shares the same `i`.

Use `let`:

```javascript
for (let i = 0; i < 3; i += 1) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
```

This logs `0`, `1`, `2` because each loop iteration gets its own `i`.

## Private Module State

Closures and module scope can protect state.

```javascript
let tasks = [];

export function addTask(title) {
  tasks = [...tasks, { id: crypto.randomUUID(), title, done: false }];
  return tasks;
}

export function getTasks() {
  return tasks;
}
```

Other modules cannot directly reassign `tasks`. They must use exported
functions. That is a useful boundary in browser apps.

## Closure Risks

Closures can also keep old values alive longer than expected.

```javascript
function attachHandler(element, largeData) {
  element.addEventListener("click", () => {
    console.log(largeData.length);
  });
}
```

The handler keeps access to `largeData`. That is fine when intentional, but
long-lived handlers can keep memory around. Remove event listeners when
components/pages are destroyed in larger apps.

## Reference Links

- [Functions and Closures Cheat Sheet](../../reference/functions-and-closures-cheat-sheet.md)
- [DOM and Events Patterns](../../reference/dom-and-events-patterns.md)

---

## Recap

- Scope controls where names are visible.
- Closures let functions remember values from where they were created.
- Event handlers often use closures.
- `let` and `const` avoid old `var` loop-scope bugs.
- Module scope is an architecture boundary.

## Try It Yourself

Build `setupToggle(button, panel)` so the click handler remembers whether the
panel is open. Each click should flip the state and update the panel text.

---

[**Next ->** Objects, Classes, and Prototypes](./03-object-patterns-and-prototypes.md)  
[**<- Previous** Functions and Parameters in Depth](./01-functions-and-parameters-in-depth.md)
