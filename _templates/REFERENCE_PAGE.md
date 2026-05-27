<!--
================================================================================
ACADEMY REFERENCE PAGE TEMPLATE
================================================================================
For quick-lookup pages (syntax tables, cheat sheets, API-style entries).
Reference pages are SCANNABLE, not narrative. Tables and short entries only.
Replace every {{TOKEN}}. Delete this comment block before publishing.

NAV TOKENS (use relative paths from THIS file's location):
	{{HOME}}         -> path back to repo root README           (e.g. ../../../README.md)
	{{LANG}}         -> Language Name
	{{LOGO}}         -> Language Logo Path (example: ../../../_assets/logos/rust.svg)
	{{LOGO_ALT}}     -> Language Logo Alt (example: Rust Logo)
	{{TRACK_ROOT}}   -> path to the language/track README       (e.g. ../README.md)
	{{REFERENCE_ROOT}} -> path to reference index               (e.g. ./README.md)
	{{COURSE_LINK}}  -> path to course root                     (e.g. ../course/)
================================================================================
-->

<h1 align="center">
		<img width="99" alt="{{LOGO_ALT}}" src="{{LOGO}}">
		<br>
		<b>{{LANG}}</b>
</h1>

[Home]({{HOME}}) / [{{LANG}}]({{TRACK_ROOT}}) / [Reference]({{REFERENCE_ROOT}})

---

# {{TOPIC}} Reference

> Quick lookup. For explanation, see the [course]({{COURSE_LINK}}).

## At a glance

| {{COL_A}} | {{COL_B}} | {{COL_C}} |
|-----------|-----------|-----------|
| {{ROW}}   | {{ROW}}   | {{ROW}}   |

---

## {{ENTRY_NAME}}

```{{LANG}}
{{SIGNATURE_OR_SYNTAX}}
```

{{ONE_OR_TWO_LINE_DESCRIPTION}}

> [!TIP]
> Keep entries short and exact. Reference pages are for lookup, not first-time teaching.

**Example:**

```{{LANG}}
{{SHORT_EXAMPLE}}
```

---

[Reference Index]({{REFERENCE_ROOT}}) / [{{LANG}}]({{TRACK_ROOT}}) / [Home]({{HOME}})
