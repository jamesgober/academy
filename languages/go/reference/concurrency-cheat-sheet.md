<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Concurrency Cheat Sheet

> Fast lookup for first-pass Go concurrency concepts.

## Quick terms

- **goroutine**: lightweight concurrent task
- **channel**: typed path for sending values
- **send**: `ch <- value`
- **receive**: `value := <-ch`

## Beginner warnings

- concurrency is not automatically faster
- shared mutable state is risky
- unclear shutdown/completion logic causes bugs

## Quick diagnostic prompts

- what starts the goroutine?
- what tells it to stop?
- where do results go?
- is a channel or simpler single-threaded code enough?

---

<div align="center">

[Reference Index](./README.md)  /  [Go](../README.md)  /  [Home](../../../README.md)

</div>



