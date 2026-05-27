<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Install Git and Create Your GitHub Account

> You need both Git (local tooling) and a GitHub account (online collaboration) to follow this track.

**You will learn:**
- How to install Git
- How to configure your identity for commits
- How to create and secure a GitHub account

**Before this page, you should know:**
- [What Is GitHub?](./01-what-is-github.md)

---

## Install Git

### Windows

Download from <https://git-scm.com/download/win> and accept default options.

<!-- SCREENSHOT: Git for Windows installer with default options selected -->

### macOS

Install Xcode Command Line Tools:

```bash
xcode-select --install
```

### Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install -y git
```

## Confirm installation

```bash
git --version
```

## Configure identity

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

> [!IMPORTANT]
> Use an email associated with your GitHub account if you want commits linked to your profile.

## Create your GitHub account

1. Go to <https://github.com/signup>.
2. Choose a username you can keep long-term.
3. Enable two-factor authentication in account security settings.

<!-- SCREENSHOT: GitHub account security page showing 2FA setup -->

> [!WARNING]
> Do not skip two-factor authentication. It protects your account and repositories.

## Related reference

- [GitHub Troubleshooting and Recovery](../../reference/github-troubleshooting-and-recovery.md)

---

## Recap

- Git runs locally; GitHub account lives online.
- You installed Git and configured commit identity.
- You created a GitHub account and secured it.

## Try it yourself

Run:

```bash
git config --global --list
```

Confirm your `user.name` and `user.email` values are correct.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← What Is GitHub?](./01-what-is-github.md) | [Chapter 01](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Create Your First Repository →](./03-create-your-first-repository.md) |

</div>
