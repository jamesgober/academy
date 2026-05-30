<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 05](./README.md)

---

# Debugging And Logging Workflow

> Professional debugging is a repeatable process. The goal is not to change code
> until the symptom disappears. The goal is to understand the failure and fix the
> smallest real cause.

**You will learn:**
- How to reproduce a bug
- How to isolate a failing path
- How to read stack traces
- How to use breakpoints
- What to log
- Why structured logging matters
- How to turn a bug into a test

**Before this page, you should know:** [Unit Testing With xUnit](./02-unit-testing-with-xunit.md)

---

## Debugging Loop

Use this sequence:

1. Reproduce the bug.
2. Write down expected behavior.
3. Write down actual behavior.
4. Find the smallest input that still fails.
5. Inspect the first suspicious boundary.
6. Fix the root cause.
7. Add or update a test.
8. Rerun the build and tests.

This keeps you from making random edits and losing track of what changed.

---

## Read Stack Traces

Example:

```text
System.ArgumentOutOfRangeException: Quantity must be positive.
   at OrderTracker.Core.OrderItem..ctor(String sku, Int32 quantity, Decimal unitPrice)
   at OrderTracker.App.Program.Main()
```

Read from the top:

```text
exception type: ArgumentOutOfRangeException
message: Quantity must be positive
where thrown: OrderItem constructor
caller: Program.Main
```

The top app frame is usually where the failure was detected.

The caller frames show how the program got there.

---

## Breakpoints

Set breakpoints at:

- method entry
- validation branches
- before state changes
- before file reads/writes
- catch blocks
- places where nullable values are used

Inspect:

- parameter values
- object state
- collection counts
- return values
- exception details

---

## Logging Mental Model

Debuggers help while you are attached.

Logs help after the fact.

Good logs answer:

```text
What happened?
Which object/request/order was involved?
Was the operation successful?
If it failed, what context helps reproduce it?
```

---

## Structured Logging

Prefer:

```csharp
logger.LogInformation(
    "Order {OrderId} processed for customer {CustomerId}",
    orderId,
    customerId
);
```

Over:

```csharp
logger.LogInformation(
    "Order " + orderId + " processed for customer " + customerId
);
```

Why?

Structured logs keep `OrderId` and `CustomerId` as queryable fields in logging
systems.

Even if you only use console logs now, learn the professional habit early.

---

## Logging Levels

| Level | Use |
|---|---|
| Trace | very detailed diagnostic flow |
| Debug | debugging details useful during development |
| Information | normal important events |
| Warning | unexpected but recoverable issue |
| Error | operation failed |
| Critical | app/system may be unusable |

Beginner rule:

```text
Do not log everything as Error.
Use the level that matches severity.
```

---

## Console App Logging Without A Framework

For small beginner apps, a tiny helper is enough:

```csharp
public static class AppLog
{
    public static void Info(string message)
    {
        Console.WriteLine($"[info] {message}");
    }

    public static void Warning(string message)
    {
        Console.WriteLine($"[warn] {message}");
    }

    public static void Error(string message)
    {
        Console.WriteLine($"[error] {message}");
    }
}
```

Use:

```csharp
AppLog.Info("Loading orders.");
AppLog.Warning("No orders file found; starting empty.");
AppLog.Error("Could not save orders.");
```

Later, use `Microsoft.Extensions.Logging` for real structured logging.

---

## Real Debugging Example

Bug:

```text
Adding quantity 0 crashes the app.
```

Expected:

```text
The app should print "Quantity must be positive" and keep running.
```

Actual:

```text
Unhandled ArgumentOutOfRangeException.
```

Root cause:

```text
Program.cs parses input and directly creates OrderItem.
OrderItem correctly throws.
Program.cs does not catch and translate the exception.
```

Fix at app boundary:

```csharp
try
{
    order.AddItem(new OrderItem(sku, quantity, unitPrice));
    Console.WriteLine("Item added.");
}
catch (ArgumentException ex)
{
    Console.WriteLine($"Could not add item: {ex.Message}");
}
```

Then add a test for `OrderItem` validation so the domain rule stays protected.

---

## Turn Bugs Into Tests

Bug report:

```text
Duplicate order IDs are accepted.
```

Test:

```csharp
[Fact]
public void Add_RejectsDuplicateOrderIds()
{
    var book = new OrderBook();

    Assert.True(book.Add(new Order(1001)));
    Assert.False(book.Add(new Order(1001)));
}
```

Now the bug has a permanent guard.

---

## Common Mistakes

### Mistake 1: Fixing Symptoms

If a null value crashes later, do not only add a null check at the crash site.
Ask why the unexpected null got there.

### Mistake 2: Logging Secrets

Do not log:

- passwords
- tokens
- full credit card numbers
- private personal data unless truly required and protected

### Mistake 3: No Correlation Data

`"Save failed"` is less useful than:

```text
Save failed for orders file data/orders.json
```

Include enough context to reproduce.

---

## Chapter Checkpoint

You should now be able to answer:

- What is the first step in debugging?
- What does a stack trace show?
- Where should you set breakpoints?
- What makes a log useful?
- Why is structured logging better than string concatenation?
- What should you avoid logging?
- How do you convert a bug into a regression test?

---

## Recap

- Debugging starts with reproduction.
- Stack traces show exception type, message, and call path.
- Breakpoints help inspect live state.
- Logs help understand behavior later.
- Structured logs preserve useful fields.
- Bug fixes should often come with tests.

## Try It Yourself

Pick one prior exercise, introduce a bad input bug, and document:

- expected behavior
- actual behavior
- smallest reproduction
- stack trace or observed failure
- root cause
- test added after the fix

---

[**Next ->** Capstone: Console Order Tracker](./04-capstone-console-order-tracker.md)  
[**<- Previous** Unit Testing With xUnit](./02-unit-testing-with-xunit.md)
