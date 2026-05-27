<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Versioning and Changelog Management

> Version numbers and changelogs make software progress understandable.

**You will learn:**
- What MAJOR, MINOR, and PATCH mean
- How pre-release labels work
- How to maintain a changelog that beginners can read

**Before this page, you should know:** [Licenses for Beginners](./03-licenses-for-beginners.md)

---

## Semantic Versioning in plain language

Semantic Versioning (SemVer) format is:

```text
MAJOR.MINOR.PATCH
```

- **MAJOR**: breaking changes that can break existing users.
- **MINOR**: backward-compatible feature additions.
- **PATCH**: backward-compatible fixes and small updates.

Official spec: <https://semver.org>

## Your development-phase strategy

Your model maps to SemVer like this:

- `0.1.0`: scaffolding and setup baseline.
- `0.2.0` through `0.8.0`: planned development milestones.
- `0.8.x`: alpha/local-testing runway.
- `0.9.x`: beta and release-candidate territory.

> [!IMPORTANT]
> In `0.x`, you can move quickly, but version meaning should stay consistent for
> your own team and contributors.

## Pre-release labels

Use pre-release identifiers after a hyphen:

```text
x.y.z-alpha.0
x.y.z-beta.0
x.y.z-rc.0
```

Your preferred pattern is valid when normalized as lowercase:

```text
1.0.0-beta.1
```

> [!NOTE]
> Some teams write `1.0.0-Beta.1`. SemVer tooling is most predictable with lowercase labels.

## Changelog micro-section standard

Every release should update `CHANGELOG.md` using a consistent section:

```markdown
## [0.4.2] - 2026-05-27
### Added
- Added repository settings checklist.

### Changed
- Improved beginner explanations for branch workflows.

### Fixed
- Corrected broken chapter navigation links.
```

Keep entries user-facing and outcome-focused. Avoid vague lines like "misc updates".

## Where this belongs in GitHub workflow

- Update changelog before creating a release.
- Tag repository after changelog update.
- Copy changelog highlights into release notes.

---

## Recap

- MAJOR/MINOR/PATCH communicate compatibility and change size.
- Pre-release labels signal maturity (`alpha`, `beta`, `rc`).
- Changelog discipline improves trust and upgrade clarity.

## Try it yourself

Create a `CHANGELOG.md` and write entries for `0.1.0`, `0.2.0`, and `0.2.1`
using Added/Changed/Fixed sections.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Licenses for Beginners](./03-licenses-for-beginners.md) | [Chapter 02](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Repository Settings That Matter →](./05-repository-settings-that-matter.md) |

</div>
