<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Common Concurrency Mistakes

> Concurrency bugs are often coordination bugs, not syntax bugs.

**You will learn:**
- Why programs can deadlock or race
- Why shared mutable state is dangerous
- How to think more carefully before adding concurrency

**Before this page, you should know:** [Channels and Message Passing](./02-channels-and-message-passing.md)

---

## Common beginner mistakes

- launching goroutines without a plan for completion
- sending or receiving on channels in the wrong order
- sharing mutable data without coordination
- assuming concurrent code always runs faster

## Plain-language warning signs

- "sometimes it works, sometimes it hangs"
- "the output order changes in weird ways"
- "I added concurrency before the single-threaded version was clear"

> [!WARNING]
> Do not add goroutines first and ask architecture questions later.

---

## Recap

- Concurrency mistakes usually involve coordination.
- Shared mutable state deserves extra caution.
- Clear simpler code should come before concurrent code.

## Try it yourself

Write down two reasons a simple program might not need concurrency at all.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Channels and Message Passing](./02-channels-and-message-passing.md) | [Chapter 05](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [A Small Worker Pattern →](./04-a-small-worker-pattern.md) |

</div>
