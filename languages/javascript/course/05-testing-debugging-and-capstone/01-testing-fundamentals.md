<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 05](./README.md)

---

# Testing Fundamentals

> Tests are small programs that prove your code still behaves the way you think it does.

**You will learn:**
- pure function tests
- DOM tests
- mocks and fakes
- user interaction tests
- what to test before a browser app is considered complete

**Before this page, you should know:** function, module, DOM, and event basics.

---

## Start with Pure Function Tests

Pure functions are easiest to test because they do not touch the DOM, storage, or
network.

```javascript
export function addTask(tasks, title) {
  return [
    ...tasks,
    {
      id: "test-id",
      title,
      done: false
    }
  ];
}
```

Minimal assertion helper:

```javascript
function assertEqual(actual, expected, message) {
  if (actual !== expected) {
    throw new Error(`${message} | expected=${expected}, actual=${actual}`);
  }
}
```

Test:

```javascript
const nextTasks = addTask([], "Learn tests");

assertEqual(nextTasks.length, 1, "adds one task");
assertEqual(nextTasks[0].title, "Learn tests", "stores title");
assertEqual(nextTasks[0].done, false, "new task starts incomplete");
```

## Test Shape

Use this mental model:

```text
arrange -> create inputs
act     -> call the behavior
assert  -> check the result
```

Example:

```javascript
const tasks = [{ id: "1", title: "A", done: false }];
const nextTasks = toggleTask(tasks, "1");

assertEqual(nextTasks[0].done, true, "toggles task to done");
```

## Test DOM Rendering

DOM rendering tests check whether state becomes the expected elements.

```javascript
function renderTasks(tasks, list) {
  list.replaceChildren();

  for (const task of tasks) {
    const item = document.createElement("li");
    item.textContent = task.title;
    list.append(item);
  }
}
```

Test in a browser-like environment:

```javascript
const list = document.createElement("ul");

renderTasks([{ id: "1", title: "Learn DOM", done: false }], list);

assertEqual(list.children.length, 1, "renders one task");
assertEqual(list.children[0].textContent, "Learn DOM", "renders task title");
```

DOM tests need a DOM. In real projects, tools often use a browser runner or a
DOM simulation such as jsdom. The important beginner idea is still the same:
create a container, render into it, inspect the result.

## Test User Interaction

Interaction tests simulate what a user does.

```javascript
const button = document.createElement("button");
let clicked = false;

button.addEventListener("click", () => {
  clicked = true;
});

button.click();

assertEqual(clicked, true, "click handler runs");
```

For forms:

```javascript
const form = document.createElement("form");
const input = document.createElement("input");
input.name = "title";
input.value = "Write tests";
form.append(input);

let submitted = false;

form.addEventListener("submit", (event) => {
  event.preventDefault();
  submitted = true;
});

form.dispatchEvent(new Event("submit", { bubbles: true, cancelable: true }));

assertEqual(submitted, true, "submit handler runs");
```

## Mocks and Fakes

A fake is a small working replacement for a real dependency.

```javascript
const fakeStorage = {
  data: new Map(),
  getItem(key) {
    return this.data.get(key) ?? null;
  },
  setItem(key, value) {
    this.data.set(key, value);
  }
};
```

Use fakes to test storage logic without touching real browser storage.

A mock records calls so a test can inspect them.

```javascript
function createMockLogger() {
  const messages = [];

  return {
    messages,
    log(message) {
      messages.push(message);
    }
  };
}
```

Use mocks sparingly. Prefer checking visible results when possible.

## Testing Fetch Logic

Keep fetch wrappers small and inject dependencies when useful.

```javascript
export async function loadTasks(fetchImpl = fetch) {
  const response = await fetchImpl("/api/tasks");

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }

  return response.json();
}
```

Test with a fake fetch:

```javascript
const fakeFetch = async () => ({
  ok: true,
  json: async () => [{ id: "1", title: "Fake task", done: false }]
});

const tasks = await loadTasks(fakeFetch);

assertEqual(tasks.length, 1, "loads fake tasks");
```

## What to Test in a Browser App

| Area | Test examples |
|---|---|
| pure state logic | add, remove, toggle, filter, count |
| validation | empty title, invalid number, missing API field |
| rendering | list rows, empty state, error message |
| events | form submit, button click, delegated list action |
| storage | save/load, corrupted JSON, missing key |
| async | success response, HTTP error, network rejection |

## Reference Links

- [Commands and Tooling](../../reference/commands-and-tooling.md)
- [DOM and Events Patterns](../../reference/dom-and-events-patterns.md)
- [Async and Promise Patterns](../../reference/async-and-promise-patterns.md)

---

## Recap

- Start with pure functions because they are easiest to test.
- DOM tests render into a container and inspect the result.
- Interaction tests simulate user actions.
- Fakes replace real dependencies with small controlled versions.
- Test success, failure, empty, and edge cases.

## Try It Yourself

Write tests for `addTask`, `toggleTask`, `saveTasks`, `loadTasks`, and
`renderTasks`. Include one corrupted-storage test and one click interaction test.

---

[**Next ->** Debugging Workflow and Tooling](./02-debugging-workflow-and-tooling.md)  
[**<- Previous** Chapter Testing, Debugging, and Capstone](./README.md)
