<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 04](./README.md)

---

# Promises and Error Handling

> A promise represents an async result that is pending now and will either fulfill or reject later.

**You will learn:**
- promise lifecycle states
- `then`, `catch`, and `finally`
- error propagation rules
- common promise mistakes
- how to retry async work safely

**Before this page, you should know:** [Asynchronous JavaScript Fundamentals](./01-asynchronous-javascript-fundamentals.md)

---

## Promise States

```text
pending
   |
   |-- fulfilled -> success value
   |
   `-- rejected  -> error/reason
```

A settled promise is either fulfilled or rejected. It does not switch back.

## Create a Promise

```javascript
function wait(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

wait(500).then(() => {
  console.log("half a second passed");
});
```

Most browser work does not require manually creating promises. APIs like
`fetch()` already return them. Still, seeing the shape helps.

## Chain with `then`, `catch`, and `finally`

```javascript
fetch("https://jsonplaceholder.typicode.com/todos/1")
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    return response.json();
  })
  .then((todo) => {
    console.log(todo.title);
  })
  .catch((error) => {
    console.error("Request failed:", error.message);
  })
  .finally(() => {
    console.log("Request finished.");
  });
```

Return values from one `then` become input to the next `then`. Throwing routes to
the next `catch`.

## Common Mistake: Forgetting `return`

Wrong:

```javascript
fetch(url)
  .then((response) => {
    response.json();
  })
  .then((data) => {
    console.log(data); // undefined
  });
```

Correct:

```javascript
fetch(url)
  .then((response) => {
    return response.json();
  })
  .then((data) => {
    console.log(data);
  });
```

With concise arrow syntax:

```javascript
fetch(url)
  .then((response) => response.json())
  .then((data) => console.log(data));
```

## Promise Error Boundaries

A `catch` handles rejections before it in the chain.

```javascript
loadUser()
  .then(renderUser)
  .catch(renderError);
```

Do not use empty catches:

```javascript
.catch(() => {});
```

That hides real failures. At minimum, log or show a user-facing state.

## Retry Pattern

Retries can help with temporary network failures. Keep them limited.

```javascript
async function retry(operation, attempts = 3) {
  let lastError;

  for (let attempt = 1; attempt <= attempts; attempt += 1) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;
    }
  }

  throw lastError;
}
```

Use:

```javascript
const todo = await retry(() => loadTodo(1), 3);
```

Do not retry validation errors or permission errors. Retry only failures that
might succeed later.

## Reference Links

- [Async and Promise Patterns](../../reference/async-and-promise-patterns.md)
- [Errors, Warnings, and Debugging Guide](../../reference/errors-warnings-and-debugging-guide.md)

---

## Recap

- Promises are pending, fulfilled, or rejected.
- `then` transforms success values.
- `catch` handles errors from earlier in the chain.
- Always return nested promises from `then`.
- Retry only when the failure might be temporary.

## Try It Yourself

Write a `wait(ms)` promise, chain two `then` calls, add a `catch`, then rewrite
the same behavior with `async`/`await` in the next lesson.

---

[**Next ->** async and await in Practice](./03-async-and-await-in-practice.md)  
[**<- Previous** Asynchronous JavaScript Fundamentals](./01-asynchronous-javascript-fundamentals.md)
