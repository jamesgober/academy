<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 01](./README.md)

---

# Chapter 01 Checkpoint

This checkpoint proves you can perform the basic C++ workflow without guessing:

```text
create file -> compile -> read errors -> fix -> run
```

Do not rush this. Every C++ project you ever build depends on this loop.

## Goal

Create a tiny program, compile it with useful warnings, run it, intentionally
break it, read the compiler message, fix it, and run it again.

## Step 1: Create A Practice Folder

Create this folder:

```text
chapter01-checkpoint/
```

Inside it, create:

```text
chapter01-checkpoint/
  hello.cpp
```

## Step 2: Write The Program

Put this in `hello.cpp`:

```cpp
#include <iostream>

int main() {
    std::cout << "C++ checkpoint started.\n";
    std::cout << "I can compile and run a program.\n";
    return 0;
}
```

Before compiling, point at each part and say what it does:

- `#include <iostream>` gives access to `std::cout`.
- `int main()` is the program entry point.
- The braces contain the body of `main`.
- `std::cout` prints text to standard output.
- `\n` moves to the next line.
- `return 0` reports success.

## Step 3: Compile

Use the command for your compiler.

GCC:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g hello.cpp -o hello
```

Clang:

```bash
clang++ -std=c++20 -Wall -Wextra -Wpedantic -g hello.cpp -o hello
```

Microsoft C++:

```powershell
cl /std:c++20 /W4 /EHsc /Zi hello.cpp
```

If the compiler prints errors, fix them before continuing.

## Step 4: Run

macOS or Linux:

```bash
./hello
```

Windows PowerShell with GCC or Clang:

```powershell
.\hello.exe
```

Windows PowerShell with Microsoft C++:

```powershell
.\hello.exe
```

Expected output:

```text
C++ checkpoint started.
I can compile and run a program.
```

## Step 5: Break It On Purpose

Remove the semicolon from this line:

```cpp
std::cout << "C++ checkpoint started.\n"
```

Compile again.

The program should not compile. Find the first error and write down:

```text
file:
line:
message:
what I think it means:
```

You are training yourself to read compiler output instead of avoiding it.

## Step 6: Fix And Recompile

Put the semicolon back:

```cpp
std::cout << "C++ checkpoint started.\n";
```

Compile again. Then run again.

The final run should work.

## Step 7: Make One Real Change

Change the program to print your own name and one reason you are learning C++:

```cpp
#include <iostream>

int main() {
    std::cout << "Name: Ada\n";
    std::cout << "Reason: I want to understand how software really runs.\n";
    return 0;
}
```

Compile and run after the edit.

## Must-Be-Able Checklist

You are ready for Chapter 02 when you can do all of this without copying blindly:

- Create a `.cpp` file in a project folder.
- Open a terminal in that folder.
- Check your current folder with `pwd` or `Get-Location`.
- List files with `ls` or `dir`.
- Compile a C++ file with a C++20 flag.
- Explain the difference between a source file and an executable.
- Run the executable from the terminal.
- Read the first compiler error before reading the rest.
- Fix a missing semicolon error.
- Explain why warnings are helpful.

## Stretch Practice

Add a third output line:

```text
Next skill: variables
```

Then intentionally misspell `std::cout` as `std::cut`. Compile and read the
error. Fix it and compile again.

The goal is not to memorize every possible message. The goal is to become calm
when the compiler talks back.

---

[**Next ->** Track Overview](../../README.md)  
[**<- Previous** Reading Errors and Warnings](./04-reading-errors-and-warnings.md)
