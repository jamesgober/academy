# Virtual DOM Intro

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Virtual DOM means representing UI as JavaScript data before updating the real DOM.

Course lesson: [DOM, Events, Rendering, and API Data](../course/04-asynchronous-javascript-and-apis/04-working-with-http-apis.md).

## Mental Model

```text
state
  |
  v
virtual tree objects
  |
  v
diff or replace
  |
  v
real DOM updates
  |
  v
browser paints UI
```

The real DOM is the browser's live page tree. A virtual DOM tree is a JavaScript
object description of what the UI should look like.

## Tiny Virtual Node

```javascript
const node = {
  tag: "button",
  props: {
    type: "button",
    className: "primary"
  },
  children: ["Save"]
};
```

This object is not visible. It is only a description.

## Turn a Virtual Node into a Real Element

```javascript
function createElementFromVirtualNode(node) {
  if (typeof node === "string") {
    return document.createTextNode(node);
  }

  const element = document.createElement(node.tag);

  for (const [name, value] of Object.entries(node.props ?? {})) {
    element[name] = value;
  }

  for (const child of node.children ?? []) {
    element.append(createElementFromVirtualNode(child));
  }

  return element;
}
```

Use it:

```javascript
document.body.append(createElementFromVirtualNode(node));
```

## Why Frameworks Use This Idea

| Problem | VDOM-style answer |
|---|---|
| many UI states | describe UI from state |
| scattered DOM updates | centralize render output |
| frequent changes | compare old and new descriptions |
| complex component trees | compose small UI descriptions |

## Important Clarification

Virtual DOM is not built into JavaScript. It is a technique used by some
libraries and frameworks. You can write excellent JavaScript with the real DOM
directly when your rendering is organized.

## Plain DOM Versus VDOM

| Use plain DOM when | VDOM/framework may help when |
|---|---|
| page is small | UI has many components |
| interactions are simple | state changes frequently |
| team wants no build step | team needs reusable component model |
| performance is already fine | manual DOM coordination is becoming brittle |

## Risk Notes

- VDOM does not automatically make code fast.
- Re-rendering huge subtrees can still be expensive.
- Direct DOM manipulation inside a framework can fight the framework.
- Learn real DOM first so framework behavior makes sense.

## Cross References

- [DOM and Events Patterns](./dom-and-events-patterns.md)
- [Arrays and Objects Patterns](./arrays-and-objects-patterns.md)
- [Objects, Classes, and Prototypes Deep Dive](./objects-classes-and-prototypes-deep-dive.md)
