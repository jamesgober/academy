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

# async and await in Practice

> `async/await` makes promise-based code read like synchronous logic while preserving non-blocking behavior.

**You will learn:**
- converting promise chains to async functions
- try/catch error boundaries with await
- parallel versus sequential awaits

**Before this page, you should know:** promise chaining.

---

## Basic async function

```javascript
async function loadUsers() {
  const response = await fetch("https://api.example.com/users");
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  return response.json();
}
```

## Error handling

```javascript
try {
  const users = await loadUsers();
  console.log(users.length);
} catch (error) {
  console.error(error.message);
}
```

## Parallel awaits

```javascript
const [users, posts] = await Promise.all([
  fetch("https://api.example.com/users").then(r => r.json()),
  fetch("https://api.example.com/posts").then(r => r.json())
]);
```

Use `Promise.all` for independent requests.

---

## Recap

- `async/await` improves readability.
- Keep error boundaries explicit.
- Use parallel request patterns when dependencies allow.

## Try it yourself

Refactor one promise chain into `async/await` with equivalent behavior.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Promises and Error Handling](./02-promises-and-error-handling.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Working with HTTP APIs →](./04-working-with-http-apis.md) |

</div>
