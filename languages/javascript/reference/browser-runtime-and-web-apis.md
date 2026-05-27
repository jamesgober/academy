# Browser Runtime and Web APIs

## Runtime model

Frontend JavaScript runs in a browser environment with access to browser-provided APIs.

Core global objects:
- `window`
- `document`
- `location`
- `history`
- `navigator`

## Timing APIs

```javascript
const id = setTimeout(() => console.log("later"), 500);
clearTimeout(id);

const interval = setInterval(() => console.log("tick"), 1000);
clearInterval(interval);
```

## Storage APIs

```javascript
localStorage.setItem("theme", "dark");
const theme = localStorage.getItem("theme");

sessionStorage.setItem("tokenHint", "abc");
```

Guidance:
- `localStorage` persists across sessions
- `sessionStorage` lasts for tab session

## Network API

```javascript
const response = await fetch("/api/items");
const data = await response.json();
```

Always check `response.ok` before parsing.

## URL and query params

```javascript
const url = new URL(window.location.href);
const page = url.searchParams.get("page") ?? "1";
```

## Browser API safety checklist

1. Guard against unavailable features in older browsers.
2. Handle storage and network failures.
3. Keep API logic separate from rendering logic.

---

[← JavaScript Reference](./README.md)
