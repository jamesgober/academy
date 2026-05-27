<!--
================================================================================
ACADEMY TUTORIAL PAGE TEMPLATE
================================================================================
HOW TO USE:
  1. Copy this file to its destination (e.g. languages/rust/course/01-.../02-variables.md)
  2. Replace every {{TOKEN}} below. Do not leave any {{...}} in a published page.
  3. Fill the body. Follow AI_AUTHORING_GUIDE.md — no filler, no deferred topics.
  4. Delete this comment block before publishing.

NAV TOKENS (use relative paths from THIS file's location):
  {{HOME}}        -> path back to repo root README           (e.g. ../../../../README.md)
  {{LANG}}        -> Language Name
  {{LOGO}}        -> Language Logo Path (example: ../../../../_assets/logos/rust.svg)
  {{LOGO_ALT}}    -> Language Logo Alt (example: Rust Logo)
  {{TRACK_ROOT}}  -> path to the language/track README        (e.g. ../../README.md)
  {{CHAPTER_ROOT}}-> path to the chapter/section README        (e.g. ../README.md)
  {{PREV}}        -> path to previous page (use {{CHAPTER_ROOT}} on first page)
  {{NEXT}}        -> path to next page, or "#" if none
  {{PREV_TITLE}}  -> title of previous page (or "Chapter Start" on first page)
  {{NEXT_TITLE}}  -> title of next page (or "—")
================================================================================
-->
<h1 align="center">
    <img width="99" alt="{{LOGO_ALT}}" src="{{LOGO}}">
    <br>
    <b>{{LANG}}</b>
</h1>

[Home]({{HOME}}) / [{{LANG}}]({{TRACK_ROOT}}) / [Chapter]({{CHAPTER_ROOT}})

---

# {{PAGE_TITLE}}

> {{ONE_LINE_SUMMARY}}

**You will learn:**
- {{OBJECTIVE_1}}
- {{OBJECTIVE_2}}
- {{OBJECTIVE_3}}

**Before this page, you should know:** {{PREREQUISITES}}

---

## {{SECTION_HEADING}}

{{BODY}}

<!--
  Body guidance:
  - Explain the WHY before the HOW.
  - Every new term gets defined the first time it appears, in plain language.
  - Show runnable code in fenced blocks with a language tag.
  - After code, explain what it does line-relevant, not line-by-line robotically.
  - Use a real, small example over an abstract one.
  - Use GitHub callouts where they improve clarity:
    > [!NOTE] for context readers should not miss.
    > [!TIP] for shortcuts and practical habits.
    > [!IMPORTANT] for prerequisites and must-do steps.
    > [!WARNING] for actions that can break setup or lose work.
-->

---

## Recap

{{RECAP_BULLETS}}

## Try it yourself

{{EXERCISE}}

---

[**Next ->** {{NEXT_TITLE}}]({{NEXT}})  
[**<- Previous** {{PREV_TITLE}}]({{PREV}})
