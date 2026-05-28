<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 04](./README.md)

---

# async and await in Practice

> `async` and `await` make promise code read top-to-bottom while still running asynchronously.

**You will learn:**
- how `async` functions return promises
- how `await` pauses the async function, not the whole page
- sequential versus parallel awaits
- `Promise.all`, `Promise.allSettled`, and cancellation with `AbortController`
- loading, success, empty, and error UI states

**Before this page, you should know:** [Promises and Error Handling](./02-promises-and-error-handling.md)

---

## Basic `async` Function

```javascript
async function loadTodo(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/todos/${id}`);

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }

  return response.json();
}
```

An `async` function always returns a promise. Returning a value fulfills the
promise. Throwing an error rejects it.

## `await` Pauses the Function

`await` waits for a promise result inside the async function. It does not freeze
the whole browser.

```javascript
async function handleClick() {
  statusEl.textContent = "Loading...";
  const todo = await loadTodo(1);
  statusEl.textContent = todo.title;
}
```

The browser can still paint, receive events, and process other work while the
request is pending.

## Error Handling with `try`/`catch`

```javascript
async function loadAndRenderTodo() {
  statusEl.textContent = "Loading...";

  try {
    const todo = await loadTodo(1);
    renderTodo(todo);
    statusEl.textContent = "Loaded.";
  } catch (error) {
    statusEl.textContent = "Could not load todo.";
    console.error(error);
  }
}
```

Catch errors where you can show a useful UI state or recover.

## Sequential Versus Parallel

Sequential: second request depends on first result.

```javascript
const user = await loadUser(userId);
const posts = await loadPostsForUser(user.id);
```

Parallel: requests are independent.

```javascript
const [users, posts] = await Promise.all([
  loadUsers(),
  loadPosts()
]);
```

`Promise.all` rejects as soon as one promise rejects.

Use `Promise.allSettled` when you want every result, even failures:

```javascript
const results = await Promise.allSettled([
  loadUsers(),
  loadPosts()
]);
```

## Cancellation with `AbortController`

`AbortController` lets you cancel a fetch request.

```javascript
async function fetchJSON(url, { signal } = {}) {
  const response = await fetch(url, { signal });

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }

  return response.json();
}
```

Timeout wrapper:

```javascript
async function fetchWithTimeout(url, ms = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), ms);

  try {
    return await fetchJSON(url, { signal: controller.signal });
  } finally {
    clearTimeout(timeoutId);
  }
}
```

Abort errors are expected when you cancel. Handle them differently from real
network failures when useful.

## UI State Pattern

```javascript
const state = {
  status: "idle",
  todos: [],
  error: null
};
```

Common states:

| State | Meaning |
|---|---|
| `idle` | nothing started yet |
| `loading` | request in progress |
| `success` | data loaded |
| `empty` | request succeeded but no items |
| `error` | request failed |

Render based on state:

```javascript
function renderStatus(state) {
  if (state.status === "loading") return "Loading...";
  if (state.status === "error") return state.error;
  if (state.status === "empty") return "No results.";
  return "";
}
```

## Retry with Delay

```javascript
function wait(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function retryWithDelay(operation, attempts = 3, delayMs = 500) {
  let lastError;

  for (let attempt = 1; attempt <= attempts; attempt += 1) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;
      if (attempt < attempts) {
        await wait(delayMs);
      }
    }
  }

  throw lastError;
}
```

Keep retries visible and limited. Endless retries create bad user experiences.

## Reference Links

- [Async and Promise Patterns](../../reference/async-and-promise-patterns.md)
- [Browser Runtime and Web APIs](../../reference/browser-runtime-and-web-apis.md)

---

## Recap

- `async` functions return promises.
- `await` pauses the async function, not the whole browser.
- Use `Promise.all` for independent work.
- Use `AbortController` for cancellation and timeouts.
- UI code should represent loading, success, empty, and error states.

## Try It Yourself

Write `loadTodos()` with `fetch`, status handling, timeout cancellation, and a
visible error message. Then load two independent endpoints with `Promise.all`.

---

[**Next ->** DOM, Events, Rendering, and API Data](./04-working-with-http-apis.md)  
[**<- Previous** Promises and Error Handling](./02-promises-and-error-handling.md)
