<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

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

**Before this page, you should know:**
- [Terminal Basics](../../../../getting-started/terminal-basics.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)
- Ideally [What Is Programming?](../../../../foundations/01-what-is-programming.md)

> [!NOTE]
> This page assumes you can open a terminal and run commands. If that part feels shaky, read the linked getting-started pages first.

> [!TIP]
> On Windows, use [PowerShell](../../../../getting-started/terminal-windows-powershell.md) as your default terminal while learning.

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

> [!IMPORTANT]
> Keep the default install options unless you have a specific reason to change them.
> Defaults are tested and documented by the Rust team.

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

> [!WARNING]
> Running install commands in an old terminal session is the most common cause of
> false "it didn't install" errors.

## Set up VS Code for Rust (recommended)

Once Rust is installed, configure your editor for completion, navigation, and
debugging support.

1. Open VS Code in your project folder (`code .`).
2. Install these extensions:
  - `rust-lang.rust-analyzer`
  - `vadimcn.vscode-lldb`
  - `tamasfe.even-better-toml`
3. Optionally install `usernamehw.errorlens` for clearer inline diagnostics.

> [!TIP]
> Keep your extension list minimal at first. Add tools only when they solve a
> specific pain point.

---

## Recap

- Rust is installed via **`rustup`**, which manages **`rustc`** (the compiler)
  and **`cargo`** (the build tool you'll actually use).
- macOS/Linux install with a single `curl` command; Windows uses the installer
  from rustup.rs.
- Confirm with `cargo --version` and `rustc --version` in a new terminal.
- Configure VS Code with `rust-analyzer`, `CodeLLDB`, and TOML support.

## Try it yourself

Run `cargo --version`. Then run `rustup doc` — it opens the complete Rust
documentation in your browser, fully offline. You won't read it now, but knowing
it's there is useful.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter 01](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Create Your First Cargo Project →](./02-first-cargo-project.md) |

</div>
