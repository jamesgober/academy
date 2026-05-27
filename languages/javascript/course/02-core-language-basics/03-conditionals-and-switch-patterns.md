<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 02](./README.md)

---

# Conditionals and Switch Patterns

> Branching logic should be explicit, shallow, and readable.

**You will learn:**
- if-only, if/else-if, and nested branches
- ternary operator boundaries
- switch statement structure and default handling

**Before this page, you should know:** comparisons and boolean logic.

---

## if and else-if

```javascript
if (score >= 90) {
  grade = "A";
} else if (score >= 80) {
  grade = "B";
} else {
  grade = "C";
}
```

Guard clause pattern:

```javascript
if (!user) return;
```

## Ternary usage

```javascript
const status = isActive ? "active" : "inactive";
```

Use ternary for small expressions; use `if` for multi-step branches.

## switch

```javascript
switch (role) {
  case "admin":
    canDelete = true;
    break;
  case "editor":
    canDelete = false;
    break;
  default:
    canDelete = false;
}
```

---

## Recap

- Choose branch style by complexity.
- Guard clauses simplify nested code.
- Always define `default` behavior in switch.

## Try it yourself

Write one if-chain and one equivalent switch for the same business rule.

---

[**Next ->** Loops, Iteration, and Control Flow](./04-loops-iteration-and-control-flow.md)  
[**<- Previous** Operators, Comparisons, and Boolean Logic](./02-operators-comparisons-and-boolean-logic.md)
