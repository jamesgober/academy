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

# Arrays, Objects, and Common Mutations

> Most JavaScript programs are data transformation pipelines over arrays and objects.

**You will learn:**
- common array operations and when they mutate
- object property access and updates
- immutable update patterns for safer state changes

**Before this page, you should know:** loops and variable basics.

---

## Array operations

```javascript
const numbers = [1, 2, 3];
numbers.push(4);      // mutates
const doubled = numbers.map(n => n * 2); // returns new array
```

## Object operations

```javascript
const user = { name: "Kai", points: 5 };
user.points += 1; // mutation

const updatedUser = { ...user, points: user.points + 1 }; // immutable pattern
```

## Mutation versus immutability

Mutation is sometimes fine in small scripts.
In larger systems, immutable updates reduce accidental side effects.

---

## Recap

- Know which APIs mutate data.
- Use `map/filter/reduce` for transform pipelines.
- Prefer immutable updates when shared state exists.

## Try it yourself

Take one object mutation and rewrite it using spread syntax.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Loops, Iteration, and Control Flow](./04-loops-iteration-and-control-flow.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Functions, Objects, and Modules →](../03-functions-objects-and-modules/README.md) |

</div>
