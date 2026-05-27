<div align="center">

[Home](../README.md) · [Getting Started](./README.md)

</div>

---

# Terminal on Windows (PowerShell)

> PowerShell is the default shell recommended in Academy for Windows learners.

**You will learn:**
- How to open PowerShell
- How to run commands and navigate folders
- How to recover from common beginner mistakes

---

## Open PowerShell

1. Press Start.
2. Type `PowerShell`.
3. Open **Windows PowerShell**.

## First commands to know

```powershell
Get-Location
Get-ChildItem
Set-Location C:\Dev
```

- `Get-Location` prints your current folder.
- `Get-ChildItem` lists files and folders.
- `Set-Location` moves to a folder.

## Running tool checks

You will see checks like this in course pages:

```powershell
cargo --version
rustc --version
```

If a command is not recognized, close PowerShell and open a new window.

> [!IMPORTANT]
> Installers often update `PATH`. A new PowerShell session is required to use
> those updates.

## Next

- [Terminal Basics](./terminal-basics.md)
- [Filesystem Navigation](./filesystem-navigation.md)

---

<div align="center">

[← Getting Started](./README.md) · [Home](../README.md)

</div>
