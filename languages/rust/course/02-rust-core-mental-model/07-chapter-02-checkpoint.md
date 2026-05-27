<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Chapter 02 Checkpoint

> Confirm you can reason with Rust's core model before moving to testing and CI.

## Must-be-able checklist

- Explain ownership and moves without hand-waving.
- Explain why mutable borrowing is exclusive.
- Read a simple lifetime signature and describe constraints.
- Use structs/enums and `match` to model state.
- Return and propagate errors with `Result` and `?`.

## Practice task

Build a small "garage intake" module:
- input: car model and optional speed string
- parse speed with `Result`
- represent status as enum (`Accepted`, `Rejected(reason)`)
- process list of cars and print status messages

## Expected output characteristics

Your solution should:
- print a status line for each input car
- use enums to distinguish accepted and rejected cases
- avoid unnecessary cloning while passing values through the flow

## Reviewer checklist

- Can the learner explain the garage intake flow from input to output?
- Are ownership and borrowing choices justified?
- Is the error path understandable without extra explanation?

> [!IMPORTANT]
> If ownership/borrowing still feels mysterious, revisit lessons 02-04 before
> continuing. The next chapter assumes comfort with these rules.

---

## Next

Continue to [Chapter 03 — Testing, Edge Cases, and CI](../03-testing-edge-cases-and-ci/README.md).

---

[**Next ->** Chapter Testing, Edge Cases, and CI](../03-testing-edge-cases-and-ci/README.md)  
[**<- Previous** Error Handling with Option and Result](./06-error-handling-with-option-and-result.md)
