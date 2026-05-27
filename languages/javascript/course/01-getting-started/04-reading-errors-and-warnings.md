<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 01](./README.md)

---

# Reading Errors and Warnings

> Debugging starts by decoding messages, not rewriting random lines.

**You will learn:**
- How JavaScript syntax/runtime errors are formatted
- Stack trace reading order
- First-error-first triage workflow

**Before this page, you should know:** how to run JavaScript and view console output.

---

## Typical syntax error

```text
app.js:4
console.log(user
            ^
SyntaxError: missing ) after argument list
```

Read in this order:
1. file and line
2. caret marker location
3. error type and message

## Typical runtime error

```text
TypeError: Cannot read properties of undefined (reading 'name')
    at printUser (app.js:8:22)
    at Object.<anonymous> (app.js:12:1)
```

Top frame usually points closest to your bug source.

## Triage loop

1. Reproduce reliably.
2. Fix first listed error.
3. Rerun.
4. Repeat.

> [!IMPORTANT]
> One syntax bug can trigger many follow-up errors. Fixing the first one often clears most others.

---

## Recap

- Read location first, then message details.
- Differentiate syntax errors from runtime errors.
- Use first-error-first workflow to reduce noise.

## Try it yourself

Create a missing-parenthesis syntax error, read its output, then fix it.

---

[**Next ->** Chapter 01 Checkpoint](./05-chapter-01-checkpoint.md)  
[**<- Previous** JavaScript Files, Modules, and Execution Flow](./03-javascript-files-modules-and-execution-flow.md)
