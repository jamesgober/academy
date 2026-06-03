<h1 align="center">
    <img width="99" alt="C++ logo" src="../../../../_assets/logos/cpp.svg">
    <br>
    <b>C++</b>
</h1>

[Home](../../../../README.md) / [C++](../../README.md) / [Chapter 01](./README.md)

---

# Your First C++ Program

This lesson is not about memorizing a magic block of code. It is about learning
what every tiny piece is doing so the program stops feeling like a spell.

Create a file named `main.cpp`:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, C++!\n";
    return 0;
}
```

When you run it, the program prints:

```text
Hello, C++!
```

## The Mental Model

A C++ program is text until a compiler translates it into an executable program.

```text
main.cpp
  |
  |  compiler checks and translates your code
  v
hello.exe or hello
  |
  |  operating system runs the executable
  v
program output
```

The file you edit is the source file. The file you run is the executable.
Changing `main.cpp` does not automatically change the executable. You must
compile again after editing.

## Line By Line

```cpp
#include <iostream>
```

`#include` copies in code from the C++ standard library before your program is
compiled. The `<iostream>` header gives you stream input and output tools such
as `std::cout`.

Think of a header as a menu of names the compiler needs to know about.

```cpp
int main() {
```

`main` is where your program starts. When the operating system launches your
program, C++ begins executing inside this function.

`int` means `main` returns an integer exit code. That exit code tells the
operating system whether the program finished successfully.

```cpp
{
    ...
}
```

Curly braces mark a block. The code between these braces belongs to `main`.
C++ uses braces heavily, so get used to reading them as "this code is grouped
together."

```cpp
std::cout << "Hello, C++!\n";
```

`std::cout` is the standard output stream. For a beginner, read it as "the
place normal text output goes."

The `<<` operator sends data into the stream:

```text
"Hello, C++!\n"  ->  std::cout  ->  terminal
```

The text inside double quotes is a string literal. It is literal text written
inside your program.

`\n` means newline. It moves the cursor to the next line after printing the
message.

```cpp
return 0;
```

`return 0` ends `main` and reports success. By convention:

- `0` means success.
- A nonzero number usually means failure.

## Semicolons

Most C++ statements end with a semicolon.

```cpp
std::cout << "Hello\n";
return 0;
```

A semicolon means "this statement is finished." Forgetting one is one of the
first beginner errors everyone makes.

```cpp
std::cout << "Hello\n"
return 0;
```

The compiler will complain because it does not know where the output statement
ended.

## `\n` Versus `std::endl`

You may see this version:

```cpp
std::cout << "Hello, C++!" << std::endl;
```

`std::endl` prints a newline and flushes the output buffer. Flushing means
"force any waiting output to be written right now."

For ordinary beginner programs, prefer `\n`:

```cpp
std::cout << "Hello, C++!\n";
```

It is simpler and usually faster. Use `std::endl` only when you specifically
need the flush behavior.

## Compile And Run

If you use GCC:

```bash
g++ -std=c++20 main.cpp -o hello
./hello
```

If you use Clang:

```bash
clang++ -std=c++20 main.cpp -o hello
./hello
```

On Windows PowerShell, the executable normally ends in `.exe`:

```powershell
.\hello.exe
```

If you use the Microsoft compiler from a Developer PowerShell:

```powershell
cl /std:c++20 /EHsc main.cpp
.\main.exe
```

Do not worry if these commands still feel a little mechanical. The next lesson
breaks the compile/run process down step by step.

## Change The Program

Edit your program:

```cpp
#include <iostream>

int main() {
    std::cout << "I can edit, compile, and run C++.\n";
    std::cout << "That loop is the core habit.\n";
    return 0;
}
```

Compile again, then run again.

This is the most important beginner workflow:

```text
edit -> compile -> read errors -> fix -> run -> repeat
```

## Common Beginner Mistakes

### Saving The File In The Wrong Folder

If this command says the file does not exist:

```bash
g++ -std=c++20 main.cpp -o hello
```

you are probably in the wrong terminal folder.

Use:

```bash
pwd
```

or on Windows PowerShell:

```powershell
Get-Location
```

Then list files:

```bash
ls
```

or:

```powershell
dir
```

You should see `main.cpp`.

### Using Curly Quotes

Code must use plain quotes:

```cpp
"Hello"
```

Do not use word-processor quotes:

```text
“Hello”
```

Curly quotes look nice in prose, but they are not valid C++ string delimiters.

### Forgetting `std::`

This will not work yet:

```cpp
cout << "Hello\n";
```

The stream is named `std::cout` because it lives in the standard namespace.
Namespaces prevent name collisions in larger programs.

You will learn more about namespaces later. For now, type `std::cout`.

### Running Old Output

If you changed the message but the old message still prints, you probably ran
the old executable without recompiling.

The fix is simple:

```text
edit source file
compile source file
run executable
```

## Mini Practice

Create a program that prints three lines:

```text
Name: Ada
Language: C++
Goal: Build useful software
```

Then change the name and goal to your own. Compile and run after each change.

The point is not the text. The point is building the habit of editing,
compiling, and running with confidence.

---

[**Next ->** Compiling and Running Step by Step](./03-compiling-and-running-step-by-step.md)  
[**<- Previous** Installing a C++ Compiler](./01-installing-a-cpp-compiler.md)
