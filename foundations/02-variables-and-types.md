<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# Variables and Types

> Variables store values. Types define what kind of value each variable can hold.

**You will learn:**
- What a variable is
- What a type is
- Why types prevent bugs and improve clarity

**Before this page, you should know:**
- [What Is Programming?](./01-what-is-programming.md)

---

## Variable in plain language

A **variable** is a named storage location in your program.

You can think of it like a labeled box:
- label: variable name
- contents: variable value

Example in Python:

```python
player_name = "Alex"
score = 1200
```

Here:
- `player_name` stores text
- `score` stores a number

## Type in plain language

A **type** describes what kind of value something is.

Common beginner types:
- text (string)
- whole number (integer)
- decimal number (float)
- true/false (boolean)

Why this matters: operations must match value kinds.

## Why types exist

Types help answer:
- Can this value be added?
- Can this value be compared?
- Can this value be printed as text directly?

Type systems catch mistakes early.

Example mistake:

```python
age = "20"
next_year = age + 1
```

This fails because `age` is text, not a number.

Corrected:

```python
age = 20
next_year = age + 1
```

> [!IMPORTANT]
> Clear variable names and correct types are the foundation of readable code.

## Naming variables well

Good names describe meaning, not storage mechanics.

Prefer:
- `total_price`
- `is_logged_in`
- `max_health`

Avoid:
- `x1`
- `data2`
- `tempValueThing`

---

## Recap

- Variable: named value holder.
- Type: category of that value.
- Good names and type awareness reduce bugs and confusion.

## Try it yourself

Write three variables for a game character:
- name (text)
- level (number)
- is_alive (true/false)

Then print them.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← What Is Programming?](./01-what-is-programming.md) | [Foundations](./README.md) · [Home](../README.md) | [Functions →](./03-functions.md) |

</div>
