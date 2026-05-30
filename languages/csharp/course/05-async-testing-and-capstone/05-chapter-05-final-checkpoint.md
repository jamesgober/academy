<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 05](./README.md)

---

# Chapter 05 Final Checkpoint

> This checkpoint verifies that you can move from beginner syntax to a practical
> C# workflow: build, test, debug, persist data, and explain your design.

---

## Skills You Should Have Now

You should be able to:

- create a solution with app, library, and test projects
- define classes that protect their rules
- use records for simple value-like data
- choose collections by access pattern
- use LINQ for readable queries
- handle expected user input failure without crashing
- throw precise exceptions for broken method contracts
- read and write JSON
- write async methods without `.Result` or `.Wait()`
- write xUnit facts and theories
- test exceptions and async methods
- explain where interfaces make testing easier

---

## Capstone Sign-Off Checklist

Your Order Tracker is ready when:

- `dotnet build` succeeds
- `dotnet test` succeeds
- console app can create an order
- console app can add a line item
- console app can show totals
- app can save orders to JSON
- app can load saved orders
- invalid quantities are rejected
- duplicate order ids are rejected
- tests cover total calculation
- tests cover invalid `OrderItem` creation
- tests cover duplicate order ids
- architecture note explains project layers

---

## Explanation Prompts

Answer these in your own words:

- Why is `OrderTracker.Core` separate from `OrderTracker.App`?
- Why is `OrderBook` backed by `Dictionary<int, Order>`?
- Why does `Order` expose `IReadOnlyList<OrderItem>` instead of `List<OrderItem>`?
- Why does `JsonOrderStore` use DTOs?
- Where does async help?
- Which parts should be tested without the console app?
- What bad input should be handled without throwing a scary stack trace at the user?

---

## Debugging Prompts

If something breaks, ask:

1. Does the project build?
2. Does the failing test describe the behavior clearly?
3. Is the failure in domain logic, file IO, or console parsing?
4. Is the JSON file missing, malformed, or shaped differently than expected?
5. Did an async method get called without `await`?
6. Did a nullable result get used without a null check?

---

## Stretch Path

After the baseline works, improve one thing at a time:

- safer console parsing with `TryParse`
- command help screen
- delete order command
- update item quantity command
- load orders at startup
- save automatically on exit
- logging with `ILogger`
- test JSON round-trip with temp files
- replace JSON file with SQLite later

Do not add all stretch work at once. Add one behavior, test it, then move on.

---

## Final Reflection

Write a short retrospective:

```md
# C# Capstone Retrospective

## What I Built

## What Was Hard

## What I Fixed

## What I Would Improve Next

## C# Concepts I Can Explain Now
```

This helps turn the project from copied code into learned skill.

---

## Recap

- Engineering quality is process plus code.
- Good C# code separates domain rules from IO.
- Collections, exceptions, async, and tests work together in real apps.
- Build and test commands are part of the workflow, not an afterthought.
- You now have a practical C# baseline for larger projects.

## Try It Yourself

Write the retrospective, then choose exactly one stretch improvement and add a
test for it before implementing it.

---

[**Next ->** Track Overview](../../README.md)  
[**<- Previous** Capstone: Console Order Tracker](./04-capstone-console-order-tracker.md)
