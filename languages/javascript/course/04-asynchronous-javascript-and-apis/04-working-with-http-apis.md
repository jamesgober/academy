<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [JavaScript](../../README.md) · [Chapter 04](./README.md)

</div>

---

# DOM, Events, Rendering, and API Data

> Frontend JavaScript connects user events, DOM updates, and async data into one interaction loop.

**You will learn:**
- DOM query and render patterns
- event handling architecture for UI actions
- fetching API data and rendering safely
- what virtual DOM means at a conceptual level

**Before this page, you should know:** async/await workflow.

---

## DOM render pattern

```javascript
const listEl = document.querySelector("#todo-list");

function renderTodos(todos) {
  listEl.innerHTML = "";
  for (const todo of todos) {
    const li = document.createElement("li");
    li.textContent = todo.title;
    listEl.appendChild(li);
  }
}
```

## Event handling pattern

```javascript
document.querySelector("#load-button").addEventListener("click", async () => {
  const todos = await getTodos();
  renderTodos(todos);
});
```

Separate event handling, data fetching, and rendering so each part stays testable.

## Fetch pattern for UI data

```javascript
async function getTodo(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/todos/${id}`);
  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }
  return response.json();
}
```

Use dedicated fetch helpers and keep DOM mutation out of data-access functions.

## Defensive checks

- verify `response.ok`
- validate required fields in parsed JSON
- wrap request in try/catch where called
- show fallback UI state on failure

## Timeout pattern

```javascript
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000);

try {
  const response = await fetch(url, { signal: controller.signal });
  // process response
} finally {
  clearTimeout(timeout);
}
```

## Intro: Virtual DOM concept

Virtual DOM (VDOM) is an in-memory representation of UI state. Libraries compare previous and next virtual trees, then apply minimal DOM changes.

Mental model:
1. state changes
2. produce new virtual tree
3. diff old/new virtual tree
4. patch real DOM minimally

You do not need a framework to learn rendering discipline, but understanding VDOM explains why modern frontend libraries structure UI around state.

---

## Recap

- Keep DOM updates in dedicated render functions.
- Always validate status and shape of API responses.
- Keep request logic isolated in reusable functions.
- Understand VDOM as a state-to-UI optimization model.

## Try it yourself

Build a tiny page with a button that fetches data and renders a list, including one visible error message state.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← async and await in Practice](./03-async-and-await-in-practice.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 04 Checkpoint →](./05-chapter-04-checkpoint.md) |

</div>
