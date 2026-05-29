<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 01](./README.md)

---

# Installing Rust

> Rust is installed through `rustup`, the official Rust toolchain installer and
> version manager.

**You will learn:**
- What `rustup`, `rustc`, and `cargo` do
- How to install Rust on Windows, macOS, and Linux
- How to verify your install
- How to update Rust
- How to install common developer components
- What to do when the terminal cannot find Cargo

**Before this page, you should know:**
- [Terminal Basics](../../../../getting-started/terminal-basics.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)
- On Windows: [PowerShell Basics](../../../../getting-started/terminal-windows-powershell.md)

---

## The Three Tools

Installing Rust gives you several tools. These three matter first:

| Tool | Plain meaning | How often you use it |
|---|---|---|
| `rustup` | Installer and version manager | Sometimes |
| `rustc` | Compiler | Rarely directly |
| `cargo` | Project manager and build tool | Constantly |

Mental model:

```text
rustup installs and updates Rust
        |
        v
cargo manages projects
        |
        v
rustc compiles code
```

Most days, you will type `cargo`, not `rustc`.

---

## Install On Windows

1. Open <https://rustup.rs>.
2. Download and run `rustup-init.exe`.
3. Accept the default installation.
4. If the installer asks for Visual Studio C++ Build Tools, install them.
5. Close and reopen PowerShell.

Why Visual Studio C++ Build Tools?

Rust on Windows needs a linker. A linker connects compiled pieces into a final
program. You do not need to understand linking deeply yet, but you do need the
tool installed.

Recommended beginner shell:

```text
PowerShell
```

Avoid mixing too many shells at first. One shell keeps command examples easier
to follow.

---

## Install On macOS And Linux

Open a terminal and run:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Accept the default installation.

When it finishes, close and reopen your terminal.

Why reopen the terminal?

The installer updates your `PATH`. New terminal windows read the updated
environment. Old terminal windows may not.

---

## Verify The Install

In a fresh terminal:

```bash
cargo --version
rustc --version
rustup --version
```

Expected shape:

```text
cargo 1.x.x (...)
rustc 1.x.x (...)
rustup 1.x.x (...)
```

Your exact versions will differ. That is fine.

Also run:

```bash
cargo new install-check
cd install-check
cargo run
```

Expected final line:

```text
Hello, world!
```

This proves more than `--version`. It proves Cargo can create, compile, link,
and run a project.

---

## If `cargo` Is Not Found

Error examples:

```text
cargo: command not found
```

```text
'cargo' is not recognized as an internal or external command
```

Try this first:

1. Close every terminal window.
2. Open a new terminal.
3. Run `cargo --version` again.

If it still fails, Cargo's bin directory may not be on `PATH`.

Common install locations:

```text
Windows: %USERPROFILE%\.cargo\bin
macOS/Linux: $HOME/.cargo/bin
```

Beginner translation:

```text
PATH is the operating system's search list for commands.
If Cargo's folder is not in PATH, the shell cannot find cargo.
```

---

## Update Rust

Rust releases regularly. Update with:

```bash
rustup update
```

Check active toolchain:

```bash
rustup show
```

Most learners should stay on stable:

```bash
rustup default stable
```

Channels:

| Channel | Use it when |
|---|---|
| `stable` | Normal learning and production work |
| `beta` | Testing upcoming Rust releases |
| `nightly` | Experimental features or special tools |

Use stable unless a lesson, tool, or project specifically says otherwise.

---

## Install Useful Components

The default rustup profile normally includes `rustfmt` and `clippy`, but you can
install them explicitly:

```bash
rustup component add rustfmt clippy
```

What they do:

| Component | Command | Purpose |
|---|---|---|
| `rustfmt` | `cargo fmt` | Formats Rust code |
| `clippy` | `cargo clippy` | Finds common mistakes and improvements |
| `rust-docs` | `rustup doc` | Opens local Rust documentation |

Open offline docs:

```bash
rustup doc
```

This is one of Rust's superpowers: excellent docs installed locally.

---

## Editor Setup

Recommended beginner editor:

```text
VS Code + rust-analyzer
```

Install these extensions:

| Extension | Why |
|---|---|
| `rust-lang.rust-analyzer` | Completion, errors, go-to-definition |
| `tamasfe.even-better-toml` | Better `Cargo.toml` editing |
| `vadimcn.vscode-lldb` | Debugging support |

Open a project folder:

```bash
code .
```

If `code .` does not work, open VS Code manually and use:

```text
File -> Open Folder
```

Open the folder containing `Cargo.toml`, not only the `src` folder.

---

## Folder Safety For Beginners

Create a practice folder somewhere easy:

Windows PowerShell:

```powershell
mkdir $HOME\rust-practice
cd $HOME\rust-practice
```

macOS/Linux:

```bash
mkdir -p ~/rust-practice
cd ~/rust-practice
```

Do not practice inside:

- System folders
- Program Files
- Random downloaded zip folders
- Folders synced by tools that constantly lock files, unless you know they work

Start boring. Boring setup is good setup.

---

## Common Installation Problems

| Problem | Likely cause | Fix |
|---|---|---|
| `cargo` not found | Old terminal or missing `PATH` | Reopen terminal, check Cargo bin path |
| Windows linker error | Missing C++ Build Tools | Install Visual Studio C++ Build Tools through rustup prompt |
| `cargo new` works but `cargo run` fails | Linker or antivirus interference | Read first error line; verify build tools |
| VS Code shows no Rust help | Opened wrong folder or missing rust-analyzer | Open folder with `Cargo.toml`; install extension |
| Commands work in one shell but not another | Shell profiles differ | Use one shell while learning |

---

## Mini Project: Installation Receipt

Create a text file named `rust-install-check.txt` and record:

```text
cargo version:
rustc version:
rustup version:
practice folder:
editor:
one thing I fixed or learned:
```

This sounds small, but it trains a professional habit: record your environment
when setup matters.

---

## Chapter Checkpoint

You should now be able to answer:

- What does `rustup` do?
- What does `cargo` do?
- Why do you rarely run `rustc` directly?
- Why do you reopen the terminal after installing Rust?
- What command updates Rust?
- What do `cargo fmt` and `cargo clippy` rely on?
- What folder should VS Code open?

---

## Recap

- Install Rust with `rustup`.
- Work day to day with `cargo`.
- Verify setup with versions and a real `cargo run`.
- Keep the stable toolchain unless you have a reason to change.
- Install or verify `rustfmt`, `clippy`, and local docs.
- Use VS Code with rust-analyzer if you want a beginner-friendly editor.

## Try It Yourself

Run:

```bash
rustup update
rustup component add rustfmt clippy
rustup doc
```

Then create and run a fresh project named `install-check`.

---

[**Next ->** Create Your First Cargo Project](./02-first-cargo-project.md)  
[**<- Previous** Chapter 01: Getting Started](./README.md)
