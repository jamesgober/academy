<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 04](./README.md)

---

# Promises and Error Handling

> Promises provide structured asynchronous sequencing and centralized error handling.

**You will learn:**
- promise lifecycle states
- chaining with `then` and `catch`
- error propagation behavior

**Before this page, you should know:** callback/event-loop basics.

---

## Promise lifecycle

- `pending`
- `fulfilled`
- `rejected`

## Chain example

```javascript
fetch("https://api.example.com/items")
  .then(response => response.json())
  .then(data => {
    console.log(data.length);
  })
  .catch(error => {
    console.error("Request failed:", error.message);
  });
```

Throwing inside `then` also routes to `catch`.

## Common pitfalls

- forgetting to return inside `then`
- swallowing errors with empty catch blocks
- mixing callbacks and promises without clear boundaries

---

## Recap

- Promises model async results explicitly.
- Chain returns carefully for predictable flow.
- Keep error handling centralized and visible.

## Try it yourself

Create a promise that resolves after 1 second and log success or failure paths.

---

[**Next ->** async and await in Practice](./03-async-and-await-in-practice.md)  
[**<- Previous** Asynchronous JavaScript Fundamentals](./01-asynchronous-javascript-fundamentals.md)
