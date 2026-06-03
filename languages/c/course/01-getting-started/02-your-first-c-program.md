<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 01](./README.md)

---

# Your First C Program

C is small, direct, and unforgiving in useful ways. It makes you see the basic
parts of a program clearly: source files, functions, statements, memory, and the
compiler.

Start with one tiny file and understand every line.

## Create `main.c`

Make a new folder for practice:

```text
c-practice/
  main.c
```

Put this in `main.c`:

```c
#include <stdio.h>

int main(void) {
    printf("Hello, C\n");
    return 0;
}
```

When compiled and run, it prints:

```text
Hello, C
```

## The Big Picture

Your C source file is text. The computer cannot run it directly.

```text
main.c
  |
  |  compiler checks and translates it
  v
hello.exe or hello
  |
  |  operating system runs it
  v
program output
```

This matters because editing `main.c` does not automatically change the
executable. After every edit, compile again.

## Line By Line

```c
#include <stdio.h>
```

`#include` asks the preprocessor to include declarations from another file
before compilation. `stdio.h` is the standard input/output header. It tells the
compiler about functions such as `printf`.

Read it as:

```text
I want to use standard input/output functions.
```

```c
int main(void) {
```

`main` is where a C program starts.

`int` means `main` returns an integer exit code to the operating system.

`void` means `main` takes no command-line arguments in this version.

The `{` starts the body of the function.

```c
printf("Hello, C\n");
```

`printf` prints formatted text to standard output, which usually means your
terminal.

The text inside quotes is a string literal:

```text
"Hello, C\n"
```

`\n` is a newline character. It moves output to the next line.

The semicolon ends the statement.

```c
return 0;
```

This exits `main` and reports success.

By convention:

- `0` means the program succeeded.
- nonzero usually means something failed.

```c
}
```

The closing brace ends the body of `main`.

## Visual Model: What `printf` Does

```text
your code
  |
  v
printf("Hello, C\n")
  |
  v
standard output
  |
  v
terminal window
```

At this stage, you do not need to know how `printf` works internally. You only
need to know that it sends text to output and that C needs `stdio.h` to recognize
it correctly.

## Semicolons Matter

This is a complete statement:

```c
printf("Hello, C\n");
```

This is not:

```c
printf("Hello, C\n")
```

C uses semicolons to separate many statements. Forgetting one is normal at
first. The goal is not to avoid every mistake; the goal is to learn how to read
what the compiler tells you.

## Common Beginner Mistakes

### Using The Wrong File Extension

Use:

```text
main.c
```

Avoid:

```text
main.txt
main.cpp
main
```

`.c` tells your editor and compiler that this is C source code.

### Forgetting The Header

This may compile with warnings or errors depending on your compiler settings:

```c
int main(void) {
    printf("Hello\n");
    return 0;
}
```

Always include the correct header:

```c
#include <stdio.h>
```

### Using Curly Quotes

C needs plain double quotes:

```c
"Hello"
```

Curly quotes from word processors are not valid C:

```text
"Hello"
```

### Running Old Code

If you changed the message but the old message still appears, you probably did
not recompile.

Use this loop:

```text
edit -> save -> compile -> run
```

## Try It Yourself

Change the program so it prints three lines:

```text
Name: Ada
Language: C
Goal: Understand computers better
```

The code will look like this:

```c
#include <stdio.h>

int main(void) {
    printf("Name: Ada\n");
    printf("Language: C\n");
    printf("Goal: Understand computers better\n");
    return 0;
}
```

Then replace the name and goal with your own.

Compile and run after each small change.

## What You Should Be Able To Explain

Before moving on, make sure you can answer:

- What is the source file?
- What is the executable?
- Where does a C program start?
- Why does this program include `stdio.h`?
- What does `printf` do?
- What does `\n` mean?
- What does `return 0` mean?

If you can explain those in plain English, you are not just copying C. You are
starting to read it.

---

[**Next ->** Compiling and Running Step by Step](./03-compiling-and-running-step-by-step.md)  
[**<- Previous** Installing a C Compiler](./01-installing-a-c-compiler.md)
