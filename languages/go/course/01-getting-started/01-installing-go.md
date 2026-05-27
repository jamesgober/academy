<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Installing Go

> Install the Go toolchain first so every later lesson has a stable base.

**You will learn:**
- What the `go` tool is
- How to install Go on your platform
- How to confirm the install worked

**Before this page, you should know:**
- [Terminal Basics](../../../../getting-started/terminal-basics.md)
- [Filesystem Navigation](../../../../getting-started/filesystem-navigation.md)
- Ideally [What Is Programming?](../../../../foundations/01-what-is-programming.md)

---

## What you are installing

Go is usually experienced as one main command: `go`.
That command handles several jobs for you:

- compiling programs
- running programs
- creating modules
- running tests
- formatting code
- downloading dependencies

You do not need to manage separate tools just to get started.
That simplicity is one of Go's strengths.

## Install it

Download Go from <https://go.dev/dl/> and use the installer for your platform.

### Step-by-step install checklist

1. Pick your OS package from the official downloads page.
2. Install with default options.
3. Open a brand-new terminal window.
4. Run `go version`.
5. If successful, continue to the first program lesson.

### Windows

Use the `.msi` installer, accept the defaults, and open a new PowerShell window
when installation finishes.

Verification commands:

```bash
go version
go env GOPATH
```

### macOS

Use the official package installer from the Go downloads page.

Verification commands:

```bash
go version
go env GOROOT
```

### Linux

Use the official archive or your package manager, but if you are learning from
course material, prefer the official release so your version matches the docs.

Verification commands:

```bash
go version
go env GOMODCACHE
```

> [!IMPORTANT]
> Keep the default install path unless you have a real reason to change it.

## Confirm it worked

Open a new terminal and run:

```bash
go version
go env GOMODCACHE
```

`go version` should print the installed version.
`go env GOMODCACHE` should print a path used by Go for downloaded modules.

If `go` is not recognized, close all terminals and open a fresh one so the new
`PATH` value is picked up.

If it still fails:
- re-run installer and confirm install completed
- check whether the terminal session is using an old shell profile
- run `go env` to verify toolchain visibility once command is available

> [!WARNING]
> An old terminal session is the most common reason a correct installation looks broken.

## VS Code setup

1. Open VS Code.
2. Install the Go extension: `golang.go`.
3. Let the extension install recommended Go tools when prompted.

This gives you formatting, navigation, quick fixes, and test integration.

---

## Recap

- Go is centered around the `go` command.
- Install from the official Go downloads page.
- Confirm installation with `go version`.
- Use the VS Code Go extension for a much better learning loop.

## Try it yourself

Run `go version`, then run `go env GOPATH` and `go env GOROOT` so you can see
that Go keeps toolchain and workspace information separate.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter 01](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Your First Go Program →](./02-your-first-go-program.md) |

</div>
