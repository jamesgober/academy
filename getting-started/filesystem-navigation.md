<div align="center">

[Home](../README.md) · [Getting Started](./README.md)

</div>

---

# Filesystem Navigation

> Move through folders confidently so terminal-based tutorials stay easy to follow.

**You will learn:**
- What files, folders, and paths are
- The minimum commands to move around safely
- How to check where you are before running commands

---

## The mental model

- A **folder** (directory) can contain files and other folders.
- A **path** is the location of a file or folder.
- Your terminal always has a **current directory** (where commands run).

> [!IMPORTANT]
> Always confirm your current directory before running commands that create,
> delete, or move files.

## Core navigation commands

| Goal | Windows PowerShell | macOS / Linux |
|------|--------------------|---------------|
| Show current folder | `Get-Location` | `pwd` |
| List files/folders | `Get-ChildItem` | `ls` |
| Move into folder | `Set-Location <path>` | `cd <path>` |
| Move up one level | `Set-Location ..` | `cd ..` |

## Example flow

Windows PowerShell:

```powershell
Get-Location
Set-Location C:\Dev\learning
Get-ChildItem
```

macOS / Linux:

```bash
pwd
cd ~/Dev/learning
ls
```

## Paths and spaces

If a folder name includes spaces, wrap the path in quotes:

```text
cd "C:\My Projects\academy"
```

> [!WARNING]
> Running commands in the wrong folder is a common way to lose time and create
> confusing errors.

## Next

- [Setting Up Your Dev Folder](./dev-folder-setup.md)
- [Terminal Basics](./terminal-basics.md)

---

<div align="center">

[← Getting Started](./README.md) · [Home](../README.md)

</div>
