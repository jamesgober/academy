<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 03](./README.md)

---

# ES Modules: import and export

> Modules are how browser JavaScript grows from one file into a maintainable app.

**You will learn:**
- named exports and default exports
- browser module paths and file extensions
- barrel files and when to avoid them
- import errors and how to debug them
- a practical module layout for frontend projects

**Before this page, you should know:** [Objects, Classes, and Prototypes](./03-object-patterns-and-prototypes.md)

---

## Use a Module Script

Browser ES modules require `type="module"`.

```html
<script type="module" src="./src/app.js"></script>
```

Module scripts are deferred by default, which means they run after the HTML is
parsed. They also run in strict mode by default.

## Named Exports

Use named exports when a module provides multiple clear values.

`src/math.js`:

```javascript
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export const PI = 3.14159;
```

`src/app.js`:

```javascript
import { add, PI } from "./math.js";

console.log(add(2, 3), PI);
```

Named exports scale well because imports show exactly which names a file uses.

## Rename Imports

```javascript
import { add as addNumbers } from "./math.js";
```

Use aliases when two modules export the same name or when a clearer local name
helps readability.

## Default Exports

A default export is the module's main value.

`src/logger.js`:

```javascript
export default function log(message) {
  console.log(`[app] ${message}`);
}
```

`src/app.js`:

```javascript
import log from "./logger.js";

log("started");
```

Default imports can be renamed by the importer. That can be convenient, but it
can also make large codebases less consistent. Prefer named exports unless the
module truly has one obvious primary value.

## Export Lists

```javascript
function saveTasks(tasks) {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function loadTasks() {
  return JSON.parse(localStorage.getItem("tasks") ?? "[]");
}

export { loadTasks, saveTasks };
```

This style keeps exports visible at the bottom of the file.

## Browser Import Paths

In browser modules, local imports need relative paths and file extensions.

```javascript
import { renderTasks } from "./render.js";
import { loadTasks } from "../storage/tasks.js";
```

Do not write this in plain browser modules:

```javascript
import { renderTasks } from "render";
```

Bare specifiers like `"react"` or `"lodash"` need a bundler, import map, or
runtime that knows how to resolve them.

## Practical Folder Structure

```text
src/
|-- app.js
|-- state.js
|-- render.js
|-- events.js
`-- storage.js
```

`app.js`:

```javascript
import { createInitialState } from "./state.js";
import { renderApp } from "./render.js";
import { connectEvents } from "./events.js";

let state = createInitialState();

function setState(nextState) {
  state = nextState;
  renderApp(state);
}

renderApp(state);
connectEvents({ getState: () => state, setState });
```

This keeps startup, state, rendering, and events separate.

## Barrel Files

A barrel file re-exports from several files.

`src/index.js`:

```javascript
export { createTask } from "./state.js";
export { renderApp } from "./render.js";
export { connectEvents } from "./events.js";
```

Barrels can make public APIs tidy. They can also hide where code comes from and
create circular import problems. Use them for package boundaries, not as a habit
inside every folder.

## Common Import Errors

### Missing file extension

```text
Failed to resolve module specifier "./state"
```

Fix:

```javascript
import { createTask } from "./state.js";
```

### Wrong relative path

```text
Failed to fetch dynamically imported module
```

Check where the current file lives. `./` starts from the current file's folder.
`../` moves up one folder.

### Export name mismatch

```text
The requested module './state.js' does not provide an export named 'addTask'
```

Fix the import or export spelling:

```javascript
export function addTask(tasks, title) {
  // ...
}
```

### CORS or `file://` module trouble

If module loading behaves strangely from `file://`, use a local server:

```bash
npx serve .
```

## Circular Imports

A circular import means module A imports module B while module B imports module A.

```text
app.js -> state.js -> app.js
```

Avoid this by moving shared logic into a third module:

```text
app.js ----\
           -> taskModel.js
state.js --/
```

## Reference Links

- [Commands and Tooling](../../reference/commands-and-tooling.md)
- [Functions and Closures Cheat Sheet](../../reference/functions-and-closures-cheat-sheet.md)

---

## Recap

- Browser modules use `type="module"`.
- Prefer named exports for most shared functions.
- Browser relative imports need file extensions.
- Barrel files are useful at boundaries but risky when overused.
- Import errors usually come from path mistakes, missing extensions, or export name mismatches.

## Try It Yourself

Split a task app into `state.js`, `render.js`, and `app.js`. Export pure state
functions from `state.js`, export DOM rendering from `render.js`, and wire both
from `app.js`.

---

[**Next ->** Chapter 03 Checkpoint](./05-chapter-03-checkpoint.md)  
[**<- Previous** Objects, Classes, and Prototypes](./03-object-patterns-and-prototypes.md)
