<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# Making Decisions

> Programs make decisions with conditions: if this is true, do this; otherwise do something else.

**You will learn:**
- What conditional logic is
- How `if`, `elif`, and `else` work
- How decision logic affects program behavior

**Before this page, you should know:**
- [Functions](./03-functions.md)

---

## Decision logic in plain language

A decision is a branch in your instruction flow.

Example question:
- Is user age >= 18?
- Is player health <= 0?
- Is file present?

Your program checks a condition and chooses a path.

## Basic conditional structure

```python
health = 42

if health <= 0:
    print("Game Over")
elif health < 25:
    print("Critical health")
else:
    print("Stable")
```

How it works:
- `if` checks first condition
- `elif` checks next condition if earlier one failed
- `else` runs if nothing else matched

## Conditions use booleans

A condition must evaluate to true or false.

Common comparison operators:
- `==` equal to
- `!=` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to

## Keep decision trees readable

- Prefer simple conditions.
- Name intermediate booleans when conditions are long.
- Avoid deeply nested branches when possible.

> [!IMPORTANT]
> Complex decision trees are a major source of bugs. Simplicity is a performance feature for humans.

---

## Recap

- Conditional logic lets programs choose behavior based on state.
- `if/elif/else` creates branching paths.
- Readable conditions are easier to debug and maintain.

## Try it yourself

Create decision logic for grade output:
- `A` for score >= 90
- `B` for score >= 80
- `C` for score >= 70
- otherwise `Needs Improvement`

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Functions](./03-functions.md) | [Foundations](./README.md) · [Home](../README.md) | [Repeating Work →](./05-repeating-work.md) |

</div>
