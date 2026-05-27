<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# Classes and Inheritance Basics

> A class defines a type of thing. Objects are the real things created from it.

**You will learn:**
- What a class is
- How subclasses extend a base class
- How to use inheritance without confusion

**Before this page, you should know:**
- [Objects in Programming](./06-objects-in-programming.md)

---

## Class in plain language

A **class** is a definition of:
- what data a thing stores
- what actions that thing can perform

A class is not one car in the game. It is the "car type" definition.

## GTA analogy for class vs object

Imagine a base class called `Car`.

`Car` might define:
- `color`
- `max_speed`
- `health`
- actions like `accelerate()` and `brake()`

Now imagine actual cars spawned in GTA world:
- one red sports car
- one black SUV
- one damaged taxi

Those are objects created from a car class.

## Subclass and extension (inheritance)

A **subclass** starts from a base class and adds or customizes behavior.

Example concept:
- Base class: `Car`
- Subclass: `PoliceCar`

`PoliceCar` gets normal car features and adds:
- siren state
- action `toggle_siren()`

This is called **inheritance**: child class reuses parent class behavior.

> [!IMPORTANT]
> Subclasses should represent a true "is-a" relationship.
> A police car is a car. A garage is not a car.

## Common beginner confusion

Confusion usually comes from mixing these terms:

- Class: definition/template
- Object: one concrete instance
- Subclass: a more specific class based on another class

If this still feels fuzzy, go back to [Objects in Programming](./06-objects-in-programming.md)
and map one base class with two concrete objects.

---

## Recap

- Class defines data and behavior.
- Objects are real instances created from classes.
- Subclasses extend base classes when relationship is truly "is-a".

## Try it yourself

Design a small class tree for a game:
- Base class: `Car`
- Subclasses: `RaceCar`, `PoliceCar`
- List 3 shared properties and 1 subclass-specific action each.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Objects in Programming](./06-objects-in-programming.md) | [Foundations](./README.md) · [Home](../README.md) | [Foundations Capstone Checkpoint →](./08-foundations-capstone-checkpoint.md) |

</div>
