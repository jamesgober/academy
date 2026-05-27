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

# Loops, Iteration, and Control Flow

> Iteration style affects both performance and readability.

**You will learn:**
- `for`, `while`, and `for...of`
- when to use loop control (`break`, `continue`)
- naming loop variables beyond `i`

**Before this page, you should know:** conditionals and switch.

---

## Loop options

```javascript
for (let index = 0; index < items.length; index++) {
  console.log(items[index]);
}

for (const item of items) {
  console.log(item);
}
```

Use `for...of` when index math is not needed.

## Control flow

```javascript
for (const value of values) {
  if (value < 0) continue;
  if (value > 1000) break;
}
```

## Naming advice

`i` works, but descriptive names are better in non-trivial loops (`productIndex`, `orderNumber`).

---

## Recap

- Pick loop style based on intent.
- Use `break` and `continue` intentionally.
- Favor descriptive loop variable names for maintainability.

## Try it yourself

Refactor one index-based loop to `for...of` and compare readability.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Conditionals and Switch Patterns](./03-conditionals-and-switch-patterns.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Arrays, Objects, and Common Mutations →](./05-arrays-objects-and-common-mutations.md) |

</div>
