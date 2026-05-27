<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Tags and Releases Quick Reference

> Quick checklist for creating clean version tags and release entries.

## Tag creation

```bash
git tag v1.2.0
git push origin v1.2.0
```

## Pre-release tags

```text
v1.2.0-alpha.0
v1.2.0-beta.0
v1.2.0-rc.0
```

## Release publishing checklist

- Tag exists on correct commit.
- `CHANGELOG.md` updated.
- Release title matches version.
- Notes include Added/Changed/Fixed.
- Pre-release checkbox set for alpha/beta/rc.

## Notes skeleton

```markdown
## Added
- ...

## Changed
- ...

## Fixed
- ...
```

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
