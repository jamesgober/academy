<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C++](../../README.md) · [Chapter 02](./README.md)

</div>

---
# Conditionals: if, else-if, ternary, switch

Conditionals control branch behavior and are critical for readable business logic.

## Patterns

- `if` without `else`
- `if / else if / else`
- nested conditionals
- ternary comparator `cond ? a : b`
- `switch` for multi-branch discrete values

## `if` without `else`

```cpp
if (!configLoaded) {
	return;
}
```

Use this for guard-style early exit.

## `if / else if / else`

```cpp
if (score >= 90) {
	grade = 'A';
} else if (score >= 80) {
	grade = 'B';
} else {
	grade = 'C';
}
```

## Nested conditions

```cpp
if (isOnline) {
	if (isAdmin) {
		access = "full";
	}
}
```

Avoid deep nesting when possible; extract helper functions for readability.

```cpp
int grade = score >= 90 ? 1 : 2;
```

Use ternary for short value selection, not complex logic.

## `switch` guidance

`switch` is cleaner for many fixed discrete values:

```cpp
switch (role) {
case 1:
	access = "owner";
	break;
case 2:
	access = "editor";
	break;
default:
	access = "viewer";
	break;
}
```

Always handle default unless project rules intentionally forbid it.
---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Functions, Parameters, and Returns](./02-functions-parameters-and-returns.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Loops and Iteration Patterns →](./04-loops-and-iteration-patterns.md) |

</div>