# Browser Runtime and Web APIs

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for browser globals, timers, fetch, storage, URL APIs, workers, and runtime boundaries.

Course lessons:

- [Setting Up a JavaScript Practice Environment](../course/01-getting-started/01-setting-up-a-javascript-practice-environment.md)
- [DOM, Events, Rendering, and API Data](../course/04-asynchronous-javascript-and-apis/04-working-with-http-apis.md)

## Browser Runtime Globals

| Global | What it represents |
|---|---|
| `window` | browser window/global object |
| `document` | DOM document |
| `location` | current URL |
| `history` | session navigation history |
| `navigator` | browser/device information |
| `localStorage` | persistent string key/value storage |
| `sessionStorage` | tab-session string key/value storage |
| `fetch` | network request API |

Node.js does not provide `document` or the browser DOM by default.

## Timers

```javascript
const timeoutId = setTimeout(() => {
  console.log("later");
}, 500);

clearTimeout(timeoutId);

const intervalId = setInterval(() => {
  console.log("tick");
}, 1000);

clearInterval(intervalId);
```

Timers are not exact scheduling guarantees. The event loop must be free before a
timer callback can run.

## Fetch

```javascript
const response = await fetch("/api/items", {
  method: "GET",
  headers: {
    Accept: "application/json"
  }
});

if (!response.ok) {
  throw new Error(`HTTP ${response.status}`);
}

const data = await response.json();
```

POST JSON:

```javascript
await fetch("/api/tasks", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ title: "Learn fetch" })
});
```

## URL and Query Params

```javascript
const url = new URL("/api/tasks", window.location.origin);
url.searchParams.set("status", "open");
url.searchParams.set("search", "dom");
```

Use `URL` and `URLSearchParams` instead of string-building query params by hand.

## Storage

```javascript
localStorage.setItem("theme", "dark");
const theme = localStorage.getItem("theme");
localStorage.removeItem("theme");
```

JSON:

```javascript
localStorage.setItem("tasks", JSON.stringify(tasks));
const tasks = JSON.parse(localStorage.getItem("tasks") ?? "[]");
```

Risk notes:

- storage values are strings
- parsing can fail
- users can clear storage
- storage is not secure for secrets
- browser settings may limit storage

## FormData

```javascript
const formData = new FormData(form);
const title = String(formData.get("title") ?? "").trim();
```

Validate values because form fields may be missing or empty.

## Web Workers

```javascript
const worker = new Worker("./worker.js", { type: "module" });

worker.postMessage([1, 2, 3]);
worker.addEventListener("message", (event) => {
  console.log(event.data);
});
```

Worker limitations:

- no direct DOM access
- communication uses messages
- useful for CPU-heavy work
- extra complexity for small tasks

## Cross References

- [DOM and Events Patterns](./dom-and-events-patterns.md)
- [Async and Promise Patterns](./async-and-promise-patterns.md)
- [Virtual DOM Intro](./virtual-dom-intro.md)
