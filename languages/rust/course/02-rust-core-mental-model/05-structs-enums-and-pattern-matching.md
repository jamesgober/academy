<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Structs, Enums, and Pattern Matching

> Rust data modeling is strongest when you model valid states directly.

**You will learn:**
- Structs for grouped fields
- Enums for one-of-many states
- Pattern matching with `match`

**Before this page, you should know:** [Lifetimes in Plain Language](./04-lifetimes-in-plain-language.md)

---

## Struct example

```rust
struct Car {
    model: String,
    max_speed: u32,
}
```

Use structs when data has all fields together.

## Enum example

```rust
enum CarState {
    Parked,
    Driving(u32),
    Broken(String),
}
```

Use enums when value can be exactly one variant.

## Pattern matching

```rust
fn describe(state: CarState) {
    match state {
        CarState::Parked => println!("Car is parked"),
        CarState::Driving(speed) => println!("Driving at {}", speed),
        CarState::Broken(reason) => println!("Broken: {}", reason),
    }
}
```

`match` is exhaustive: every variant must be handled.

> [!IMPORTANT]
> Enums + exhaustive matching remove many invalid-state bugs found in other languages.

---

## Recap

- Structs model combined fields.
- Enums model exclusive variants.
- `match` enforces handling every valid variant.

## Try it yourself

Model a download state enum with variants like `Pending`, `InProgress(u8)`, and
`Failed(String)`, then print a message for each using `match`.

---

[**Next ->** Error Handling with Option and Result](./06-error-handling-with-option-and-result.md)  
[**<- Previous** Lifetimes in Plain Language](./04-lifetimes-in-plain-language.md)
