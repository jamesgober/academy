# AI Authoring Guide

Rules for any AI (or person) generating content for Academy. These exist to
keep quality high and to prevent the failure modes that make auto-generated
documentation worthless. **Follow these literally.**

---

## The prime directive

Write for a specific reader: someone smart who simply has not learned this yet.
Never write down to them. Never pad. If a sentence does not teach something,
delete it.

---

## Hard rules

1. **No filler.** Banned phrases: "In today's fast-paced world," "It's important
   to note that," "As we all know," "Let's dive in," "powerful and flexible."
   If a paragraph could be deleted without losing information, delete it.

2. **Never defer the hard part.** Do not write "advanced topics are beyond the
   scope of this guide" as a way to skip the difficult thing the reader actually
   came for. If a topic is genuinely out of scope, link to where it IS covered.
   Hollow justifications for skipping hard material are forbidden.

3. **Define every term on first use.** The first time a page says "compiler,"
   "borrow," "heap," etc., define it in plain words in the same sentence or the
   next one.

4. **Explain WHY before HOW.** A reader who knows why a feature exists learns
   the syntax in seconds. Motivation first, mechanics second.

5. **Every code block runs.** No pseudo-code presented as real. If it's a
   fragment, say so. Tag every fence with its language.

6. **One real example beats three abstract ones.** Prefer a tiny, concrete,
   relatable example over `foo`/`bar` abstraction.

7. **Stay in template.** Use TUTORIAL_PAGE.md or REFERENCE_PAGE.md. Fill the
   head-nav and foot-nav. Never leave a `{{TOKEN}}` in a published page.

---

## Tone

Direct and warm. Confident, not chatty. No emoji in body text. No exclamation
marks except where genuine. Address the reader as "you."

---

## Self-check before publishing

- [ ] Could a total beginner follow this without getting stuck on an undefined word?
- [ ] Did I skip or hand-wave any hard part the title promised?
- [ ] Does every code block actually run?
- [ ] Are all nav links filled and correct?
- [ ] Did I delete every `{{TOKEN}}` and every template comment block?
- [ ] Is there a single sentence of filler I can cut? (Cut it.)
