/*
```javascript
*/

const states = [
  "transparent",
  "#b2f2bb",
  "#fff3bf",
  "#ffc9c9",
];

const selected = ea.getViewSelectedElements();
if (selected.length === 0) {
  new Notice("Select an element to toggle");
  return;
}

ea.copyViewElementsToEAforEditing(selected);

for (const el of ea.getElements()) {
  if (el.type === "text" || el.type === "arrow" || el.type === "line") continue;
  const current = el.backgroundColor || "transparent";
  const idx = states.indexOf(current);
  const next = states[(idx + 1) % states.length];
  el.backgroundColor = next;
  if (next === "transparent") {
    el.fillStyle = "solid";
  } else {
    el.fillStyle = "solid";
  }
}

await ea.addElementsToView(false, true);

/*
```
*/
