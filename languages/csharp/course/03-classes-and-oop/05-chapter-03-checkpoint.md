<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 03](./README.md)

---

# Chapter 03 Checkpoint

> Validate your object modeling decisions before moving into data-heavy programming.

## You should be able to do all of this

- Design a class with private fields and validated properties
- Use constructors to keep objects valid
- Program against interfaces
- Explain class vs record vs struct choice

## Design challenge

Model a small `Invoice` domain:
- one type for invoice header
- one type for line items
- one interface for output formatting

Justify which types are classes versus records.

---

## Recap

- OOP quality is mostly design quality.
- Interfaces reduce coupling.
- Type choice should match data semantics.

## Try it yourself

Refactor one class to use an interface dependency instead of direct concrete type usage.

---

[**Next ->** Collections, Exceptions, and Data](../04-collections-exceptions-and-data/README.md)  
[**<- Previous** Records, Structs, and When to Use Them](./04-records-structs-and-when-to-use-them.md)


