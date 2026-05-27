# DOM and Events Patterns

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for selecting, creating, updating, appending, removing, and responding to DOM elements.

Course lesson: [DOM, Events, Rendering, and API Data](../course/04-asynchronous-javascript-and-apis/04-working-with-http-apis.md).

## DOM Query Methods

| API | Parameters | Returns | Use for | Notice |
|---|---|---|---|---|
| `document.querySelector(selector)` | CSS selector string | first `Element` or `null` | one required element | check for `null` |
| `document.querySelectorAll(selector)` | CSS selector string | static `NodeList` | many matching elements | use `for...of` |
| `element.closest(selector)` | CSS selector string | ancestor/self or `null` | event delegation | starts at current element |
| `element.matches(selector)` | CSS selector string | boolean | target checks | useful in handlers |

Required element helper:

```javascript
function getRequiredElement(selector) {
  const element = document.querySelector(selector);

  if (!element) {
    throw new Error(`Missing element: ${selector}`);
  }

  return element;
}
```

## Create and Update Elements

| API/property | Parameters/value | Use for | Risk/tradeoff |
|---|---|---|---|
| `document.createElement(tagName)` | tag name string | create element | not visible until appended |
| `document.createTextNode(text)` | text string | explicit text node | often `textContent` is simpler |
| `element.textContent = value` | string | safe user text | replaces child text |
| `element.innerHTML = html` | HTML string | trusted markup only | unsafe for user data |
| `element.classList.add(name)` | class name | styling state | class must exist in CSS |
| `element.dataset.id = value` | string | store DOM metadata | always string values |
| `element.setAttribute(name, value)` | attr name/value | generic attributes | prefer properties for common fields |

Safe render:

```javascript
function renderTask(task) {
  const item = document.createElement("li");
  const title = document.createElement("span");

  title.textContent = task.title;
  item.append(title);

  return item;
}
```

## Add, Replace, and Remove Nodes

| API | Use |
|---|---|
| `parent.append(child1, child2)` | add nodes or strings at the end |
| `parent.prepend(child)` | add at the beginning |
| `parent.replaceChildren(...children)` | clear and replace children |
| `element.remove()` | remove element from DOM |
| `parent.insertBefore(node, beforeNode)` | insert before a specific node |

Render list:

```javascript
function renderTasks(tasks) {
  const list = getRequiredElement("#task-list");
  const items = tasks.map(renderTask);
  list.replaceChildren(...items);
}
```

## Events

| API/property | Meaning |
|---|---|
| `addEventListener(type, handler)` | run handler when event occurs |
| `event.preventDefault()` | stop default browser behavior |
| `event.target` | original element that triggered event |
| `event.currentTarget` | element the listener is attached to |
| `event.stopPropagation()` | stop bubbling; use sparingly |

Form submit:

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();
  // read inputs, update state, render
});
```

Event delegation:

```javascript
list.addEventListener("click", (event) => {
  const button = event.target.closest("button[data-action]");
  if (!button) return;

  const action = button.dataset.action;
  const id = button.dataset.id;

  // route action
});
```

## State-Driven Rendering Checklist

1. Read user input.
2. Validate it.
3. Create next state.
4. Render DOM from state.
5. Show message/loading/error state.

Avoid code where five unrelated functions all mutate the DOM directly. That
becomes hard to debug.

## Cross References

- [Virtual DOM Intro](./virtual-dom-intro.md)
- [Arrays and Objects Patterns](./arrays-and-objects-patterns.md)
- [Browser Runtime and Web APIs](./browser-runtime-and-web-apis.md)
