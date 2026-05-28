<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# Functions and Parameters in Depth

> Functions are the main way JavaScript code becomes reusable, testable, and understandable.

**You will learn:**
- function declarations, expressions, and arrow functions
- default, rest, and destructured parameters
- callbacks and higher-order functions
- pure functions and side effects
- how arrow functions handle `this`
- how these choices affect DOM code

**Before this page, you should know:** [Arrays, Objects, and Common Mutations](../02-core-language-basics/05-arrays-objects-and-common-mutations.md)

---

## Function Forms

Function declaration:

```javascript
function add(a, b) {
  return a + b;
}
```

Function expression:

```javascript
const multiply = function (a, b) {
  return a * b;
};
```

Arrow function:

```javascript
const subtract = (a, b) => a - b;
```

Use declarations for named top-level behavior. Use arrow functions for short
callbacks and small local helpers.

## Parameters and Arguments

A parameter is the name in the function definition. An argument is the actual
value passed during a call.

```javascript
function greet(name) {
  return `Hello, ${name}`;
}

greet("Maya");
```

`name` is the parameter. `"Maya"` is the argument.

## Default Parameters

```javascript
function createTask(title, done = false) {
  return { id: crypto.randomUUID(), title, done };
}
```

Default parameters are used when the caller passes `undefined` or omits the
argument.

```javascript
createTask("Learn DOM");
createTask("Ship project", true);
```

## Rest Parameters

Rest parameters gather extra arguments into an array.

```javascript
function sum(...values) {
  return values.reduce((total, value) => total + value, 0);
}

console.log(sum(1, 2, 3)); // 6
```

Use rest parameters when a function accepts any number of values.

## Destructured Parameters

Destructuring makes object-shaped inputs clear.

```javascript
function renderTaskLabel({ title, done }) {
  return `${done ? "[x]" : "[ ]"} ${title}`;
}
```

With defaults:

```javascript
function createButton({ text, type = "button", disabled = false }) {
  const button = document.createElement("button");
  button.type = type;
  button.disabled = disabled;
  button.textContent = text;
  return button;
}
```

Use object parameters when a function has several options or boolean flags.

## Return Values

Prefer returning a value when a function calculates something.

```javascript
function toggleTask(tasks, id) {
  return tasks.map((task) => {
    if (task.id !== id) return task;
    return { ...task, done: !task.done };
  });
}
```

This is easier to test than a function that mutates a global array and updates
the DOM at the same time.

## Side Effects

A side effect is when a function changes something outside itself: the DOM,
storage, network, console, timers, or outer variables.

Pure function:

```javascript
function countCompleted(tasks) {
  return tasks.filter((task) => task.done).length;
}
```

Side-effect function:

```javascript
function showMessage(message) {
  document.querySelector("#message").textContent = message;
}
```

Both are useful. Keep them separate so pure logic is easy to test.

## Callbacks

A callback is a function passed to another function.

```javascript
const openTasks = tasks.filter((task) => !task.done);
```

The arrow function is a callback passed to `filter`.

DOM callbacks:

```javascript
button.addEventListener("click", () => {
  console.log("button clicked");
});
```

Callbacks let code run later, once an array method, timer, event, or async
operation decides it is time.

## Higher-Order Functions

A higher-order function accepts a function, returns a function, or both.

```javascript
function createMinimumLengthValidator(minLength) {
  return function validate(value) {
    return value.trim().length >= minLength;
  };
}

const isValidTitle = createMinimumLengthValidator(3);
console.log(isValidTitle("DOM")); // true
```

This pattern is common in validation, event handling, and reusable UI behavior.

## Arrow Functions and `this`

Arrow functions do not create their own `this`. They capture `this` from the
surrounding scope.

Regular method:

```javascript
const counter = {
  count: 0,
  increment() {
    this.count += 1;
  }
};
```

Do not write object methods as arrows when they need `this`:

```javascript
const brokenCounter = {
  count: 0,
  increment: () => {
    // `this` is not brokenCounter here.
  }
};
```

Use arrow functions for callbacks where lexical `this` is useful, not for object
methods that rely on the receiver.

## Real UI Pattern

Separate state update from render:

```javascript
function addTask(tasks, title) {
  return [
    ...tasks,
    {
      id: crypto.randomUUID(),
      title,
      done: false
    }
  ];
}

function handleSubmit(event) {
  event.preventDefault();
  tasks = addTask(tasks, titleInput.value.trim());
  renderTasks(tasks);
}
```

`addTask` is pure. `handleSubmit` has side effects because it reads the event,
changes module state, and renders.

## Reference Links

- [Functions and Closures Cheat Sheet](../../reference/functions-and-closures-cheat-sheet.md)
- [Arrays and Objects Patterns](../../reference/arrays-and-objects-patterns.md)

---

## Recap

- Use clear function forms for clear jobs.
- Default, rest, and destructured parameters make calling code easier to read.
- Callbacks power array methods, DOM events, timers, and async workflows.
- Pure functions are easier to test than side-effect-heavy functions.
- Arrow functions do not have their own `this`.

## Try It Yourself

Write pure functions for a task app:

- `createTask(title)`
- `addTask(tasks, title)`
- `toggleTask(tasks, id)`
- `countCompleted(tasks)`

Then write one side-effect function that renders the count into the DOM.

---

[**Next ->** Scope, Closures, and Lexical Environments](./02-scope-closures-and-lexical-environments.md)  
[**<- Previous** Chapter Functions, Objects, and Modules](./README.md)
