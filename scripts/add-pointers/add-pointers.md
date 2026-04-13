/*
```javascript
*/

const sets = {
  "i, j": [
    { label: "i", color: "#2f9e44" },
    { label: "j", color: "#e03131" },
  ],
  "L, R": [
    { label: "L", color: "#2f9e44" },
    { label: "R", color: "#e03131" },
  ],
  "slow, fast": [
    { label: "slow", color: "#2f9e44" },
    { label: "fast", color: "#e03131" },
  ],
  "cur, prev": [
    { label: "cur", color: "#2f9e44" },
    { label: "prev", color: "#e03131" },
  ],
};

const names = Object.keys(sets);
const pick = await utils.suggester(names, names, "Pointer set");
if (!pick) return;

const pointers = sets[pick];

const GAP = 30;
const ARROW_H = 30;

ea.style.roughness = 0;
ea.style.fontFamily = 3;
ea.style.fontSize = 18;
ea.style.strokeWidth = 2;
ea.style.startArrowHead = null;
ea.style.endArrowHead = "arrow";

let offsetX = 0;

for (const ptr of pointers) {
  ea.style.strokeColor = ptr.color;

  const metrics = ea.measureText(ptr.label);
  const width = Math.max(metrics.width, 20);

  const arrowId = ea.addArrow(
    [[offsetX + width / 2, ARROW_H], [offsetX + width / 2, 0]],
    {}
  );

  const textId = ea.addText(offsetX, ARROW_H + 4, ptr.label);

  ea.addToGroup([arrowId, textId]);

  offsetX += width + GAP;
}

await ea.addElementsToView(true, false);

/*
```
*/
