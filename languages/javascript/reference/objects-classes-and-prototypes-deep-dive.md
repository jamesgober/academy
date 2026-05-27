# Objects, Classes, and Prototypes Deep Dive

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for JavaScript object categories, class syntax, prototype lookup, factories, and composition.

Course lesson: [Objects, Classes, and Prototypes](../course/03-functions-objects-and-modules/03-object-patterns-and-prototypes.md).

## Object Categories

| Category | Example | Description | Use for |
|---|---|---|---|
| plain object | `{ id: 1 }` | key/value record | state, config, JSON-like data |
| array | `[1, 2, 3]` | ordered object with array methods | lists |
| function | `function save() {}` | callable object | behavior |
| class instance | `new Task("A")` | object created by class | data plus shared methods |
| `Map` | `new Map()` | key/value collection | non-string keys, frequent add/remove |
| `Set` | `new Set()` | unique-value collection | dedupe, membership |
| `Date` | `new Date()` | date/time object | timestamps and formatting inputs |
| `URL` | `new URL(location.href)` | URL parser/model | query params and links |
| `Error` | `new Error("Bad")` | failure object | throwing/catching errors |
| DOM object | `document.body` | browser-provided object | live page interaction |
| `Promise` | `fetch(url)` | future async result | async work |

## Prototype Lookup

```text
object itself
    |
    v
object prototype
    |
    v
next prototype
    |
    v
null
```

If a property is not found on the object, JavaScript checks the prototype chain.

```javascript
const base = { role: "reader" };
const user = Object.create(base);
user.name = "Ada";

console.log(user.name); // own property
console.log(user.role); // inherited property
```

## Class Syntax

```javascript
class Task {
  constructor(title) {
    this.title = title;
    this.done = false;
  }

  toggle() {
    this.done = !this.done;
  }
}
```

| Part | Meaning |
|---|---|
| `class Task` | declares a class |
| `constructor(...)` | runs when `new Task(...)` is called |
| `this.title` | instance property |
| `toggle()` | method stored on `Task.prototype` |
| `new Task("x")` | creates an instance |

## Class Features

Public field:

```javascript
class Counter {
  count = 0;
}
```

Private field:

```javascript
class Counter {
  #count = 0;

  value() {
    return this.#count;
  }
}
```

Static method:

```javascript
class Task {
  static fromJSON(raw) {
    return new Task(raw.title);
  }

  constructor(title) {
    this.title = title;
  }
}
```

Inheritance:

```javascript
class AdminUser extends User {
  canDelete() {
    return true;
  }
}
```

Use inheritance sparingly. Favor composition unless the relationship is truly
"is a" and stays shallow.

## Factory Function

```javascript
function createTask(title) {
  let done = false;

  return {
    title,
    toggle() {
      done = !done;
    },
    isDone() {
      return done;
    }
  };
}
```

Factories are simple and can use closure-private data. Classes are clearer when
you want shared prototype methods and recognizable type identity.

## Choosing a Pattern

| Need | Prefer |
|---|---|
| serializable app state | plain object/array |
| model with shared methods | class |
| closure-private data | factory |
| utility behavior | named functions or module |
| unique values | `Set` |
| key/value lookup where keys are objects | `Map` |
| DOM updates | DOM element objects from browser APIs |

## Risk Notes

| Pattern | Risk |
|---|---|
| deep inheritance | hard to reason about and test |
| mutating shared objects | surprising UI bugs |
| relying on `this` in callbacks | receiver may be lost |
| using class instances for API JSON | parsed JSON does not automatically regain methods |
| changing built-in prototypes | can break other code |

## Cross References

- [Arrays and Objects Patterns](./arrays-and-objects-patterns.md)
- [DOM and Events Patterns](./dom-and-events-patterns.md)
- [Functions and Closures Cheat Sheet](./functions-and-closures-cheat-sheet.md)
