<div align="center">

[Home](../README.md) · [Getting Started](./README.md)

</div>

---

# Linux Setup

> Install a code editor and get ready to write code on Linux.

**You will learn:**
- How to install Visual Studio Code
- How to confirm it works
- Where to go next

---

## 1. Install Visual Studio Code

The cleanest method on most desktop distributions is the official package.

**Debian / Ubuntu:**

```bash
sudo apt update
sudo apt install -y wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/packages.microsoft.gpg
echo "deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install -y code
```

**Fedora:**

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf install -y code
```

<!-- SCREENSHOT: VS Code running on a Linux desktop -->

## 2. Confirm it works

From a terminal, run:

```bash
code --version
```

If it prints a version number, you're set. You can also launch it from your
desktop's application menu.

> [!TIP]
> When working from a project folder, run `code .` to open the whole folder in VS Code.

---

## Next

- Learn terminal workflow: [Terminal on Linux](./terminal-linux.md)
- Learn folder navigation: [Filesystem Navigation](./filesystem-navigation.md)
- Learn editor workflow: [VS Code Basics](./vscode-basics.md)
- Organize your projects: [Setting Up Your Dev Folder](./dev-folder-setup.md).
- Install a language toolchain from a [language course](../languages/).

---

<div align="center">

[← Getting Started](./README.md) · [Home](../README.md)

</div>
