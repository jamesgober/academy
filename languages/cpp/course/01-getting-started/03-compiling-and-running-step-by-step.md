<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 01](./README.md)

---

# Compiling And Running Step By Step

New programmers often blur three different things together:

- writing code
- compiling code
- running the finished program

C++ makes those steps visible. That can feel annoying at first, but it is also
powerful because you learn exactly where a problem happens.

## The Three Files You Care About

Imagine this folder:

```text
cpp-practice/
  main.cpp
```

At first there is only one file. `main.cpp` is your source code.

After compiling, your folder may look like this:

```text
cpp-practice/
  main.cpp
  hello
```

or on Windows:

```text
cpp-practice/
  main.cpp
  hello.exe
```

The executable is the program your operating system can run.

## Step 1: Open A Terminal In The Project Folder

Your terminal has a current folder. Compiler commands look for files in that
folder unless you give a longer path.

Check where you are:

```bash
pwd
```

PowerShell:

```powershell
Get-Location
```

List the files:

```bash
ls
```

PowerShell:

```powershell
dir
```

You should see:

```text
main.cpp
```

If you do not see it, move to the right folder before compiling.

## Step 2: Compile The Source File

With GCC:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o hello
```

With Clang:

```bash
clang++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o hello
```

With Microsoft Visual C++:

```powershell
cl /std:c++20 /W4 /EHsc /Zi main.cpp
```

Read the command in pieces:

```text
g++              compiler program
-std=c++20       use the C++20 language version
-Wall            enable many warnings
-Wextra          enable more warnings
-Wpedantic       warn about non-standard code
-g               include debug information
main.cpp         source file to compile
-o hello         output executable named hello
```

The Microsoft command uses different flag names:

```text
cl               Microsoft compiler
/std:c++20       use the C++20 language version
/W4              enable strong warnings
/EHsc            use standard C++ exception handling mode
/Zi              include debug information
main.cpp         source file to compile
```

## Step 3: Read Compiler Output

If compilation succeeds, many compilers print little or nothing.

That can feel suspicious, but it is normal:

```text
no compiler output often means success
```

If compilation fails, the compiler prints errors. The error usually includes:

```text
file name
line number
column number
message
```

Example:

```text
main.cpp:4:42: error: expected ';' after expression
```

Read that as:

```text
In main.cpp, around line 4, the compiler expected a semicolon.
```

Do not panic at the whole wall of text. Start with the first error.

## Step 4: Run The Executable

On macOS or Linux:

```bash
./hello
```

On Windows PowerShell:

```powershell
.\hello.exe
```

If you used `cl` without specifying `/Fe`, the default executable name is based
on the source file:

```powershell
.\main.exe
```

The `./` or `.\` means "run the program from this folder."

## Compile-Time Errors Versus Runtime Behavior

Compile-time errors happen before your program runs.

```text
source code -> compiler finds a problem -> no successful executable
```

Runtime behavior happens after the program starts.

```text
source code -> compiler succeeds -> executable runs -> program does something
```

This distinction matters:

- Missing semicolon: compile-time error.
- Printing the wrong message: runtime behavior.
- Dividing by zero: often runtime behavior.
- Misspelling `std::cout`: compile-time error.

## A Good Beginner Build Loop

Use this pattern:

```text
1. Save the file.
2. Compile.
3. If errors appear, read the first one.
4. Fix the source file.
5. Compile again.
6. Run only after the compile succeeds.
```

Do not edit ten things at once while learning. Make one change, compile, then
make the next change.

Small steps keep errors understandable.

## Practice: Break It On Purpose

Start with:

```cpp
#include <iostream>

int main() {
    std::cout << "Practice build loop\n";
    return 0;
}
```

Compile and run it.

Now remove the semicolon:

```cpp
std::cout << "Practice build loop\n"
```

Compile again. It should fail.

Look for:

- the file name
- the line number
- the word `error`
- a hint about `;`

Put the semicolon back and compile again.

This exercise teaches an important emotional skill: compiler errors are not
judgment. They are feedback.

## Practice: Change The Output Name

Compile the same source file into an executable named `practice`:

```bash
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o practice
./practice
```

PowerShell:

```powershell
g++ -std=c++20 -Wall -Wextra -Wpedantic -g main.cpp -o practice.exe
.\practice.exe
```

The source file name and executable name do not have to match.

## Troubleshooting

### `command not found`

The compiler is not installed or not on your `PATH`.

Check:

```bash
g++ --version
clang++ --version
```

PowerShell:

```powershell
cl
```

For Microsoft C++, run `cl` inside a Developer PowerShell or Developer Command
Prompt, not an ordinary terminal.

### `main.cpp: No such file or directory`

The compiler cannot see your source file.

Use `pwd` or `Get-Location`, then `ls` or `dir`. Move into the folder that
contains `main.cpp`.

### The Old Program Keeps Running

You edited the source file but did not recompile, or you compiled one file and
ran a different executable.

Name your output clearly:

```bash
g++ -std=c++20 main.cpp -o hello
./hello
```

Then repeat the exact same executable name after each build.

### Warnings Appear But The Program Runs

Warnings mean "this compiled, but something looks suspicious."

Treat warnings as practice errors while learning. They catch many bugs before
you spend an hour wondering why your program behaves strangely.

## What You Should Be Able To Say Now

By the end of this lesson, you should be able to explain:

- `main.cpp` is the source file.
- The compiler turns source code into an executable.
- You run the executable, not the source file.
- Compile errors happen before the program runs.
- Runtime behavior happens after the program starts.
- Strict warnings help you find mistakes earlier.

---

[**Next ->** Reading Errors and Warnings](./04-reading-errors-and-warnings.md)  
[**<- Previous** Your First C++ Program](./02-your-first-cpp-program.md)
