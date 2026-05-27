<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# Objects in Programming

> An object is one specific "thing" in your program with data and behavior.

**You will learn:**
- What an object is in plain language
- How one class can create many different objects
- Why objects make code easier to reason about

**Before this page, you should know:**
- [What Is Programming?](./01-what-is-programming.md)

---

## Simple definition

An **object** is one concrete instance of something in your program.

If a class is a blueprint, an object is one built item from that blueprint.

## GTA car analogy (beginner-friendly)

In GTA, think about cars:

- There is a shared design for a car type.
- But in the world, you see many actual cars.
- Each car can have different properties:
  - color
  - speed
  - health/damage
  - position on the map

Each individual in-game car is an **object**.

So even if they come from the same model/type, they are not the same object.
They have their own current state.

> [!TIP]
> Ask "Which exact thing am I talking about right now?" If the answer is one
> specific thing, you are probably thinking about an object.

## Object state changes over time

Objects are useful because their values change as the program runs.

A GTA car object might start with:
- color: red
- health: 100
- speed: 0

After a crash, that same object might be:
- health: 42
- speed: 0

Same object, updated state.

## Where class fits in

Objects are created from a **class** definition. Class is covered next.

- Class: defines what data/behavior exist
- Object: one actual thing with real values now

---

## Recap

- An object is one specific instance in memory.
- Multiple objects can come from one class.
- Objects hold changing state while your program runs.

## Try it yourself

Pick a game object (car, player, weapon, mission). Write 5 properties it has
right now and 2 actions it can do.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Repeating Work](./05-repeating-work.md) | [Foundations](./README.md) · [Home](../README.md) | [Classes and Inheritance Basics →](./07-classes-and-inheritance-basics.md) |

</div>
