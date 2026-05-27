<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Wiki and Long-Form Project Knowledge

> Wiki is useful for evolving guides that are too large for README.

**You will learn:**
- When to use Wiki vs docs folder
- How to keep wiki content discoverable
- How to avoid split-documentation confusion

**Before this page, you should know:** [GitHub Pages for Project Docs](./03-github-pages-for-project-docs.md)

---

## Wiki vs docs folder

Use Wiki when:
- you need collaborative long-form internal/external guides
- frequent edits are expected from multiple contributors

Use `docs/` in repository when:
- docs changes must be reviewed in normal PR flow
- docs and code need strict version coupling

> [!WARNING]
> Splitting docs between Wiki and `docs/` without a policy can confuse users.

## Keep navigation clear

- Add a docs map page.
- Link core pages from README.
- Archive outdated pages instead of silent deletion.

---

## Recap

- Wiki is a tool, not a default requirement.
- Choose one primary docs source and document that choice.
- Clear linking prevents "where is the real doc" confusion.

## Try it yourself

Write a one-page documentation policy deciding when your project uses Wiki vs
`docs/` folder.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← GitHub Pages for Project Docs](./03-github-pages-for-project-docs.md) | [Chapter 05](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Tags, Releases, and Release Notes →](./05-tags-releases-and-release-notes.md) |

</div>
