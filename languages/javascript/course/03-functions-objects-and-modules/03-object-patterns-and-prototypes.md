<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [JavaScript](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Object Patterns and Prototypes

> JavaScript objects are prototype-based, even when class syntax hides details.

**You will learn:**
- object literals and method patterns
- prototype chain basics
- different object and class patterns
- when to use classes versus factory functions

**Before this page, you should know:** function and scope fundamentals.

---

## Object literal pattern

```javascript
const user = {
  name: "Lee",
  points: 10,
  addPoints(amount) {
    this.points += amount;
  }
};
```

## Object categories you will use in frontend JavaScript

- plain data objects (state snapshots, API models)
- behavior objects (services/utilities)
- class instances (UI/domain entities)
- built-in objects (`Date`, `Map`, `Set`, `URL`, `Error`)

Choose based on behavior needs, not habit.

## Prototype concept

```javascript
const base = { kind: "base" };
const child = Object.create(base);
child.name = "child";

console.log(child.kind); // inherited from base
```

## Class syntax

```javascript
class Task {
  constructor(title) {
    this.title = title;
  }

  print() {
    console.log(this.title);
  }
}
```

Class methods still live on the prototype under the hood.

## Factory versus class

Factory function:

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

Class:

```javascript
class TaskModel {
  constructor(title) {
    this.title = title;
    this.done = false;
  }

  toggle() {
    this.done = !this.done;
  }
}
```

Use factories for simple closures/object construction, classes for shared behavior models with clear type identity.

## Inheritance versus composition

Prefer composition for frontend apps:
- small reusable objects/functions combined together
- lower coupling than deep class hierarchies

Inheritance is useful, but deep trees usually increase maintenance cost.

---

## Recap

- Object literals are simple and powerful.
- Prototypes power inheritance behavior.
- Classes are syntax over prototype mechanics.
- Composition-first design usually scales better in frontend code.

## Try it yourself

Create one factory function and one class that model the same entity, then compare clarity.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Scope, Closures, and Lexical Environments](./02-scope-closures-and-lexical-environments.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [ES Modules: import and export →](./04-es-modules-import-and-export.md) |

</div>
