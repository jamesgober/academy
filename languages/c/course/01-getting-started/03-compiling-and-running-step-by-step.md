<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 01](./README.md)

---

# Compiling And Running Step By Step

C is a compiled language. That means your source file must be translated before
the operating system can run it.

This lesson makes the workflow explicit:

```text
write source -> compile source -> run executable
```

## Start In The Right Folder

Suppose your folder looks like this:

```text
c-practice/
  main.c
```

Open a terminal in `c-practice`.

Check your current folder:

```bash
pwd
```

PowerShell:

```powershell
Get-Location
```

List files:

```bash
ls
```

PowerShell:

```powershell
dir
```

You should see:

```text
main.c
```

If the compiler says `main.c` does not exist, your terminal is probably in the
wrong folder.

## Compile With Useful Warnings

Use a command like this with GCC or Clang:

```bash
cc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o hello
```

If your compiler command is named `gcc`:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o hello
```

If your compiler command is named `clang`:

```bash
clang -std=c17 -Wall -Wextra -Wpedantic -g main.c -o hello
```

Read the command in pieces:

```text
cc                 compiler command
-std=c17           use the C17 language version
-Wall              enable many warnings
-Wextra            enable more warnings
-Wpedantic         warn about non-standard code
-g                 include debug information
main.c             source file to compile
-o hello           output executable named hello
```

Warnings are important. A warning means "this code compiled, but something
looks suspicious." While learning, treat warnings as problems to understand, not
noise to ignore.

## Run The Executable

On macOS or Linux:

```bash
./hello
```

On Windows PowerShell:

```powershell
.\hello.exe
```

The `./` or `.\` means "run the program in this folder."

## Compile Output Versus Program Output

When compilation succeeds, your compiler may print nothing.

That is normal.

```text
compiler output: maybe empty
program output: appears when you run the executable
```

If you see:

```text
Hello, C
```

that came from your program, not from the compiler.

## Compile-Time Problems

Compile-time problems happen before the program runs.

Examples:

- missing semicolon
- misspelled function name
- missing header
- syntax error

Break the program on purpose:

```c
#include <stdio.h>

int main(void) {
    printf("Hello, C\n")
    return 0;
}
```

Compile again. You should get an error near the missing semicolon.

When reading compiler output, look for:

```text
file name
line number
error or warning
message
```

Start with the first error. Later errors are often side effects of the first
one.

## Runtime Behavior

Runtime behavior happens after the program successfully starts.

This compiles:

```c
#include <stdio.h>

int main(void) {
    printf("Hlelo, C\n");
    return 0;
}
```

But it prints a typo:

```text
Hlelo, C
```

The compiler usually cannot know that you meant `Hello`. That is not a syntax
error. It is program behavior.

## A Reliable Beginner Workflow

Use this loop:

```text
1. Edit one small thing.
2. Save the file.
3. Compile.
4. Read the first error or warning.
5. Fix the issue.
6. Compile again.
7. Run after compilation succeeds.
```

Small changes are easier to debug than large mystery changes.

## Troubleshooting

### `cc: command not found`

Your compiler is not installed or not on your terminal `PATH`.

Try:

```bash
gcc --version
clang --version
cc --version
```

On Windows, you may need to install a compiler toolchain or use a developer
terminal for the compiler you installed.

### `main.c: No such file or directory`

Your terminal is not in the folder that contains `main.c`.

Check:

```bash
pwd
ls
```

PowerShell:

```powershell
Get-Location
dir
```

### The Program Did Not Change

You edited `main.c`, but you ran an old executable.

Compile again:

```bash
cc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o hello
```

Then run the same output file:

```bash
./hello
```

## Practice

Create this program:

```c
#include <stdio.h>

int main(void) {
    printf("Compile step complete\n");
    printf("Run step complete\n");
    return 0;
}
```

Then do this:

1. Compile it.
2. Run it.
3. Remove one semicolon.
4. Compile again and read the first error.
5. Put the semicolon back.
6. Compile again.
7. Run again.

That loop is the foundation of learning C.

---

[**Next ->** What Source Files and Executables Are](./04-what-source-files-and-executables-are.md)  
[**<- Previous** Your First C Program](./02-your-first-c-program.md)
