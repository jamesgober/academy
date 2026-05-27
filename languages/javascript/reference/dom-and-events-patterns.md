# DOM and Events Patterns

## Querying DOM nodes

```javascript
const app = document.querySelector("#app");
const buttons = document.querySelectorAll("button[data-action]");
```

## Safe rendering pattern

```javascript
function renderItems(items) {
  const list = document.querySelector("#list");
  list.innerHTML = "";

  for (const item of items) {
    const li = document.createElement("li");
    li.textContent = item.label;
    list.appendChild(li);
  }
}
```

Prefer `textContent` for user data to avoid accidental HTML injection.

## Event listeners

```javascript
document.querySelector("#save").addEventListener("click", onSave);
```

Keep handlers small and delegate work to service/render functions.

## Event delegation

```javascript
document.querySelector("#list").addEventListener("click", (event) => {
  const button = event.target.closest("button[data-id]");
  if (!button) return;
  const id = button.dataset.id;
  // handle action
});
```

Useful when list rows are dynamic.

## DOM update checklist

1. Derive state first.
2. Render from state.
3. Avoid scattered direct DOM mutations across many functions.

---

[← JavaScript Reference](./README.md)
