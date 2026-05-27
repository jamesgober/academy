<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 01](./README.md)

---

# Create Your First Cargo Project

> Start with Cargo from day one so your projects use the same layout, commands, and dependency workflow as real Rust codebases.

**You will learn:**
- what Cargo does and why Rust projects use it
- how to create, run, and inspect a binary project
- what each generated file means
- how `Cargo.toml` connects project metadata, edition, dependencies, and build behavior
- how to troubleshoot the first common setup failures

**Before this page, you should know:**
- [Installing Rust](./01-installing-rust.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)

---

## Why Cargo Exists

Cargo is Rust's project manager. It creates projects, downloads libraries, builds
code, runs tests, formats code, and stores repeatable dependency versions.

You can compile one file with `rustc`, the Rust compiler, but real projects
quickly need more than "turn this one file into a program." They need a standard
folder layout, a place to list dependencies, a test command, and a reliable way
for teammates or future you to rebuild the same project.

Visual model:

```text
You write Rust files
        |
        v
Cargo reads Cargo.toml -----> Cargo.lock records exact dependency versions
        |
        v
Cargo calls rustc
        |
        v
Executable or library
```

## Create a Binary Project

A binary project produces an executable program. From a practice folder, run:

```bash
cargo new hello-rust
cd hello-rust
cargo run
```

Expected output:

```text
   Compiling hello-rust v0.1.0 (.../hello-rust)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in ...
     Running `target/debug/hello-rust`
Hello, world!
```

The timing and path will differ on your machine. The important part is that the
last line prints `Hello, world!`.

> [!TIP]
> Use lowercase, hyphen-separated package names such as `hello-rust` or
> `inventory-cli`. Cargo accepts underscores too, but hyphens are common for
> package names and repository names.

## What Cargo Created

Your project now looks like this:

```text
hello-rust/
|-- Cargo.toml
|-- Cargo.lock
|-- src/
|   `-- main.rs
`-- .gitignore
```

`Cargo.toml` is the manifest: the configuration file Cargo reads for package
metadata, dependencies, editions, features, and build targets.

`Cargo.lock` records exact dependency versions. Applications should commit it so
builds stay repeatable. Libraries usually commit it only when the repository also
builds examples, binaries, or applications.

`src/main.rs` is the binary crate root. A crate is the smallest unit Rust
compiles. A binary crate becomes a runnable program.

`.gitignore` keeps build output out of version control.

## Read the Generated Code

Open `src/main.rs`:

```rust
fn main() {
    println!("Hello, world!");
}
```

`fn` starts a function definition. A function is a named block of code.

`main` is the special function Rust runs first in a binary program.

`println!` prints text and a newline. The `!` means it is a macro, not a regular
function. A macro can generate Rust code before compilation.

Now change it:

```rust
fn main() {
    let language = "Rust";
    println!("I am learning {language} with Cargo.");
}
```

Run it again:

```bash
cargo run
```

Expected output:

```text
I am learning Rust with Cargo.
```

## Read the Generated Cargo.toml

Open `Cargo.toml`:

```toml
[package]
name = "hello-rust"
version = "0.1.0"
edition = "2024"

[dependencies]
```

The exact edition may differ depending on your installed Rust version and Cargo
defaults. The edition chooses which Rust language edition the compiler uses.
Editions let Rust improve over time without breaking old projects.

Required for a normal local project:

| Field | Required? | Meaning |
|---|---:|---|
| `name` | yes | Package name used by Cargo and dependencies. |
| `version` | yes for publishing | Package version, usually semantic versioning like `0.1.0`. |
| `edition` | strongly recommended | Rust edition for parsing and language behavior. |
| `[dependencies]` | no entries required | External crates your code uses. |

Useful optional fields:

| Field | Use it when |
|---|---|
| `description` | You plan to publish or share the crate. |
| `license` | You want users to know legal reuse terms. |
| `repository` | You want docs/crates.io to link back to source. |
| `readme` | Your README is not the default `README.md`. |
| `rust-version` | You want to state the minimum supported Rust version. |

Example publish-ready metadata:

```toml
[package]
name = "inventory-cli"
version = "0.1.0"
edition = "2024"
rust-version = "1.85"
description = "A small command-line inventory tracker."
license = "MIT OR Apache-2.0"
repository = "https://github.com/your-name/inventory-cli"
readme = "README.md"

[dependencies]
```

See the reference later: [Cargo Manifest and Config](../../reference/cargo-manifest-and-config.md).

## Add Your First Dependency

A dependency is a library your project uses. Rust libraries are usually called
crates. The main public registry is [crates.io](https://crates.io/), and the API
documentation hub is [docs.rs](https://docs.rs/).

Add the `rand` crate:

```bash
cargo add rand
```

Cargo updates `Cargo.toml` and `Cargo.lock`. Your `[dependencies]` section will
look similar to this, though the version may differ:

```toml
[dependencies]
rand = "0.9"
```

Use it in `src/main.rs`:

```rust
use rand::Rng;

fn main() {
    let mut rng = rand::rng();
    let number = rng.random_range(1..=10);

    println!("Random number: {number}");
}
```

Run:

```bash
cargo run
```

You should see a number from 1 through 10.

> [!NOTE]
> If `cargo add` is unavailable, update Rust with `rustup update`. Modern Cargo
> includes it. You can also edit `Cargo.toml` by hand, but beginners should use
> `cargo add` first because it reduces typo mistakes.

## Common First Errors

### `cargo` is not recognized

PowerShell or your terminal cannot find Cargo.

Check:

```bash
cargo --version
```

If that fails, restart the terminal. If it still fails, Rust may not be on your
`PATH`, which is the operating system's list of folders to search for commands.
Return to [Installing Rust](./01-installing-rust.md).

### `could not find Cargo.toml`

You ran a Cargo command outside the project folder.

Fix:

```bash
cd hello-rust
cargo run
```

Cargo commands that build, test, or run a project need to be inside a folder
that contains `Cargo.toml` or one of its child folders.

### Dependency version or download failure

If adding a dependency fails, read the first error line and check:

- Are you online?
- Did you spell the crate name correctly?
- Does the crate exist on [crates.io](https://crates.io/)?
- Are you behind a company proxy that needs Cargo config?

## Practice: Make It Yours

Change the program to print:

- your name
- a random number from 100 through 999
- a sentence explaining what Cargo did

Example final output:

```text
James is learning Rust.
Practice number: 417
Cargo built the project and ran target/debug/hello-rust.
```

---

## Recap

- Cargo is the normal way to create, build, run, test, and manage Rust projects.
- `Cargo.toml` describes the package and its dependencies.
- `src/main.rs` is where a binary program starts.
- `cargo run` builds and runs the project.
- `cargo add <crate>` adds a library from the Rust ecosystem.

## Try It Yourself

Create a new project named `number-practice`, add `rand`, print three random
numbers, and explain in a comment what `Cargo.toml` and `Cargo.lock` do.

---

[**Next ->** Rust Project Structure](./03-rust-project-layout.md)  
[**<- Previous** Installing Rust](./01-installing-rust.md)
