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

# Operators, Comparisons, and Boolean Logic

> Correct comparisons prevent subtle business bugs.

**You will learn:**
- Arithmetic and assignment operators
- Strict equality versus loose equality
- Boolean composition with `&&`, `||`, and `!`

**Before this page, you should know:** variables and primitive types.

---

## Equality choices

```javascript
console.log(5 === "5"); // false
console.log(5 == "5"); // true (coercion)
```

Prefer `===` and `!==` for predictable comparisons.

## Boolean composition

```javascript
const canCheckout = isLoggedIn && hasValidPayment && !isBanned;
```

Use parentheses when expressions grow complex.

## Nullish coalescing and optional chaining

```javascript
const city = user?.address?.city ?? "Unknown";
```

This avoids deep property access crashes and preserves valid falsy values like `0`.

---

## Recap

- Prefer strict equality for clarity.
- Boolean chains should remain readable.
- Optional chaining and nullish coalescing reduce guard noise.

## Try it yourself

Rewrite one loose-equality condition to strict equality and verify behavior.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Variables, Types, and Coercion](./01-variables-types-and-coercion.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Conditionals and Switch Patterns →](./03-conditionals-and-switch-patterns.md) |

</div>
