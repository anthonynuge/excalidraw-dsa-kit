/*
```javascript
*/

const selected = ea.getViewSelectedElements().filter(e => e.type === "text");
if (selected.length === 0) {
  new Notice("Select a text element containing an array");
  return;
}

const source = selected[0];
const raw = source.rawText.replace(/[\[\]]/g, "").trim();
if (!raw) return;

const mode = await utils.suggester(
  ["Array", "Tree", "Linked List", "Matrix"],
  ["array", "tree", "linkedlist", "matrix"],
  "Visualize as..."
);
if (!mode) return;

const startX = source.x;
const startY = source.y + source.height + 40;

if (mode === "array") {
  const values = raw.split(",").map(s => s.trim()).filter(s => s.length > 0);

  const BOX_W = 60;
  const BOX_H = 60;

  ea.style.strokeColor = "#1e1e1e";
  ea.style.backgroundColor = "transparent";
  ea.style.fillStyle = "solid";
  ea.style.roughness = 0;
  ea.style.fontFamily = 3;
  ea.style.roundness = null;

  ea.addRect(startX, startY, BOX_W * values.length, BOX_H);

  for (let i = 1; i < values.length; i++) {
    ea.addLine([[startX + i * BOX_W, startY], [startX + i * BOX_W, startY + BOX_H]]);
  }

  ea.style.fontSize = 28;

  for (let i = 0; i < values.length; i++) {
    const metrics = ea.measureText(values[i]);
    const cellX = startX + i * BOX_W;
    const textX = cellX + (BOX_W - metrics.width) / 2;
    const textY = startY + (BOX_H - metrics.height) / 2;
    ea.addText(textX, textY, values[i]);
  }

} else if (mode === "tree") {
  const tokens = raw.split(",").map(s => {
    const v = s.trim();
    return v === "null" ? null : v;
  });

  const NODE_R = 25;
  const NODE_D = NODE_R * 2;
  const V_GAP = 120;
  const BASE_H_GAP = 100;

  // Parse compact BFS (LeetCode format) into tree nodes
  // Each node stores: value, level, parent index, left/right child flag
  const nodes = [];
  const parentMap = [];

  if (tokens.length === 0 || tokens[0] === null) return;

  nodes.push({ value: tokens[0], level: 0 });
  parentMap.push(-1);

  let ti = 1;
  let qi = 0;

  while (ti < tokens.length && qi < nodes.length) {
    const parentLevel = nodes[qi].level;

    // Left child
    if (ti < tokens.length) {
      if (tokens[ti] !== null) {
        parentMap.push(qi);
        nodes.push({ value: tokens[ti], level: parentLevel + 1, side: "left" });
      }
      ti++;
    }

    // Right child
    if (ti < tokens.length) {
      if (tokens[ti] !== null) {
        parentMap.push(qi);
        nodes.push({ value: tokens[ti], level: parentLevel + 1, side: "right" });
      }
      ti++;
    }

    qi++;
  }

  const maxLevel = Math.max(...nodes.map(n => n.level));
  const depth = maxLevel + 1;
  const totalWidth = Math.pow(2, depth - 1) * (NODE_D + BASE_H_GAP);

  // Assign x positions using binary subdivision
  // Root gets the full width, each child gets half the parent's slot
  const positions = [];
  // Track slot boundaries: [leftBound, rightBound]
  const slots = [];

  for (let i = 0; i < nodes.length; i++) {
    if (i === 0) {
      slots.push([0, totalWidth]);
      const cx = totalWidth / 2;
      positions.push({ x: startX + cx - NODE_R, y: startY });
    } else {
      const pi = parentMap[i];
      const [pLeft, pRight] = slots[pi];
      const pMid = (pLeft + pRight) / 2;

      let slotLeft, slotRight;
      if (nodes[i].side === "left") {
        slotLeft = pLeft;
        slotRight = pMid;
      } else {
        slotLeft = pMid;
        slotRight = pRight;
      }

      slots.push([slotLeft, slotRight]);
      const cx = (slotLeft + slotRight) / 2;
      positions.push({
        x: startX + cx - NODE_R,
        y: startY + nodes[i].level * (NODE_D + V_GAP),
      });
    }
  }

  // Draw nodes as circles with bound text
  ea.style.strokeColor = "#1e1e1e";
  ea.style.backgroundColor = "transparent";
  ea.style.fillStyle = "solid";
  ea.style.roughness = 0;
  ea.style.roundness = null;
  ea.style.fontFamily = 3;
  ea.style.fontSize = 22;

  const ellipseIds = [];
  for (let i = 0; i < nodes.length; i++) {
    const { x, y } = positions[i];
    const before = ea.getElements().length;
    ea.addText(x, y, nodes[i].value, {
      width: NODE_D,
      height: NODE_D,
      textAlign: "center",
      textVerticalAlign: "middle",
      box: "ellipse",
      boxPadding: 0,
    });
    const added = ea.getElements().slice(before);
    const ellipse = added.find(e => e.type === "ellipse");
    ellipseIds.push(ellipse ? ellipse.id : null);
  }

  // Commit nodes
  await ea.addElementsToView(false, false);

  // Connect parent to child nodes with elbowed flowchart arrows
  ea.style.strokeWidth = 2;
  ea.style.roughness = 0;
  ea.style.roundness = null;

  for (let i = 1; i < nodes.length; i++) {
    const pi = parentMap[i];
    if (ellipseIds[i] === null || ellipseIds[pi] === null) continue;

    const arrowId = ea.connectObjects(ellipseIds[pi], "bottom", ellipseIds[i], "top", {
      endArrowHead: "arrow",
    });

    const arrow = ea.getElement(arrowId);
    if (arrow) {
      arrow.elbowed = true;
      arrow.fixedSegments = null;
      arrow.startIsSpecial = null;
      arrow.endIsSpecial = null;
      if (arrow.startBinding) {
        arrow.startBinding.mode = "orbit";
        arrow.startBinding.fixedPoint = [0.5, 1.094];
      }
      if (arrow.endBinding) {
        arrow.endBinding.mode = "orbit";
        arrow.endBinding.fixedPoint = [0.5, -0.094];
      }
    }
  }

  await ea.addElementsToView(false, false);
  return;

} else if (mode === "linkedlist") {
  const values = raw.split(",").map(s => s.trim()).filter(s => s.length > 0);

  const BOX_W = 60;
  const BOX_H = 60;
  const ARROW_GAP = 80;

  ea.style.strokeColor = "#1e1e1e";
  ea.style.backgroundColor = "transparent";
  ea.style.fillStyle = "solid";
  ea.style.roughness = 0;
  ea.style.fontFamily = 3;
  ea.style.fontSize = 28;
  ea.style.roundness = null;

  const boxIds = [];
  for (let i = 0; i < values.length; i++) {
    const x = startX + i * (BOX_W + ARROW_GAP);
    const before = ea.getElements().length;
    ea.addText(x, startY, values[i], {
      width: BOX_W,
      height: BOX_H,
      textAlign: "center",
      textVerticalAlign: "middle",
      box: "rectangle",
      boxPadding: 0,
    });
    const added = ea.getElements().slice(before);
    const rect = added.find(e => e.type === "rectangle");
    boxIds.push(rect ? rect.id : null);
  }

  await ea.addElementsToView(false, false);

  ea.style.strokeWidth = 2;

  for (let i = 0; i < values.length - 1; i++) {
    if (boxIds[i] === null || boxIds[i + 1] === null) continue;
    ea.connectObjects(boxIds[i], "right", boxIds[i + 1], "left", {
      endArrowHead: "arrow",
      startArrowHead: "none",
    });
  }

  await ea.addElementsToView(false, false);
  return;

} else if (mode === "matrix") {
  // Parse 2D array: [[1,2,3],[4,5,6],[7,8,9]]
  let grid;
  try {
    const jsonStr = "[" + source.rawText.replace(/[\[\]]/g, m => m) + "]";
    grid = JSON.parse(source.rawText);
  } catch (e) {
    // Fallback: try to parse manually
    const inner = source.rawText.trim().replace(/^\[/, "").replace(/\]$/, "");
    grid = [];
    let depth = 0;
    let current = "";
    for (const ch of inner) {
      if (ch === "[") {
        if (depth === 0) current = "";
        depth++;
      } else if (ch === "]") {
        depth--;
        if (depth === 0) {
          grid.push(current.split(",").map(s => s.trim()));
          current = "";
        }
      } else if (depth === 1) {
        current += ch;
      }
    }
  }

  if (!grid || grid.length === 0) {
    new Notice("Could not parse matrix");
    return;
  }

  const rows = grid.length;
  const cols = Math.max(...grid.map(r => r.length));
  const CELL_W = 60;
  const CELL_H = 60;

  ea.style.strokeColor = "#1e1e1e";
  ea.style.backgroundColor = "transparent";
  ea.style.fillStyle = "solid";
  ea.style.roughness = 0;
  ea.style.fontFamily = 3;
  ea.style.roundness = null;

  // Outer rectangle
  ea.addRect(startX, startY, CELL_W * cols, CELL_H * rows);

  // Vertical dividers
  for (let c = 1; c < cols; c++) {
    ea.addLine([[startX + c * CELL_W, startY], [startX + c * CELL_W, startY + CELL_H * rows]]);
  }

  // Horizontal dividers
  for (let r = 1; r < rows; r++) {
    ea.addLine([[startX, startY + r * CELL_H], [startX + CELL_W * cols, startY + r * CELL_H]]);
  }

  // Cell values
  ea.style.fontSize = 28;

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      const val = grid[r] && grid[r][c] != null ? String(grid[r][c]) : "";
      if (!val) continue;
      const metrics = ea.measureText(val);
      const cellX = startX + c * CELL_W;
      const cellY = startY + r * CELL_H;
      ea.addText(
        cellX + (CELL_W - metrics.width) / 2,
        cellY + (CELL_H - metrics.height) / 2,
        val
      );
    }
  }
}

const ids = ea.getElements().map(e => e.id);
ea.addToGroup(ids);

await ea.addElementsToView(false, false);

/*
```
*/
