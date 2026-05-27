# Virtual DOM Intro

## What virtual DOM means

Virtual DOM (VDOM) is an in-memory representation of UI structure.
Frameworks compare previous and next virtual trees, then update only changed real DOM nodes.

## Why this concept exists

Direct DOM updates are often expensive and hard to coordinate in large apps.
VDOM introduces a state-first model:
1. compute next UI state
2. create next virtual tree
3. diff against previous tree
4. patch real DOM minimally

## Conceptual example

State change:

```javascript
state.count += 1;
```

VDOM-oriented thinking:
- recompute UI from `state`
- render updates in one coherent pass

## Important clarification

VDOM is a technique, not JavaScript itself.
You can still write excellent frontend JavaScript without a VDOM framework by keeping rendering state-driven and centralized.

## When VDOM helps

- large component trees
- frequent UI updates
- state-heavy applications

## When plain DOM is enough

- small widgets
- simple pages
- low-frequency updates

---

[← JavaScript Reference](./README.md)
