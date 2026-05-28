# Types and Coercion Cheat Sheet

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for primitive types, checks, truthiness, equality, and parsing.

Course lesson: [Variables, Types, and Coercion](../course/02-core-language-basics/01-variables-types-and-coercion.md).

## Primitive Types

| Type | Example | `typeof` | Notes |
|---|---|---|---|
| string | `"Ada"` | `"string"` | text |
| number | `42`, `3.14`, `NaN` | `"number"` | normal numeric type |
| boolean | `true` | `"boolean"` | branch values |
| undefined | `undefined` | `"undefined"` | missing/unassigned |
| null | `null` | `"object"` | intentional absence; `typeof` quirk |
| bigint | `42n` | `"bigint"` | very large integers |
| symbol | `Symbol("id")` | `"symbol"` | unique keys |

## Common Checks

```javascript
typeof value === "string"
typeof value === "number"
Number.isNaN(value)
Number.isInteger(value)
Array.isArray(value)
value === null
value === undefined
```

Object check:

```javascript
function isObject(value) {
  return typeof value === "object" && value !== null && !Array.isArray(value);
}
```

## Falsy Values

```text
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is truthy, including `[]` and `{}`.

## Equality

| Operator | Meaning | Default? |
|---|---|---:|
| `===` | strict equality, no type coercion | yes |
| `!==` | strict inequality, no type coercion | yes |
| `==` | loose equality with coercion | rare |
| `!=` | loose inequality with coercion | rare |

Risk examples:

```javascript
false == 0; // true
"" == 0; // true
null == undefined; // true
```

## Parsing

```javascript
const count = Number(input.value);

if (!Number.isInteger(count) || count < 1) {
  throw new Error("Count must be a positive integer.");
}
```

JSON boundary:

```javascript
const data = JSON.parse(raw);

if (typeof data.title !== "string") {
  throw new Error("Invalid title.");
}
```

## Cross References

- [Arrays and Objects Patterns](./arrays-and-objects-patterns.md)
- [Errors, Warnings, and Debugging Guide](./errors-warnings-and-debugging-guide.md)
- [DOM and Events Patterns](./dom-and-events-patterns.md)
