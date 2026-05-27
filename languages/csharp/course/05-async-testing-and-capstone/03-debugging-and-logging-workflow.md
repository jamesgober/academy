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

# Debugging and Logging Workflow

> Professional debugging is a repeatable process, not random edits.

**You will learn:**
- A practical bug triage sequence
- How to combine debugger and logs
- How to isolate root causes faster

**Before this page, you should know:** build output and exception handling.

---

## Triage sequence

1. Reproduce bug reliably.
2. Capture exact input and expected behavior.
3. Set breakpoints near suspected logic.
4. Inspect variable values and call flow.
5. Add temporary logs to confirm assumptions.
6. Fix root cause, not symptoms.
7. Add/adjust tests to prevent regression.

## Logging tip

Use structured logs when possible:

```csharp
logger.LogInformation("Order {OrderId} processed for {CustomerId}", orderId, customerId);
```

Structured logs are easier to query than string-concatenated logs.

---

## Recap

- Reproduction is step zero.
- Debugger plus logging beats guesswork.
- Add tests after fixes to lock in behavior.

## Try it yourself

Pick one prior exercise, simulate a failure, and document your step-by-step triage.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Unit Testing with xUnit](./02-unit-testing-with-xunit.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Capstone: Console Order Tracker →](./04-capstone-console-order-tracker.md) |

</div>
