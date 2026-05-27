<div align="center">

[Home](../README.md) · [Foundations](./README.md)

</div>

---

# What Is Programming?

> A program is a list of instructions a computer follows, one step at a time.

**You will learn:**
- What a program actually is
- What happens when code "runs"
- The difference between source code and a running program

**Before this page, you should know:** nothing. This is the beginning.

---

## A program is a recipe

Imagine writing instructions for someone who is extremely fast, never gets
tired, and follows directions *exactly* — but has zero common sense. If you say
"add salt," they'll ask how much. If you forget to say "stop," they'll keep
going forever.

That is a computer. A **program** is the recipe you write for it: a precise,
ordered list of instructions. Each instruction is small ("add these two
numbers," "show this text on screen"). On their own they're trivial. Stacked
together in the right order, they become a game, a website, or a database.

The job of programming is breaking a big goal ("show the user their email")
into instructions small enough that a machine with no common sense can follow
them without guessing.

## Source code vs. the running program

When you write instructions, you type them as text in a file. That text is
called **source code** — it's the recipe on paper. Here is real source code in
a language called Python:

```python
print("Hello, world.")
```

That single line is an instruction: *show the text "Hello, world." on screen.*

But text in a file doesn't *do* anything by itself, the same way a recipe
sitting on the counter doesn't cook dinner. Something has to read the
instructions and carry them out. When that happens, we say the program
**runs**, and the running program is called a **process**.

So there are two states worth naming clearly:

- **Source code** — the instructions, written as text, sitting in a file.
- **A running program (process)** — those instructions actively being carried
  out by the computer right now.

## What "running" actually does

Computers ultimately only understand extremely simple commands expressed in
numbers — this is called **machine code**. Humans don't write machine code by
hand because it's unbearable. Instead we write friendly source code like the
line above, and a translator turns it into something the machine can execute.

There are two common kinds of translator, and the difference matters later:

- A **compiler** translates your entire source file into machine code *ahead of
  time*, producing a standalone program you can run. (Rust, C, and Go work this
  way.)
- An **interpreter** reads your source code and carries it out *line by line, as
  it goes*, with no separate build step. (Python and JavaScript work this way.)

You don't need to memorize that yet. Just hold onto the idea: the text you
write is not what the machine runs — something translates it first.

---

## Recap

- A **program** is an ordered list of precise instructions for a computer.
- The computer follows them exactly and has no common sense, so the
  instructions must be complete and unambiguous.
- **Source code** is those instructions as text in a file; a **running program**
  (a **process**) is those instructions being carried out.
- A **compiler** translates everything ahead of time; an **interpreter**
  translates and runs line by line.

## Try it yourself

You don't need to install anything. Open [the Python sandbox in your
browser](https://www.python.org/shell/), type the line below, and press Enter:

```python
print("Hello, world.")
```

You just wrote source code and ran it. The text appeared because the
interpreter carried out your instruction. That's the entire loop, in miniature.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| ← — | [Foundations](./README.md) · [Home](../README.md) | Variables & Types → *(coming soon)* |

</div>
