<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 01](./README.md)

---

# Compiling and Running Step by Step

> In C, you usually compile first and run second. Keeping those steps separate helps you understand the toolchain.

**You will learn:**
- How to compile a C file
- How to run the resulting program
- Why compile errors and runtime behavior are separate concerns

**Before this page, you should know:** [Your First C Program](./02-your-first-c-program.md)

---

## Example compile command

```bash
gcc main.c -o hello
```

This reads `main.c` and creates an executable named `hello`.

## Then run it

```bash
./hello
```

On Windows, the produced file may be `hello.exe`.

## Mental model

- source file: what you write
- compiler step: transforms source into program
- executable: what the computer runs

---

## Recap

- C uses an explicit compile step.
- The compiler creates the executable.
- Running happens after compilation succeeds.

## Try it yourself

Compile your first program, then intentionally remove a semicolon and recompile so you can see the difference between success and compiler failure.

---

[**Next ->** What Source Files and Executables Are](./04-what-source-files-and-executables-are.md)
[**<- Previous** Your First C Program](./02-your-first-c-program.md)


