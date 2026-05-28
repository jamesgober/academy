<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 01](./README.md)

---

# Chapter 01 Checkpoint

> Verify setup and diagnostics literacy before moving deeper into language mechanics.

## You should be able to do all of this

- Set up a stable JavaScript practice environment
- Run `.js` files in a browser-based workflow
- Split code into modules and import functions
- Read and fix basic syntax/runtime errors

## Checkpoint mini challenge

Create this folder:

```text
js-setup-checkpoint/
|-- index.html
`-- src/
    |-- app.js
    `-- math.js
```

Requirements:

- `index.html` loads `src/app.js` with `type="module"`
- `math.js` exports `add` and `multiply`
- `app.js` imports both functions
- the page renders the result into a DOM element
- the Console has no errors

## Hints

- Browser module imports need file extensions: `./math.js`.
- Use a local server if module loading fails from `file://`.
- Use `document.querySelector` and check for `null`.

## Solution Direction

`app.js` should import functions, calculate values, then set `textContent` on a
real page element. Do not only use `console.log`.

---

## Recap

- Stable setup avoids future confusion.
- Module boundaries improve maintainability.
- Error interpretation is a core developer skill.

## Try it yourself

Intentionally trigger one syntax and one runtime error, then resolve both.

---

[**Next ->** Chapter Core Language Basics](../02-core-language-basics/README.md)  
[**<- Previous** Reading Errors and Warnings](./04-reading-errors-and-warnings.md)
