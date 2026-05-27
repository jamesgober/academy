<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# GitHub Troubleshooting and Recovery

> Common problems, likely causes, and safe recovery steps.

## Push rejected (non-fast-forward)

Cause: remote branch advanced since your last pull.

Fix:

```bash
git pull --rebase
git push
```

## Accidentally committed wrong files

Fix before push:

```bash
git reset --soft HEAD~1
```

Then recommit correctly.

## Need to rewrite your own branch history

```bash
git push --force-with-lease
```

> [!WARNING]
> Use force push only on your branch and never on protected shared branches.

## Detached HEAD confusion

```bash
git switch -c fix/recover-detached-head
```

This preserves your current work on a new branch.

## Merge conflict after syncing main

1. Open conflicted files.
2. Resolve markers.
3. Run:

```bash
git add .
git commit
```

## Release tag created on wrong commit

If not pushed:

```bash
git tag -d v0.5.0
```

If pushed:

```bash
git push --delete origin v0.5.0
git tag -d v0.5.0
```

Recreate tag on correct commit.

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
