<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Tags, Releases, and Release Notes

> Tags mark version points; releases package those points with human-readable notes.

**You will learn:**
- Difference between tags and releases
- How to publish a release safely
- How to write useful release notes

**Before this page, you should know:** [Wiki and Long-Form Project Knowledge](./04-wiki-and-long-form-project-knowledge.md)

---

## Tags vs releases

- **Tag**: version pointer in Git history (for example `v0.4.0`).
- **Release**: GitHub artifact built on a tag with notes and attachments.

## Tagging flow

```bash
git tag v0.4.0
git push origin v0.4.0
```

Then create a GitHub release for that tag.

## Release labels and pre-release versions

Use pre-release forms like:

```text
x.y.z-alpha.0
x.y.z-beta.0
x.y.z-rc.0
```

Example:

```text
1.0.0-beta.1
```

> [!NOTE]
> See the SemVer specification: <https://semver.org>.

## Release notes example

```markdown
## v0.4.0 - 2026-05-27
### Added
- Added GitHub troubleshooting reference page.

### Changed
- Reworked chapter navigation for beginner clarity.

### Fixed
- Corrected broken relative links in chapter index.

### Notes
- This release is pre-1.0 and may contain API/doc structure changes.
```

> [!IMPORTANT]
> Release notes should describe outcomes for users, not internal implementation trivia.

## Related reference

- [Semantic Versioning and Changelog Guide](../../reference/semantic-versioning-and-changelog.md)
- [Tags and Releases Quick Reference](../../reference/tags-and-releases-quick-reference.md)

---

## Recap

- Tags identify version snapshots.
- Releases communicate what changed and why it matters.
- Strong release notes reduce upgrade confusion.

## Try it yourself

Create a practice tag `v0.1.0`, publish a draft release, and write Added/Changed/Fixed notes.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Wiki and Long-Form Project Knowledge](./04-wiki-and-long-form-project-knowledge.md) | [Chapter 05](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [GitHub Capstone Project →](./06-github-capstone-project.md) |

</div>
