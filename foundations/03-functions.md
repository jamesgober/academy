<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# Functions

> A function is a reusable block of instructions that does one clear job.

**You will learn:**
- Why functions exist
- Inputs and outputs in plain language
- How functions improve code quality

**Before this page, you should know:**
- [Variables and Types](./02-variables-and-types.md)

---

## Why functions exist

Without functions, code repeats quickly.

Repeated code causes:
- copy-paste bugs
- harder updates
- lower readability

Functions let you write logic once and reuse it many times.

## Input and output

A function can receive **inputs** (parameters) and return an **output**.

Example:

```python
def add_tax(price, tax_rate):
    return price * (1 + tax_rate)

final_price = add_tax(100, 0.07)
print(final_price)
```

Here:
- inputs: `price`, `tax_rate`
- output: calculated final price

## Good function habits

- One function, one purpose.
- Name function by behavior (`calculate_total`, `send_email`).
- Keep function body focused and short when possible.

> [!TIP]
> If you need to explain a function name in a sentence, the function may be doing too much.

## Common beginner mistake

Writing giant functions that handle many unrelated tasks.

Better approach:
- split by responsibility
- compose smaller functions

---

## Recap

- Functions package reusable logic.
- Parameters are inputs; return value is output.
- Smaller focused functions are easier to test and maintain.

## Try it yourself

Write a function `greet(name)` that returns `"Hello, <name>"` and call it with
three different names.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Variables and Types](./02-variables-and-types.md) | [Foundations](./README.md) · [Home](../README.md) | [Making Decisions →](./04-making-decisions.md) |

</div>
