<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Installing Rust

> Rust is installed through `rustup`, a tool that manages the compiler and
> keeps it up to date.

**You will learn:**
- What `rustup`, `rustc`, and `cargo` are
- How to install Rust on your platform
- How to confirm the install worked

**Before this page, you should know:** how to open a terminal, and ideally
[what a compiler is](../../../../foundations/01-what-is-programming.md).

---

## The three tools you're installing

Installing Rust gives you three programs. It's worth knowing what each does so
the rest of the course isn't a mystery:

- **`rustup`** — the *installer and version manager*. You rarely call it after
  setup; it just keeps Rust updated and can switch between versions.
- **`rustc`** — the *compiler*. It turns your Rust source code into a runnable
  program. You'll almost never call it directly.
- **`cargo`** — the *project manager and build tool*. This is the one you
  actually use day to day: it compiles your code, runs it, downloads libraries,
  and runs tests. Cargo calls `rustc` for you.

In short: you install with `rustup`, and you work with `cargo`.

## Install it

### macOS and Linux

Open a terminal and run:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Follow the prompt — pressing Enter to accept the default installation is the
right choice for almost everyone. When it finishes, close and reopen your
terminal so the new tools are picked up.

### Windows

Download and run the installer from <https://rustup.rs>. It will check for the
Visual Studio C++ build tools (Rust needs them to link programs) and walk you
through installing them if they're missing. Accept the defaults.

<!-- SCREENSHOT: rustup-init running on Windows -->

## Confirm it worked

In a fresh terminal, run:

```bash
cargo --version
rustc --version
```

Each should print a version number, like `cargo 1.x.x`. If both do, Rust is
installed and you're ready to write code.

If you instead get "command not found," close every terminal window and open a
new one — the installer updates your `PATH`, but only new terminals see the
change.

---

## Recap

- Rust is installed via **`rustup`**, which manages **`rustc`** (the compiler)
  and **`cargo`** (the build tool you'll actually use).
- macOS/Linux install with a single `curl` command; Windows uses the installer
  from rustup.rs.
- Confirm with `cargo --version` and `rustc --version` in a new terminal.

## Try it yourself

Run `cargo --version`. Then run `rustup doc` — it opens the complete Rust
documentation in your browser, fully offline. You won't read it now, but knowing
it's there is useful.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| ← — | [Chapter 01](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | Your First Program → *(coming soon)* |

</div>
