<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Chapter 05 Capstone

> Demonstrate a disciplined C workflow with explicit memory-safety handling.

## Project brief

Build a small C module (for example, record storage or sensor buffer) that includes:
- struct-based data model
- array iteration and updates
- at least one dynamically allocated buffer
- explicit cleanup paths
- strict compile flags and sanitizer run notes

## Expected outputs

Your project should show:
- successful strict compile
- sanitizer-clean run
- no leak in normal execution path
- clear ownership note for heap allocations

## Reviewer checklist

- Is memory ownership explicit for each allocation?
- Are all allocations freed exactly once?
- Are risky paths (error returns) still cleaned up?
- Are quality commands documented and repeatable?

---

## Next

Use the C reference for fast lookup, then move to project-level exercises.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Quality Workflow and Release Checklist](./05-quality-workflow-and-release-checklist.md) | [Chapter 05](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [C Reference →](../../reference/README.md) |

</div>
