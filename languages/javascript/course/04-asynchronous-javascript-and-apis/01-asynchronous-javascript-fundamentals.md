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

# Asynchronous JavaScript Fundamentals

> JavaScript is single-threaded, but asynchronous APIs allow non-blocking workflows.

**You will learn:**
- event loop mental model
- callback sequencing basics
- why blocking code harms responsiveness

**Before this page, you should know:** function and module basics.

---

## Event loop idea

- call stack runs current code
- async operations complete later
- queued callbacks execute when stack clears

## Callback example

```javascript
console.log("start");

setTimeout(() => {
  console.log("timeout done");
}, 0);

console.log("end");
```

Output order:

```text
start
end
timeout done
```

## Why it matters

If you block the event loop with heavy synchronous work, timers, IO callbacks, and API responses are delayed.

---

## Recap

- Asynchronous does not mean parallel by default.
- Event loop order explains many "unexpected" outputs.
- Keep the main thread responsive.

## Try it yourself

Predict output order for a script with two `setTimeout` calls and verify your prediction.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Promises and Error Handling →](./02-promises-and-error-handling.md) |

</div>
