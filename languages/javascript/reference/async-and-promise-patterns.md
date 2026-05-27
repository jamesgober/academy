# Async and Promise Patterns

## Event loop reminder

JavaScript executes synchronous code first, then queued asynchronous callbacks.

## Promise states

- `pending`
- `fulfilled`
- `rejected`

## Promise chain

```javascript
fetch(url)
  .then(r => r.json())
  .then(data => process(data))
  .catch(err => handle(err));
```

Return values from `then` when chaining dependent work.

## async/await pattern

```javascript
try {
  const r = await fetch(url);
  if (!r.ok) throw new Error(`HTTP ${r.status}`);
  const data = await r.json();
} catch (err) {
  console.error(err.message);
}
```

## Sequential versus parallel

Sequential:

```javascript
const user = await getUser();
const orders = await getOrders(user.id);
```

Parallel:

```javascript
const [users, posts] = await Promise.all([getUsers(), getPosts()]);
```

Use sequential when second request depends on first result.

## Parallel requests

```javascript
const [a, b] = await Promise.all([reqA(), reqB()]);
```

## Timeout/cancellation pattern

```javascript
const controller = new AbortController();
const timer = setTimeout(() => controller.abort(), 5000);

try {
  const response = await fetch(url, { signal: controller.signal });
  return await response.json();
} finally {
  clearTimeout(timer);
}
```

## UI integration pattern

1. set loading state
2. perform async call
3. render success state
4. render error state on failure
5. clear loading state

## Guidance

- Use `Promise.all` for independent operations.
- Use explicit error boundaries.
- Avoid mixing callbacks and promises in one flow unless necessary.
- Keep async data logic separate from DOM rendering functions.

---

[← JavaScript Reference](./README.md)
