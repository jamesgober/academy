<div align="center">

[Home](../README.md) · [Getting Started](./README.md)

</div>

---

# Setting Up Your Dev Folder

> A place for every project, and every project in its place.

**You will learn:**
- Why a single, consistent projects folder saves you constant friction
- A folder layout that works on any platform
- How to navigate it quickly from a terminal

**Before this page, you should know:** how to open a terminal on your platform.

---

## Why this matters

If your projects are scattered across the Desktop, Downloads, and three random
folders, you'll waste time hunting for them and your tools won't know where to
look. One dedicated root folder fixes that permanently.

## The layout

Create a single top-level folder for all code. The exact name is yours, but
keep it short and at a path you can type quickly:

| Platform | Recommended root |
|----------|------------------|
| Windows | `C:\Dev` |
| macOS | `~/Dev` |
| Linux | `~/Dev` |

Inside it, group by purpose rather than dumping everything flat:

```
Dev/
├── learning/      practice projects and course exercises
├── projects/      your real projects
└── sandbox/       throwaway experiments
```

## Create it

**Windows (PowerShell):**

```powershell
mkdir C:\Dev\learning, C:\Dev\projects, C:\Dev\sandbox
```

**macOS / Linux:**

```bash
mkdir -p ~/Dev/{learning,projects,sandbox}
```

## Navigate it fast

From a terminal, `cd` ("change directory") moves you into a folder:

```bash
cd ~/Dev/projects
```

On Windows PowerShell it's the same command:

```powershell
cd C:\Dev\projects
```

Tip: open the whole folder in your editor at once with `code .` (the `.` means
"the current folder"). That's the fastest way to start working.

---

## Recap

- Keep one root folder for all code.
- Group by purpose: learning, projects, sandbox.
- Use `cd` to move around and `code .` to open a folder in your editor.

---

<div align="center">

[← Getting Started](./README.md) · [Home](../README.md)

</div>
