/*
```javascript
*/

const selected = ea.getViewSelectedElements();
if (selected.length === 0) {
  new Notice("Select elements to reset");
  return;
}

ea.copyViewElementsToEAforEditing(selected);

for (const el of ea.getElements()) {
  if (el.type === "text" || el.type === "arrow" || el.type === "line") continue;
  el.backgroundColor = "transparent";
}

await ea.addElementsToView(false, true);

/*
```
*/
