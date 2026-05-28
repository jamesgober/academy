<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 01](./README.md)

---

# Setting Up a JavaScript Practice Environment

> Use a browser-first workflow so every lesson can create, inspect, and change a real web page.

**You will learn:**
- how to run JavaScript in browser DevTools
- when a plain HTML file is enough
- when to use a local server
- how script tags differ from module scripts
- where Node.js fits without turning this into a Node course
- a beginner-friendly folder structure for browser projects

**Before this page, you should know:** terminal basics and navigating folders.

---

## The Main Environment: Browser + DevTools

JavaScript runs in many places, but this track is about web JavaScript. The main
runtime is the browser. A runtime is the program that executes your JavaScript
and provides extra APIs. Browsers provide `document`, DOM elements, events,
`fetch`, storage, timers, and many other Web APIs.

Open a modern browser such as Chrome, Edge, Firefox, or Safari. Open Developer
Tools, then use:

| DevTools area | Use it for |
|---|---|
| Console | run small snippets, inspect errors, log values |
| Elements/Inspector | inspect and edit live HTML/CSS |
| Sources/Debugger | set breakpoints and step through code |
| Network | inspect API requests, responses, headers, and failures |
| Application/Storage | inspect `localStorage`, `sessionStorage`, cookies, and caches |

Console test:

```javascript
console.log("hello from browser console");
document.title = "JavaScript Practice";
```

If the page title changes, you touched the live document.

## File-Based Practice

Create this folder:

```text
js-practice/
|-- index.html
`-- app.js
```

`index.html`:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>JavaScript Practice</title>
    <script src="./app.js" defer></script>
  </head>
  <body>
    <h1 id="heading">JavaScript Practice</h1>
  </body>
</html>
```

`app.js`:

```javascript
const heading = document.querySelector("#heading");

if (!heading) {
  throw new Error("Missing #heading element.");
}

heading.textContent = "JavaScript is running.";
console.log("app.js loaded");
```

Open `index.html` in your browser and check the page and Console.

## Script Tags: Classic Versus Module

Classic script:

```html
<script src="./app.js" defer></script>
```

Module script:

```html
<script type="module" src="./src/app.js"></script>
```

| Script type | Use when | Notes |
|---|---|---|
| classic + `defer` | simple one-file or early lessons | waits until HTML is parsed before running |
| `type="module"` | using `import`/`export` | deferred by default, strict mode by default |

This track uses module scripts once lessons start splitting files.

## Why a Local Server Matters

Opening `index.html` directly creates a `file://` page. That is fine for simple
scripts, but browser modules, fetch requests, and some APIs work better from
`http://localhost`.

If Node.js is installed, you can run a small local server:

```bash
npx serve .
```

Or with Python:

```bash
python -m http.server 5173
```

Then open:

```text
http://localhost:5173
```

Localhost means "this computer." A local server is not deployment. It is just a
browser-friendly way to load project files.

## Where Node.js Fits

Node.js is a JavaScript runtime outside the browser. It is useful for:

- installing frontend tools with npm
- running tests
- running formatters and linters
- building projects
- writing server-side JavaScript

This track uses Node lightly for tools. A separate Node/TypeScript section can
go deeper later.

Check Node and npm:

```bash
node --version
npm --version
```

If those commands fail, you can still complete browser-only lessons. Install
Node before testing/tooling chapters if you want the full workflow.

## Recommended Browser Project Structure

```text
task-tracker/
|-- index.html
|-- styles.css
|-- src/
|   |-- app.js
|   |-- state.js
|   |-- render.js
|   |-- events.js
|   `-- storage.js
`-- tests/
    `-- state.test.js
```

Use this structure when a project grows beyond one file:

- `app.js` starts the app and wires modules together.
- `state.js` changes data without touching the DOM.
- `render.js` turns state into DOM.
- `events.js` handles user actions.
- `storage.js` reads/writes browser storage.
- `tests/` checks pure logic.

## Visual Model

```text
Browser
    |
    |-- DevTools Console  -> quick experiments
    |-- index.html        -> page structure
    |-- app.js            -> behavior
    |-- DOM               -> live page objects
    `-- localStorage      -> browser persistence

Optional tool layer:
Node.js + npm -> tests, linting, formatting, local server
```

## Setup Checklist

- Browser opens DevTools.
- Console can run `console.log("test")`.
- `index.html` loads `app.js`.
- DOM text changes from JavaScript.
- You know whether you are using `file://` or `http://localhost`.
- You know Node is for tools here, not the main course target.

## Reference Links

- [Commands and Tooling](../../reference/commands-and-tooling.md)
- [Browser Runtime and Web APIs](../../reference/browser-runtime-and-web-apis.md)

---

## Recap

- This JavaScript track is browser-first.
- DevTools is part of the programming environment, not an optional extra.
- Use `defer` for classic scripts and `type="module"` for module-based projects.
- Use a local server for module-heavy or API-heavy browser work.
- Node.js appears here mainly for tooling; deeper Node belongs in its own track.

## Try It Yourself

Create `index.html` and `app.js`, change an `<h1>` with JavaScript, inspect the
element in DevTools, then reload the page from a local server.

---

[**Next ->** Your First JavaScript Program](./02-your-first-javascript-program.md)  
[**<- Previous** Chapter Getting Started](./README.md)
