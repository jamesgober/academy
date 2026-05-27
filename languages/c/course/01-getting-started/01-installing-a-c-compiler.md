<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Installing a C Compiler

> Before you can write C, you need a compiler that turns C source into a runnable program.

**You will learn:**
- What a C compiler does
- Which common compiler families exist
- How to confirm your install worked

**Before this page, you should know:**
- [Terminal Basics](../../../../getting-started/terminal-basics.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)

---

## What the compiler does

A compiler reads your `.c` source file and produces machine code the computer can run.

Common choices:
- `gcc`
- `clang`
- Microsoft's C compiler through Visual Studio Build Tools

## Where to get compilers

- MSVC Build Tools (Windows): <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
- LLVM/Clang downloads: <https://releases.llvm.org/download.html>
- MSYS2 (Windows package manager with GCC/Clang): <https://www.msys2.org/>

## Beginner guidance

Use whichever compiler is easiest to install on your machine first.
The goal is to get a working loop, not to start compiler wars.

## Platform install commands

Windows (MSVC):
1. Download from <https://visualstudio.microsoft.com/visual-cpp-build-tools/>.
2. Install the "Desktop development with C++" workload.
3. Open Developer PowerShell and run:

```bash
cl
```

Windows (GCC via MSYS2):

```bash
pacman -Syu
pacman -S --needed mingw-w64-ucrt-x86_64-gcc
gcc --version
```

macOS (Clang):

```bash
xcode-select --install
clang --version
```

Ubuntu or Debian:

```bash
sudo apt update
sudo apt install -y build-essential clang
gcc --version
```

Fedora:

```bash
sudo dnf install -y gcc clang
gcc --version
```

Arch:

```bash
sudo pacman -S --needed gcc clang
gcc --version
```

## Confirm it worked

Try one of these commands depending on your setup:

```bash
gcc --version
clang --version
cl
```

At least one should print version information.

## Visual model

```mermaid
flowchart LR
    A[Write hello.c] --> B[Compile with gcc or clang or cl]
    B --> C[Link into executable]
    C --> D[Run in terminal]
```

---

## Recap

- A compiler turns C source into a program.
- `gcc`, `clang`, and `cl` are common choices.
- Confirm the install before writing code.

## Try it yourself

Run your compiler version command and note which compiler family you are using.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter 01](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Your First C Program →](./02-your-first-c-program.md) |

</div>
