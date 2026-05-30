<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 01](./README.md)

---

# Installing A C Compiler

> C source code is plain text. A compiler turns that text into a program your
> computer can run. Your first goal is not to pick the "perfect" compiler. Your
> first goal is to get a boring, repeatable edit-build-run loop.

**You will learn:**
- What a C compiler does
- What compiling and linking mean
- Which compiler families are common
- How to install or find one on your platform
- How to verify the install
- How to choose beginner-safe commands

**Before this page, you should know:**
- [Terminal Basics](../../../../getting-started/terminal-basics.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)

---

## The Beginner Mental Model

```text
hello.c
  |
  | compile
  v
object code
  |
  | link with runtime/libraries
  v
executable program
  |
  | run
  v
output in terminal
```

When you run:

```bash
gcc hello.c -o hello
```

you are asking GCC to compile and link in one command.

That is perfect for beginner programs.

---

## Common Compiler Families

| Compiler | Common platform | Notes |
|---|---|---|
| GCC | Linux, MSYS2/MinGW, many systems | very common in C learning material |
| Clang | macOS, Linux, Windows via LLVM | excellent diagnostics |
| MSVC `cl` | Windows Visual Studio tools | common for Windows C/C++ development |

This course shows GCC/Clang style commands most often:

```bash
gcc main.c -o app
clang main.c -o app
```

And MSVC where useful:

```powershell
cl main.c
```

---

## Windows Option 1: Visual Studio Build Tools

Use this if you want Microsoft's compiler.

1. Install Visual Studio Build Tools.
2. Select the "Desktop development with C++" workload.
3. Open "Developer PowerShell" or "Developer Command Prompt."
4. Run:

```powershell
cl
```

Expected:

```text
Microsoft (R) C/C++ Optimizing Compiler...
```

`cl` may show an error about no input files. That is fine. It means the compiler
exists.

---

## Windows Option 2: MSYS2 GCC

Use this if you want GCC on Windows.

Install MSYS2, then in the correct MSYS2 shell:

```bash
pacman -Syu
pacman -S --needed mingw-w64-ucrt-x86_64-gcc
gcc --version
```

Expected:

```text
gcc ...
```

Beginner notice:

```text
Windows can have multiple terminals. Make sure you are using the terminal where
your compiler is actually on PATH.
```

---

## macOS

Install command-line tools:

```bash
xcode-select --install
```

Verify:

```bash
clang --version
```

On macOS, `gcc` may actually point to Clang. That is normal.

---

## Ubuntu Or Debian

```bash
sudo apt update
sudo apt install -y build-essential clang
gcc --version
clang --version
```

`build-essential` installs the common C build tools.

---

## Fedora

```bash
sudo dnf install -y gcc clang
gcc --version
clang --version
```

---

## Arch

```bash
sudo pacman -S --needed gcc clang
gcc --version
clang --version
```

---

## Verify With A Tiny Program

Create `hello.c`:

```c
#include <stdio.h>

int main(void) {
    printf("Hello, C!\n");
    return 0;
}
```

Compile with GCC:

```bash
gcc hello.c -o hello
```

Run on macOS/Linux/MSYS2:

```bash
./hello
```

Run on Windows PowerShell if it produced `hello.exe`:

```powershell
.\hello.exe
```

Compile with Clang:

```bash
clang hello.c -o hello
```

Compile with MSVC:

```powershell
cl hello.c
.\hello.exe
```

---

## Common First Install Problems

### Problem: Command Not Found

```text
gcc: command not found
```

Means:

```text
The shell cannot find gcc on PATH.
```

Fix:

- confirm the compiler is installed
- open the terminal that came with the toolchain
- restart the terminal after installation
- check installation instructions for PATH setup

### Problem: `cl` Works Only In Developer Prompt

MSVC tools often require a Developer Command Prompt or Developer PowerShell.

If normal PowerShell cannot find `cl`, open the developer terminal.

### Problem: Program Compiles But Does Not Run

Check the output filename:

```bash
gcc hello.c -o hello
```

creates `hello` or `hello.exe` depending on platform.

Run from the same folder.

---

## Beginner Compiler Choice

Pick one working path:

```text
Windows and Visual Studio installed? Use cl.
Windows and MSYS2 installed? Use gcc.
macOS? Use clang.
Linux? Use gcc or clang.
```

Do not switch compilers every five minutes while learning basics.

Consistency helps you learn error messages.

---

## Chapter Checkpoint

You should now be able to answer:

- What does a compiler do?
- What does linking do?
- What command verifies GCC?
- What command verifies Clang?
- What command verifies MSVC?
- Why might `cl` require a Developer Prompt?
- What does `-o hello` control?

---

## Recap

- C needs a compiler before code can run.
- GCC, Clang, and MSVC are common choices.
- Beginner commands usually compile and link in one step.
- Verify the compiler before continuing.
- Use one working setup consistently while learning.

## Try It Yourself

Install or locate one compiler, run its version command, compile `hello.c`, and
write down:

- compiler name
- version command used
- compile command used
- run command used

---

[**Next ->** Your First C Program](./02-your-first-c-program.md)  
[**<- Previous** Chapter Start](./README.md)
