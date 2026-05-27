<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# ES Modules: import and export

> Module boundaries are the first architecture layer in JavaScript projects.

**You will learn:**
- named and default exports
- import syntax variants
- practical module organization habits

**Before this page, you should know:** object and function design basics.

---

## Named exports

`math.js`:

```javascript
export function add(a, b) {
  return a + b;
}

export const PI = 3.14159;
```

`app.js`:

```javascript
import { add, PI } from "./math.js";
console.log(add(2, 3), PI);
```

## Default export

```javascript
export default function log(message) {
  console.log(message);
}
```

Import:

```javascript
import log from "./logger.js";
```

## Organization guidance

- Keep modules focused by responsibility.
- Avoid circular imports.
- Keep import paths explicit and local.

---

## Recap

- Named exports are explicit and scalable.
- Default exports are useful for one-main-value modules.
- Strong module boundaries simplify testing and maintenance.

## Try it yourself

Split one utility file into two modules and wire imports from an entry file.

---

[**Next ->** Chapter 03 Checkpoint](./05-chapter-03-checkpoint.md)  
[**<- Previous** Objects, Classes, and Prototypes](./03-object-patterns-and-prototypes.md)
