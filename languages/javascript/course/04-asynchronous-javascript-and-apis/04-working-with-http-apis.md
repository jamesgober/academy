<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 04](./README.md)

---

# DOM, Events, Rendering, and API Data

> Browser JavaScript connects state, user events, DOM updates, and async data into one interaction loop.

**You will learn:**
- what the DOM is and how JavaScript sees a page
- how to query, create, update, append, and remove elements
- how event listeners and event delegation work
- how to render UI from state without scattering DOM mutations
- how API data flows from `fetch` into visible elements
- what virtual DOM means by building a tiny live element representation yourself

**Before this page, you should know:** [async and await in Practice](./03-async-and-await-in-practice.md)

---

## What the DOM Is

The Document Object Model, or DOM, is the browser's object representation of the
HTML document. When the browser loads HTML, it builds a tree of objects.
JavaScript can read and change that tree.

```text
document
    |
    `-- html
        |-- head
        `-- body
            |-- h1
            |-- form
            `-- ul#task-list
```

HTML is text. The DOM is live objects created from that text. Changing a DOM
object changes what the user sees.

## A Minimal Page to Practice With

Create `index.html`:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>DOM Practice</title>
    <script type="module" src="./app.js"></script>
  </head>
  <body>
    <h1>Task List</h1>

    <form id="task-form">
      <label>
        Task title
        <input id="task-title" name="title" autocomplete="off">
      </label>
      <button type="submit">Add task</button>
    </form>

    <p id="message" role="status"></p>
    <ul id="task-list"></ul>
  </body>
</html>
```

The `type="module"` script lets you use modern JavaScript modules and runs after
the document is parsed enough for normal element selection.

## Query DOM Elements

Create `app.js`:

```javascript
const form = document.querySelector("#task-form");
const titleInput = document.querySelector("#task-title");
const message = document.querySelector("#message");
const taskList = document.querySelector("#task-list");

if (!form || !titleInput || !message || !taskList) {
  throw new Error("Required DOM element is missing.");
}
```

`document.querySelector(selector)` returns the first matching element or `null`.
The selector uses CSS selector syntax: `#id`, `.class`, `button`, or
`button[data-action="save"]`.

The null check matters. If an id is misspelled in HTML, the page should fail with
a clear error instead of breaking later with a confusing message.

## Create and Append Elements

```javascript
const item = document.createElement("li");
item.textContent = "Learn createElement";
taskList.append(item);
```

`createElement("li")` creates a new element object. It does not appear on the
page until you append it somewhere.

Use `textContent` for user-provided text. It treats the value as text, not HTML.

Avoid this with user data:

```javascript
item.innerHTML = titleFromUser;
```

`innerHTML` parses HTML. That is useful when you intentionally render trusted
markup, but unsafe for normal user input.

## Render From State

State is the data your UI is based on.

```javascript
let tasks = [
  { id: crypto.randomUUID(), title: "Learn DOM", done: false },
  { id: crypto.randomUUID(), title: "Render from state", done: true }
];
```

Render means "turn state into visible DOM."

```javascript
function renderTasks() {
  taskList.replaceChildren();

  for (const task of tasks) {
    const item = document.createElement("li");
    const checkbox = document.createElement("input");
    const label = document.createElement("span");

    checkbox.type = "checkbox";
    checkbox.checked = task.done;
    checkbox.dataset.id = task.id;
    checkbox.dataset.action = "toggle";

    label.textContent = task.title;

    item.append(checkbox, " ", label);
    taskList.append(item);
  }
}

renderTasks();
```

`replaceChildren()` clears existing child nodes. Then the function rebuilds the
list from the current state.

This is not the only render strategy, but it is beginner-safe because the DOM is
always a reflection of the data.

## Handle Form Events

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();

  const title = titleInput.value.trim();

  if (title === "") {
    message.textContent = "Enter a task title.";
    return;
  }

  tasks = [
    ...tasks,
    {
      id: crypto.randomUUID(),
      title,
      done: false
    }
  ];

  titleInput.value = "";
  message.textContent = "Task added.";
  renderTasks();
});
```

`event.preventDefault()` stops the form from reloading the page. The handler
updates state first, then calls `renderTasks()`.

## Use Event Delegation for Dynamic Lists

List items are recreated during render, so attaching a listener to every checkbox
can become annoying. Event delegation puts one listener on the parent.

```javascript
taskList.addEventListener("change", (event) => {
  const target = event.target;

  if (!(target instanceof HTMLInputElement)) return;
  if (target.dataset.action !== "toggle") return;

  const id = target.dataset.id;

  tasks = tasks.map((task) => {
    if (task.id !== id) return task;
    return { ...task, done: !task.done };
  });

  renderTasks();
});
```

The parent receives events from children because many DOM events bubble upward.
The handler checks whether the real target is the checkbox it cares about.

## Remove Elements

Add a remove button in the render function:

```javascript
const removeButton = document.createElement("button");
removeButton.type = "button";
removeButton.textContent = "Remove";
removeButton.dataset.id = task.id;
removeButton.dataset.action = "remove";

item.append(checkbox, " ", label, " ", removeButton);
```

Handle it:

```javascript
taskList.addEventListener("click", (event) => {
  const button = event.target.closest("button[data-action='remove']");
  if (!button) return;

  const id = button.dataset.id;
  tasks = tasks.filter((task) => task.id !== id);
  renderTasks();
});
```

`closest(selector)` walks from the target upward until it finds a matching
element. This is helpful when the user clicks an icon or nested span inside a
button.

## Fetch API Data and Render It

```javascript
async function loadTodo(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/todos/${id}`);

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  const todo = await response.json();

  if (typeof todo.title !== "string") {
    throw new Error("API response is missing a title.");
  }

  return {
    id: String(todo.id),
    title: todo.title,
    done: Boolean(todo.completed)
  };
}
```

Keep `fetch` logic separate from DOM logic. The fetch function should return
data. The event handler decides how to show loading, success, or error states.

```javascript
async function loadAndRenderTodo() {
  message.textContent = "Loading...";

  try {
    const task = await loadTodo(1);
    tasks = [task];
    message.textContent = "Loaded task.";
    renderTasks();
  } catch (error) {
    message.textContent = "Could not load task.";
    console.error(error);
  }
}
```

## Tiny Virtual DOM Intro with Live Element Creation

Virtual DOM means "describe the UI as plain JavaScript objects, compare or
replace the description, then update the real DOM." Frameworks do this with much
more sophistication. This tiny version is only for the mental model.

First, describe an element:

```javascript
const virtualItem = {
  tag: "li",
  props: {
    className: "task"
  },
  children: ["Learn virtual DOM idea"]
};
```

Then turn that object into a real DOM element:

```javascript
function createElementFromVirtualNode(node) {
  if (typeof node === "string") {
    return document.createTextNode(node);
  }

  const element = document.createElement(node.tag);

  for (const [name, value] of Object.entries(node.props ?? {})) {
    element[name] = value;
  }

  for (const child of node.children ?? []) {
    element.append(createElementFromVirtualNode(child));
  }

  return element;
}

const realElement = createElementFromVirtualNode(virtualItem);
taskList.append(realElement);
```

Visual model:

```text
state
  |
  v
virtual node objects
  |
  v
real DOM elements
  |
  v
browser paints pixels
```

This is not React. It is the core idea at tiny scale: UI can be described as
data, then rendered into the real DOM.

## Practical DOM Rules

- Query elements once at module startup when the page structure is stable.
- Check for `null` when an element is required.
- Store app data in state, not scattered across DOM text.
- Render from state in one place.
- Use `textContent` for user data.
- Use event delegation for dynamic lists.
- Keep fetch functions separate from render functions.
- Show loading, success, empty, and error states.

---

## Recap

- The DOM is the browser's live object tree for the page.
- `querySelector`, `createElement`, `textContent`, `append`, `replaceChildren`, and `remove` are core DOM tools.
- Events let the page respond to users.
- Rendering should turn state into DOM.
- A virtual DOM is a data description of UI that can be turned into real elements.

## Try It Yourself

Build a browser page that:

- lets the user add tasks through a form
- renders tasks with checkboxes and remove buttons
- uses event delegation for list actions
- stores all task data in a `tasks` array
- shows one loading message and one error message for a fake API load

---

[**Next ->** Chapter 04 Checkpoint](./05-chapter-04-checkpoint.md)  
[**<- Previous** async and await in Practice](./03-async-and-await-in-practice.md)
