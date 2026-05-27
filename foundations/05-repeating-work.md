<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# Repeating Work

> Loops repeat instructions so you do not write the same logic over and over.

**You will learn:**
- Why loops exist
- The difference between counted loops and condition loops
- How to avoid infinite-loop bugs

**Before this page, you should know:**
- [Making Decisions](./04-making-decisions.md)

---

## Why loops exist

If you need to process many items, loops are the scalable approach.

Examples:
- print each player name
- validate each input field
- retry a task until success or timeout

## For loop (counted/collection loop)

```python
players = ["Alex", "Sam", "Riley"]

for player in players:
    print(player)
```

Use `for` when iterating through a list or fixed range.

## While loop (condition loop)

```python
attempts = 0

while attempts < 3:
    print("Trying...")
    attempts += 1
```

Use `while` when you need to continue until a condition changes.

## Infinite loops

A loop that never becomes false runs forever.

Typical cause:
- forgetting to update loop control variable

> [!WARNING]
> Infinite loops can freeze programs or consume resources continuously.

## Loop control tools

- `break`: exit loop early
- `continue`: skip current iteration and move to next

---

## Recap

- Loops automate repetition.
- `for` is great for collections/ranges.
- `while` is great for condition-driven repetition.
- Always ensure loop conditions can eventually end.

## Try it yourself

Write a loop that prints numbers 1 through 10, then another that prints only
even numbers.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Making Decisions](./04-making-decisions.md) | [Foundations](./README.md) · [Home](../README.md) | [Objects in Programming →](./06-objects-in-programming.md) |

</div>
