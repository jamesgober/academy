# Installing a C++ Compiler

Install a compiler first so all later C++ lessons run consistently.

## What a compiler does

A C++ compiler translates `.cpp` source files into machine code your OS can run.
Without a compiler, you cannot run C++ programs.

## Common compiler choices

- GCC (`g++`)
- Clang (`clang++`)
- MSVC (`cl`)

You only need one to start.

## Windows setup

### Option A: Visual Studio Build Tools (MSVC)

1. Install Visual Studio Build Tools.
2. Include the "Desktop development with C++" workload.
3. Open "Developer PowerShell".
4. Run:

```bash
cl
```

If installed correctly, you should see MSVC version output.

### Option B: MinGW-w64 (GCC)

1. Install MinGW-w64 distribution.
2. Add its `bin` folder to `PATH`.
3. Open new terminal and run:

```bash
g++ --version
```

## macOS setup

Install Xcode Command Line Tools:

```bash
xcode-select --install
```

Then verify:

```bash
clang++ --version
```

## Linux setup

Install compiler package for your distro (GCC or Clang), then verify:

```bash
g++ --version
```

or

```bash
clang++ --version
```

## Installation checks

Run one of:

```bash
g++ --version
clang++ --version
cl
```

At least one should print version info.

## Beginner troubleshooting

- reopen terminal after install
- verify PATH updates
- check command spelling (`g++`, not `gcc` for C++)
- use one compiler consistently while learning

> [!IMPORTANT]
> Switching compilers every lesson adds confusion. Stay consistent until your
> C++ fundamentals are stable.

---

[← Chapter 01](./README.md) · [C++](../../README.md)
