<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 04](./README.md)

---

# Asynchronous JavaScript Fundamentals

> Async JavaScript keeps the page responsive while timers, events, requests, and background work finish later.

**You will learn:**
- call stack, task queue, microtask queue, and event loop basics
- why `setTimeout(..., 0)` still runs later
- how promises schedule microtasks
- when to use Web Workers for background work
- where Node.js fits conceptually without becoming the focus

**Before this page, you should know:** function and module basics.

---

## The Main Thread

Browser JavaScript usually runs on the main thread. A thread is a path of
execution. The main thread also handles user input, layout, painting, and many
DOM operations.

If you run slow synchronous code on the main thread, the page can freeze.

```javascript
const start = Date.now();

while (Date.now() - start < 3000) {
  // Blocks the main thread for roughly 3 seconds.
}

console.log("done");
```

Do not write loops like this in UI code. The browser cannot respond smoothly
while the main thread is blocked.

## Event Loop Mental Model

```text
call stack runs current JavaScript
        |
        v
microtasks run after current stack clears
        |
        v
one queued task runs
        |
        v
browser may render
        |
        v
repeat
```

The event loop coordinates work. It decides when queued callbacks run after the
current synchronous code finishes.

## Tasks Versus Microtasks

Tasks, also called macrotasks, include timers, user events, and some browser
callbacks.

Microtasks include promise `.then`, promise rejection handlers, and code after
`await` resumes.

```javascript
console.log("script start");

setTimeout(() => {
  console.log("timeout task");
}, 0);

Promise.resolve().then(() => {
  console.log("promise microtask");
});

console.log("script end");
```

Output:

```text
script start
script end
promise microtask
timeout task
```

The promise callback runs before the timer because microtasks run before the next
task.

## User Events Are Async Too

```javascript
button.addEventListener("click", () => {
  console.log("clicked later");
});
```

The handler does not run when you register it. It runs later, when the browser
receives a click and the event loop reaches that task.

## Background Workers

A Web Worker runs JavaScript in a background thread. It cannot directly touch
the DOM, but it can do CPU-heavy work and send messages back to the main thread.

Use a worker when work is expensive enough to make the UI stutter.

`worker.js`:

```javascript
self.addEventListener("message", (event) => {
  const numbers = event.data;
  const total = numbers.reduce((sum, value) => sum + value, 0);
  self.postMessage(total);
});
```

`app.js`:

```javascript
const worker = new Worker("./worker.js", { type: "module" });

worker.addEventListener("message", (event) => {
  console.log("worker result:", event.data);
});

worker.postMessage([1, 2, 3, 4]);
```

Worker model:

```text
main thread
    |
    | postMessage(data)
    v
worker thread does background work
    |
    | postMessage(result)
    v
main thread updates DOM
```

Important limits:

- workers cannot use `document.querySelector`
- data is copied or transferred between threads
- workers add complexity, so do not use them for tiny tasks

## Node.js Context

Node.js also has an event loop, promises, timers, and async I/O. The exact APIs
and environment differ because Node is not a browser. Node does not provide the
DOM by default.

This track mentions Node for tooling and context. A dedicated Node course should
cover file systems, servers, streams, packages, and backend patterns separately.

## Reference Links

- [Async and Promise Patterns](../../reference/async-and-promise-patterns.md)
- [Browser Runtime and Web APIs](../../reference/browser-runtime-and-web-apis.md)

---

## Recap

- JavaScript runs synchronous code on the call stack.
- Timers and events run later through the task queue.
- Promise continuations run as microtasks.
- Blocking the main thread harms UI responsiveness.
- Web Workers move CPU-heavy work off the main thread but cannot touch the DOM.
- Node has async concepts too, but this track remains browser-first.

## Try It Yourself

Predict the output order of a script with `console.log`, `setTimeout`, and
`Promise.resolve().then`. Then create a tiny worker that sums an array and logs
the result in the main page.

---

[**Next ->** Promises and Error Handling](./02-promises-and-error-handling.md)  
[**<- Previous** Chapter Asynchronous JavaScript and APIs](./README.md)
