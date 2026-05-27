<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 01](./README.md)

---

# JavaScript Files, Modules, and Execution Flow

> Understanding load order and module boundaries prevents hidden bugs later.

**You will learn:**
- How JavaScript execution starts from an entry script
- ES module basics
- How imports/exports shape project organization

**Before this page, you should know:** basic JavaScript syntax.

---

## Execution flow

JavaScript starts at the entry script and executes top-to-bottom.

```javascript
console.log("start");
console.log("end");
```

## Module pattern (ES modules)

`math.js`:

```javascript
export function add(a, b) {
  return a + b;
}
```

`app.js`:

```javascript
import { add } from "./math.js";

console.log(add(2, 3));
```

For browser usage, load module entry points with `type="module"` in HTML script tags.

## Visual model

```text
entry file
    |
    v
imports are resolved
    |
    v
module code is loaded
    |
    v
main logic runs
```

---

## Recap

- JavaScript executes entry scripts top-to-bottom.
- Modules keep code separated and reusable.
- ESM requires explicit module entry configuration in your environment.

## Try it yourself

Split one script into two modules and import one function into the entry file.

---

[**Next ->** Reading Errors and Warnings](./04-reading-errors-and-warnings.md)  
[**<- Previous** Your First JavaScript Program](./02-your-first-javascript-program.md)
