# Errors, Warnings, and Debugging Guide

## Syntax error pattern

```text
SyntaxError: Unexpected token '}'
```

Usually points to malformed JS grammar near the indicated line.

## Runtime error pattern

```text
TypeError: Cannot read properties of undefined (reading 'name')
```

Check object existence before property access.

## Stack trace reading

1. Read top application frame first.
2. Identify file + line.
3. Reproduce with same input.
4. Fix and rerun.

## Debug workflow

- reproduce reliably
- isolate failing function
- inspect state with debugger/logs
- add regression test

## Warning sources

JavaScript warnings typically come from linting tools, not the runtime. Treat lint warnings as quality issues, not optional noise.

---

[← JavaScript Reference](./README.md)
