# Objects, Classes, and Prototypes Deep Dive

## Object categories in frontend JavaScript

- plain state objects: `const state = { ... }`
- service objects: methods around API/storage logic
- class instances: richer domain entities
- built-in objects: `Date`, `Map`, `Set`, `URL`, `Error`

## Prototype chain essentials

```javascript
const base = { type: "base" };
const child = Object.create(base);
child.name = "child";

console.log(child.type); // inherited
```

Lookups walk upward through prototype chain until match or `null`.

## Class syntax and prototype reality

```javascript
class Task {
  constructor(title) {
    this.title = title;
  }

  rename(nextTitle) {
    this.title = nextTitle;
  }
}
```

Class methods live on `Task.prototype`, not duplicated per instance.

## Factory functions

```javascript
function createTask(title) {
  return {
    title,
    done: false,
    toggle() {
      this.done = !this.done;
    }
  };
}
```

Factories are straightforward and often ideal for small modules.

## Composition versus inheritance

Prefer composition in most frontend apps:
- combine small focused modules/functions
- avoid deep inheritance trees
- easier testing and refactoring

Use inheritance only when true "is-a" hierarchy stays clear and shallow.

## Mutability guidelines

- keep shared state changes explicit
- prefer immutable updates for app state snapshots
- avoid hidden side effects in object methods that touch global state

---

[← JavaScript Reference](./README.md)
