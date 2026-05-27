<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Capstone: Console Order Tracker

> Build a small but realistic C# project that combines OOP, collections, async, and testing.

**You will learn:**
- How to shape a small project structure
- How to separate domain logic from IO
- How to define completion criteria

**Before this page, you should know:** all prior chapters.

---

## Suggested structure

```text
order-tracker/
├── src/
│   ├── OrderTracker.App/
│   └── OrderTracker.Core/
└── tests/
    └── OrderTracker.Core.Tests/
```

## Features

- create orders
- add line items
- calculate totals
- save and load from JSON
- validate input and report clear errors

## Completion checklist

- `dotnet build` clean
- `dotnet test` passing
- no unresolved warnings you do not understand
- one short architecture note explaining type choices

---

## Recap

- Real projects combine multiple language features.
- Separation of concerns keeps code testable.
- Completion criteria must be explicit.

## Try it yourself

Implement one vertical slice: create order -> add line -> show total.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Debugging and Logging Workflow](./03-debugging-and-logging-workflow.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 05 Final Checkpoint →](./05-chapter-05-final-checkpoint.md) |

</div>
