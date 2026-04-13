/*
```javascript
*/

const mode = await utils.suggester(
  ["Array / Grid", "Hash Map"],
  ["array", "hashmap"],
  "Generate..."
);
if (!mode) return;

const BOX_W = 60;
const BOX_H = 60;

if (mode === "array") {
  const input = await utils.inputPrompt("Size (cols or cols,rows)", "e.g. 6 or 3,4");
  if (!input) return;

  const parts = input.trim().split(",").map(s => parseInt(s.trim()));
  const cols = parts[0];
  const rows = parts.length > 1 && !isNaN(parts[1]) ? parts[1] : 1;

  if (isNaN(cols) || cols < 1 || rows < 1) return;

  const INDEX_GAP = 8;
  const rowLabelW = rows > 1 ? 30 : 0;

  // Column index labels
  ea.style.strokeColor = "#868e96";
  ea.style.roughness = 0;
  ea.style.fontFamily = 3;
  ea.style.fontSize = 18;

  for (let c = 0; c < cols; c++) {
    ea.addText(
      rowLabelW + c * BOX_W,
      0,
      c.toString(),
      { width: BOX_W, textAlign: "center" }
    );
  }

  const topY = INDEX_GAP + 22;

  // Row index labels
  if (rows > 1) {
    for (let r = 0; r < rows; r++) {
      const metrics = ea.measureText(r.toString());
      ea.addText(
        (rowLabelW - metrics.width) / 2,
        topY + r * BOX_H + (BOX_H - metrics.height) / 2,
        r.toString()
      );
    }
  }

  // Outer rectangle
  ea.style.strokeColor = "#1e1e1e";
  ea.style.backgroundColor = "transparent";
  ea.style.fillStyle = "solid";
  ea.style.roughness = 0;
  ea.style.roundness = null;

  ea.addRect(rowLabelW, topY, BOX_W * cols, BOX_H * rows);

  // Vertical dividers
  for (let c = 1; c < cols; c++) {
    ea.addLine([[rowLabelW + c * BOX_W, topY], [rowLabelW + c * BOX_W, topY + BOX_H * rows]]);
  }

  // Horizontal dividers
  for (let r = 1; r < rows; r++) {
    ea.addLine([[rowLabelW, topY + r * BOX_H], [rowLabelW + BOX_W * cols, topY + r * BOX_H]]);
  }

  // Empty cell text
  ea.style.fontSize = 28;
  ea.style.fontFamily = 3;

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      ea.addText(
        rowLabelW + c * BOX_W,
        topY + r * BOX_H,
        "",
        {
          width: BOX_W,
          height: BOX_H,
          textAlign: "center",
          textVerticalAlign: "middle",
        }
      );
    }
  }

} else if (mode === "hashmap") {
  const input = await utils.inputPrompt("Number of rows", "e.g. 6");
  if (!input) return;

  const rows = parseInt(input.trim());
  if (isNaN(rows) || rows < 1) return;

  const COL_W = 100;
  const HEADER_H = 40;

  ea.style.roughness = 0;
  ea.style.fontFamily = 3;
  ea.style.roundness = null;
  ea.style.fillStyle = "solid";

  // Header background
  ea.style.strokeColor = "#1e1e1e";
  ea.style.backgroundColor = "#343a40";

  ea.addRect(0, 0, COL_W * 2, HEADER_H);

  // Header labels
  ea.style.strokeColor = "#ffffff";
  ea.style.fontSize = 20;

  const keyMetrics = ea.measureText("Key");
  ea.addText(
    (COL_W - keyMetrics.width) / 2,
    (HEADER_H - keyMetrics.height) / 2,
    "Key"
  );

  const valMetrics = ea.measureText("Value");
  ea.addText(
    COL_W + (COL_W - valMetrics.width) / 2,
    (HEADER_H - valMetrics.height) / 2,
    "Value"
  );

  // Header divider
  ea.style.strokeColor = "#1e1e1e";
  ea.addLine([[COL_W, 0], [COL_W, HEADER_H]]);

  // Body
  ea.style.backgroundColor = "transparent";

  ea.addRect(0, HEADER_H, COL_W * 2, BOX_H * rows);

  // Column divider
  ea.addLine([[COL_W, HEADER_H], [COL_W, HEADER_H + BOX_H * rows]]);

  // Row dividers
  for (let r = 1; r < rows; r++) {
    ea.addLine([[0, HEADER_H + r * BOX_H], [COL_W * 2, HEADER_H + r * BOX_H]]);
  }

  // Empty cells
  ea.style.fontSize = 28;

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < 2; c++) {
      ea.addText(
        c * COL_W,
        HEADER_H + r * BOX_H,
        "",
        {
          width: COL_W,
          height: BOX_H,
          textAlign: "center",
          textVerticalAlign: "middle",
        }
      );
    }
  }
}

// Group everything
const ids = ea.getElements().map(e => e.id);
ea.addToGroup(ids);

await ea.addElementsToView(true, false);

/*
```
*/
