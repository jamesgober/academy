# Async and Promise Patterns

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for event loop behavior, promises, async/await, cancellation, retries, and UI states.

Course lessons:

- [Asynchronous JavaScript Fundamentals](../course/04-asynchronous-javascript-and-apis/01-asynchronous-javascript-fundamentals.md)
- [Promises and Error Handling](../course/04-asynchronous-javascript-and-apis/02-promises-and-error-handling.md)
- [async and await in Practice](../course/04-asynchronous-javascript-and-apis/03-async-and-await-in-practice.md)

## Event Loop

```text
sync stack -> microtasks -> next task -> render opportunity -> repeat
```

| Queue/source | Examples |
|---|---|
| call stack | current synchronous function calls |
| microtasks | promise `.then`, `await` continuation |
| tasks | timers, user events, message events |

## Promise APIs

| API | Use | Risk/tradeoff |
|---|---|---|
| `promise.then(onFulfilled)` | success transform | return nested promises |
| `promise.catch(onRejected)` | error handling | empty catch hides bugs |
| `promise.finally(callback)` | cleanup | cannot inspect success value directly |
| `Promise.all([...])` | all independent work must succeed | rejects on first rejection |
| `Promise.allSettled([...])` | collect successes and failures | caller must inspect statuses |
| `Promise.race([...])` | first settled wins | easy to misuse for cancellation |
| `Promise.any([...])` | first fulfilled wins | rejects only if all reject |

## async/await

```javascript
async function loadJSON(url) {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }

  return response.json();
}
```

## Cancellation

```javascript
const controller = new AbortController();
const response = await fetch(url, { signal: controller.signal });
controller.abort();
```

Timeout:

```javascript
async function fetchWithTimeout(url, ms) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), ms);

  try {
    return await fetch(url, { signal: controller.signal });
  } finally {
    clearTimeout(timeoutId);
  }
}
```

## Retry

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

Retry temporary failures, not validation or permission errors.

## UI State Checklist

```text
idle -> loading -> success
                 -> empty
                 -> error
```

Always render loading and error states for user-visible async work.

## Cross References

- [Browser Runtime and Web APIs](./browser-runtime-and-web-apis.md)
- [DOM and Events Patterns](./dom-and-events-patterns.md)
- [Errors, Warnings, and Debugging Guide](./errors-warnings-and-debugging-guide.md)
