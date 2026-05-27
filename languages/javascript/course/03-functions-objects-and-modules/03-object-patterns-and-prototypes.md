<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# Objects, Classes, and Prototypes

> JavaScript has class syntax, but its object system is still based on prototypes.

**You will learn:**
- the major object types you will use in browser JavaScript
- how plain objects, arrays, functions, class instances, maps, sets, dates, errors, and DOM objects differ
- how prototypes explain shared methods like `array.map()` and `task.toggle()`
- how to write classes, private fields, static methods, and factory functions
- when to prefer composition over inheritance in frontend code

**Before this page, you should know:**
- [Functions and Parameters in Depth](./01-functions-and-parameters-in-depth.md)
- [Scope, Closures, and Lexical Environments](./02-scope-closures-and-lexical-environments.md)

---

## The Big Idea

An object is a value that can hold properties. A property is a named slot that
points to another value.

```javascript
const user = {
  id: 1,
  name: "Lee",
  role: "editor"
};
```

In frontend code, objects usually represent state, configuration, API data,
services, errors, DOM elements, or domain concepts such as tasks and users.

## Object Types You Must Recognize

| Type/category | Example | Use for |
|---|---|---|
| plain object | `{ title: "Learn DOM" }` | state, config, API data |
| array | `["a", "b"]` | ordered lists |
| function object | `function save() {}` | reusable behavior |
| class instance | `new Task("Study")` | data plus shared methods |
| built-in object | `new Date()`, `new URL()` | platform-provided models |
| `Map` | `new Map()` | key/value data with non-string keys |
| `Set` | `new Set()` | unique values |
| `Error` | `new Error("Failed")` | failure information |
| DOM object | `document.querySelector("button")` | live page nodes |
| `Promise` | `fetch("/api")` | future async result |

Do not treat all objects the same. A DOM element is not just a plain state
object. A `Promise` is not the final data. A class instance can have methods
that plain JSON from an API does not have.

## Plain Objects for State

Plain objects are best for serializable data: data you can save as JSON, send to
an API, or use as UI state.

```javascript
const task = {
  id: crypto.randomUUID(),
  title: "Build task renderer",
  done: false
};
```

Plain objects are easy to inspect and copy:

```javascript
const completedTask = {
  ...task,
  done: true
};
```

Use plain objects for most frontend state.

## Behavior Objects

A behavior object groups related functions.

```javascript
const taskStorage = {
  key: "tasks",

  load() {
    return JSON.parse(localStorage.getItem(this.key) ?? "[]");
  },

  save(tasks) {
    localStorage.setItem(this.key, JSON.stringify(tasks));
  }
};
```

This is useful for services, but be careful with `this`. If you pass
`taskStorage.load` as a callback, it can lose its receiver.

Safer alternative:

```javascript
const taskStorage = {
  loadTasks() {
    return JSON.parse(localStorage.getItem("tasks") ?? "[]");
  },

  saveTasks(tasks) {
    localStorage.setItem("tasks", JSON.stringify(tasks));
  }
};
```

## The Prototype Chain

JavaScript object lookup follows a chain.

```text
task object
    |
    v
Task.prototype
    |
    v
Object.prototype
    |
    v
null
```

When you read `task.toggle`, JavaScript first checks the object itself. If the
property is not there, it checks the prototype, then the next prototype, until it
finds the property or reaches `null`.

Small proof:

```javascript
const base = {
  describe() {
    return "from base";
  }
};

const child = Object.create(base);
child.name = "child";

console.log(child.name); // "child"
console.log(child.describe()); // "from base"
```

`describe` is not stored directly on `child`; it is found through the prototype.

## Why Array Methods Exist

Arrays have methods like `map`, `filter`, and `pop` because arrays inherit from
`Array.prototype`.

```javascript
const names = ["Ada", "Grace"];

console.log(names.hasOwnProperty("map")); // false
console.log(typeof names.map); // "function"
```

The array does not own `map`; JavaScript finds it through the prototype chain.

## Class Syntax

Classes are the modern readable way to create objects with shared behavior.

```javascript
class Task {
  constructor(title) {
    this.title = title;
    this.done = false;
  }

  toggle() {
    this.done = !this.done;
  }

  label() {
    return `${this.done ? "[x]" : "[ ]"} ${this.title}`;
  }
}

const task = new Task("Learn classes");
task.toggle();
console.log(task.label()); // "[x] Learn classes"
```

`constructor` runs when you call `new Task(...)`. Methods like `toggle` and
`label` are shared through `Task.prototype`, not copied into every instance.

## Public Fields, Private Fields, and Static Methods

Class fields can define per-instance data.

```javascript
class Counter {
  count = 0;

  increment() {
    this.count += 1;
  }
}
```

Private fields start with `#` and can only be used inside the class body.

```javascript
class Timer {
  #startedAt = Date.now();

  elapsedMs() {
    return Date.now() - this.#startedAt;
  }
}
```

Static methods belong to the class itself, not instances.

```javascript
class Task {
  static fromText(text) {
    return new Task(text.trim());
  }

  constructor(title) {
    this.title = title;
  }
}

const task = Task.fromText("  Clean up imports  ");
```

Use static methods for constructors with names, parsing helpers, or factory-like
operations that belong near the class.

## Inheritance with `extends`

Inheritance means one class builds on another class.

```javascript
class Notification {
  constructor(message) {
    this.message = message;
  }

  renderText() {
    return this.message;
  }
}

class ErrorNotification extends Notification {
  renderText() {
    return `Error: ${this.message}`;
  }
}
```

Use inheritance only when the relationship is truly "is a." An error
notification is a notification. A task list is not a task.

## Factory Functions

A factory function returns a new object.

```javascript
function createTask(title) {
  let done = false;

  return {
    title,
    toggle() {
      done = !done;
    },
    label() {
      return `${done ? "[x]" : "[ ]"} ${title}`;
    }
  };
}
```

Factories are useful when you want closure-private data without class syntax.
The tradeoff is that methods are often recreated for each object unless you
intentionally share them.

## Composition First

Composition means building larger behavior by combining small pieces.

```javascript
function createSelectable(item) {
  return {
    ...item,
    selected: false,
    toggleSelected() {
      this.selected = !this.selected;
    }
  };
}

function createRenderable(item) {
  return {
    ...item,
    renderLabel() {
      return `${this.selected ? ">" : " "} ${this.title}`;
    }
  };
}
```

Frontend apps usually scale better with composition than deep inheritance trees.
Small data transformations, render functions, and services are easier to test.

## DOM Objects Are Objects Too

When you select an element, the browser gives you an object with properties and
methods.

```javascript
const button = document.querySelector("button");

button.textContent = "Save";
button.addEventListener("click", () => {
  console.log("clicked");
});
```

The button is not a plain object. It is a browser-provided object with a long
prototype chain: `HTMLButtonElement`, `HTMLElement`, `Element`, `Node`, and more.

That is why DOM objects have methods such as `append`, `remove`,
`addEventListener`, and `querySelector`.

## Choosing the Right Pattern

| Need | Good default |
|---|---|
| API response data | plain object |
| UI state snapshot | plain object or array |
| reusable domain model | class or factory |
| browser element | DOM object from `document` |
| unique list of values | `Set` |
| key/value lookup | `Map` or plain object |
| grouped utility behavior | module functions or service object |

---

## Recap

- JavaScript objects use prototype-based lookup.
- Classes are syntax for creating objects with shared prototype methods.
- Plain objects are best for state and API data.
- DOM elements are browser objects, not plain JSON.
- Use inheritance sparingly; prefer composition for frontend app structure.

## Try It Yourself

Model a `Task` three ways:

- as a plain object plus standalone functions
- as a factory function
- as a class with `toggle()` and `label()`

Then explain which version you would use in a browser task tracker and why.

---

[**Next ->** ES Modules: import and export](./04-es-modules-import-and-export.md)  
[**<- Previous** Scope, Closures, and Lexical Environments](./02-scope-closures-and-lexical-environments.md)
