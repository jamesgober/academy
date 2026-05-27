<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [JavaScript](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Variables, Types, and Coercion

> JavaScript flexibility is powerful, but type coercion can surprise you if you do not read expressions carefully.

**You will learn:**
- `let`, `const`, and safe variable patterns
- Primitive types and `typeof` checks
- Explicit versus implicit coercion behavior

**Before this page, you should know:** basic JS file execution.

---

## Variable declarations

```javascript
const appName = "Tracker";
let retryCount = 0;
```

Use `const` by default. Use `let` only when reassignment is required.

## Primitive types

- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `symbol`
- `bigint`

```javascript
console.log(typeof "x"); // string
console.log(typeof 10); // number
```

## Coercion pitfalls

```javascript
console.log("5" + 1); // "51"
console.log("5" - 1); // 4
```

Prefer explicit conversion for readability:

```javascript
const total = Number("5") + 1;
```

---

## Recap

- Use `const` by default.
- Know primitive types and inspect with `typeof`.
- Avoid relying on implicit coercion in production logic.

## Try it yourself

Create three values with different types and convert them explicitly before comparison.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Operators, Comparisons, and Boolean Logic →](./02-operators-comparisons-and-boolean-logic.md) |

</div>
