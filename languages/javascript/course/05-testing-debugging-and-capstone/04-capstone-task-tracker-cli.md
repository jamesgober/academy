<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 05](./README.md)

---

# Capstone: Task Tracker Web App

> Build a real browser app using modules, state, DOM rendering, events, storage, validation, and tests.

**You will learn:**
- practical frontend project structure
- how state, render, events, and storage work together
- how to build a complete vertical slice before adding features
- how to test the logic that powers a DOM app

**Before this page, you should know:** all previous JavaScript chapters.

---

## Final Project Structure

```text
task-tracker/
|-- index.html
|-- styles.css
|-- src/
|   |-- app.js
|   |-- state.js
|   |-- render.js
|   |-- events.js
|   `-- storage.js
`-- tests/
    `-- state.test.js
```

## Step 1: HTML Shell

`index.html`:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Task Tracker</title>
    <link rel="stylesheet" href="./styles.css">
    <script type="module" src="./src/app.js"></script>
  </head>
  <body>
    <main>
      <h1>Task Tracker</h1>

      <form id="task-form">
        <label>
          Task
          <input id="task-title" name="title" autocomplete="off">
        </label>
        <button type="submit">Add</button>
      </form>

      <p id="message" role="status"></p>

      <div>
        <button type="button" data-filter="all">All</button>
        <button type="button" data-filter="open">Open</button>
        <button type="button" data-filter="done">Done</button>
      </div>

      <p id="summary"></p>
      <ul id="task-list"></ul>
    </main>
  </body>
</html>
```

## Step 2: State Logic

`src/state.js`:

```javascript
export function createTask(title) {
  return {
    id: crypto.randomUUID(),
    title,
    done: false
  };
}

export function addTask(state, title) {
  const cleanTitle = title.trim();

  if (cleanTitle === "") {
    return {
      ...state,
      message: "Enter a task title."
    };
  }

  return {
    ...state,
    tasks: [...state.tasks, createTask(cleanTitle)],
    message: "Task added."
  };
}

export function toggleTask(state, id) {
  return {
    ...state,
    tasks: state.tasks.map((task) => {
      if (task.id !== id) return task;
      return { ...task, done: !task.done };
    }),
    message: "Task updated."
  };
}

export function removeTask(state, id) {
  return {
    ...state,
    tasks: state.tasks.filter((task) => task.id !== id),
    message: "Task removed."
  };
}

export function setFilter(state, filter) {
  return {
    ...state,
    filter
  };
}

export function getVisibleTasks(state) {
  if (state.filter === "open") {
    return state.tasks.filter((task) => !task.done);
  }

  if (state.filter === "done") {
    return state.tasks.filter((task) => task.done);
  }

  return state.tasks;
}

export function getSummary(tasks) {
  const done = tasks.filter((task) => task.done).length;
  return `${done} of ${tasks.length} complete`;
}
```

This file has pure state functions. It does not touch the DOM.

## Step 3: Storage

`src/storage.js`:

```javascript
const STORAGE_KEY = "academy.task-tracker.tasks";

export function saveTasks(tasks, storage = localStorage) {
  storage.setItem(STORAGE_KEY, JSON.stringify(tasks));
}

export function loadTasks(storage = localStorage) {
  const raw = storage.getItem(STORAGE_KEY);

  if (raw === null) {
    return [];
  }

  try {
    const parsed = JSON.parse(raw);
    return Array.isArray(parsed) ? parsed : [];
  } catch {
    return [];
  }
}
```

Storage is isolated so failure cases are easy to test.

## Step 4: Rendering

`src/render.js`:

```javascript
import { getSummary, getVisibleTasks } from "./state.js";

function createTaskItem(task) {
  const item = document.createElement("li");

  const checkbox = document.createElement("input");
  checkbox.type = "checkbox";
  checkbox.checked = task.done;
  checkbox.dataset.action = "toggle";
  checkbox.dataset.id = task.id;

  const title = document.createElement("span");
  title.textContent = task.title;

  const removeButton = document.createElement("button");
  removeButton.type = "button";
  removeButton.textContent = "Remove";
  removeButton.dataset.action = "remove";
  removeButton.dataset.id = task.id;

  item.append(checkbox, " ", title, " ", removeButton);
  return item;
}

export function renderApp(state, elements) {
  elements.message.textContent = state.message;
  elements.summary.textContent = getSummary(state.tasks);

  const visibleTasks = getVisibleTasks(state);
  const items = visibleTasks.map(createTaskItem);

  if (items.length === 0) {
    const empty = document.createElement("li");
    empty.textContent = "No tasks to show.";
    elements.taskList.replaceChildren(empty);
    return;
  }

  elements.taskList.replaceChildren(...items);
}
```

Rendering uses DOM APIs but does not decide business rules.

## Step 5: Events

`src/events.js`:

```javascript
import { addTask, removeTask, setFilter, toggleTask } from "./state.js";

export function connectEvents({ elements, getState, setState }) {
  elements.form.addEventListener("submit", (event) => {
    event.preventDefault();

    const formData = new FormData(elements.form);
    const title = String(formData.get("title") ?? "");

    setState(addTask(getState(), title));
    elements.titleInput.value = "";
    elements.titleInput.focus();
  });

  elements.taskList.addEventListener("change", (event) => {
    const target = event.target;

    if (!(target instanceof HTMLInputElement)) return;
    if (target.dataset.action !== "toggle") return;

    setState(toggleTask(getState(), target.dataset.id));
  });

  elements.taskList.addEventListener("click", (event) => {
    const button = event.target.closest("button[data-action='remove']");
    if (!button) return;

    setState(removeTask(getState(), button.dataset.id));
  });

  document.addEventListener("click", (event) => {
    const button = event.target.closest("button[data-filter]");
    if (!button) return;

    setState(setFilter(getState(), button.dataset.filter));
  });
}
```

Event handlers read user actions, call state functions, and request a re-render.

## Step 6: App Entry

`src/app.js`:

```javascript
import { connectEvents } from "./events.js";
import { renderApp } from "./render.js";
import { loadTasks, saveTasks } from "./storage.js";

function getRequiredElement(selector) {
  const element = document.querySelector(selector);

  if (!element) {
    throw new Error(`Missing element: ${selector}`);
  }

  return element;
}

const elements = {
  form: getRequiredElement("#task-form"),
  titleInput: getRequiredElement("#task-title"),
  message: getRequiredElement("#message"),
  summary: getRequiredElement("#summary"),
  taskList: getRequiredElement("#task-list")
};

let state = {
  tasks: loadTasks(),
  filter: "all",
  message: ""
};

function getState() {
  return state;
}

function setState(nextState) {
  state = nextState;
  saveTasks(state.tasks);
  renderApp(state, elements);
}

renderApp(state, elements);
connectEvents({ elements, getState, setState });
```

The app entry owns the current state and connects modules together.

## Step 7: Minimal CSS

`styles.css`:

```css
body {
  font-family: system-ui, sans-serif;
  line-height: 1.5;
  margin: 2rem;
}

main {
  max-width: 42rem;
}

li {
  margin: 0.5rem 0;
}
```

Keep CSS minimal while learning behavior. Styling can improve later.

## Step 8: Pure Logic Tests

`tests/state.test.js`:

```javascript
import { addTask, getVisibleTasks, toggleTask } from "../src/state.js";

function assertEqual(actual, expected, message) {
  if (actual !== expected) {
    throw new Error(`${message}: expected ${expected}, got ${actual}`);
  }
}

let state = {
  tasks: [],
  filter: "all",
  message: ""
};

state = addTask(state, "Write tests");
assertEqual(state.tasks.length, 1, "adds task");
assertEqual(state.tasks[0].done, false, "task starts open");

state = toggleTask(state, state.tasks[0].id);
assertEqual(state.tasks[0].done, true, "toggles task");

state = { ...state, filter: "done" };
assertEqual(getVisibleTasks(state).length, 1, "filters done tasks");
```

Run with a browser-compatible test runner later, or adapt this to Node after
mocking browser-only APIs such as `crypto.randomUUID`.

## Completion Checklist

- Add task works.
- Empty task shows a message.
- Toggle works.
- Remove works.
- Filters work.
- Summary updates.
- Reload restores tasks from `localStorage`.
- DOM rendering uses `textContent`, not user-controlled `innerHTML`.
- State logic is separate from DOM logic.
- At least state functions have tests.

## Extension Ideas

- Add edit task title.
- Add created date.
- Add search.
- Add clear completed.
- Add import/export JSON.
- Add a worker to compute expensive statistics if the task list becomes huge.

---

## Recap

- A complete frontend app needs structure, not just snippets.
- Keep pure state logic separate from rendering and events.
- Storage should be isolated and defensive.
- The capstone combines arrays, objects, modules, DOM, events, storage, and tests.

## Try It Yourself

Build the app file by file. After each step, reload the browser and verify one
visible behavior before moving on.

---

[**Next ->** Chapter 05 Final Checkpoint](./05-chapter-05-final-checkpoint.md)  
[**<- Previous** Linting, Formatting, and Quality Gates](./03-linting-formatting-and-quality-gates.md)
