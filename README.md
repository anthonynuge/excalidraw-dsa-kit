# excalidraw-dsa-kit

A collection of [Obsidian Excalidraw](https://github.com/zsviczian/obsidian--excalidraw-plugin) scripts for visualizing data structures and algorithm patterns. Built for LeetCode problem-solving and DSA study sessions

I kept hitting the same wall every time I sat down to practice LeetCode. I'd open a problem, read the input — something like `[3, 1, 4, 1, 5, 9]` or a tree serialized as `[1, 2, 3, null, 4]` — and before I could even think about the algorithm I was spending five minutes drawing boxes, connecting arrows, labeling nodes by hand. By the time the diagram looked right, I'd lost the thread of the problem.

I was already using Obsidian Excalidraw as my thinking canvas, so I started writing scripts to automate the parts that were slowing me down. Paste the input, select it, click once — the structure appears. Drop some pointers, step through an iteration, mark a node visited, reset and go again. What used to take several minutes of setup is now a few seconds.

This repo is that collection of scripts. If you practice DSA visually and find yourself drawing the same arrays, trees, and linked lists over and over, these might save you the same frustration they saved me.

## Demo

<!-- Add demo gif/video here -->

[Demo coming soon]()

## Scripts

| Script                                              | Description                                                                           |
| --------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [parse-datastructure](scripts/parse-datastructure/) | Parse a selected text element and render it as an Array, Tree, Linked List, or Matrix |
| [blank-datastructure](scripts/blank-datastructure/) | Insert a blank Array/Grid or HashMap scaffold — no input required                     |
| [add-pointers](scripts/add-pointers/)               | Drop labeled pointer arrows (i/j, L/R, slow/fast, cur/prev)                           |
| [toggle-state](scripts/toggle-state/)               | Cycle selected elements through highlight colors                                      |
| [reset-state](scripts/reset-state/)                 | Clear background color on selected elements                                           |

## Requirements

- [Obsidian](https://obsidian.md/) (any recent version)
- [Excalidraw plugin](https://github.com/zsviczian/obsidian-excalidraw-plugin) v2.0+

## Installation

### Step 1 — Get the files

**Clone** (if you have Git):

```bash
git clone https://github.com/anthonybturner/excalidraw-dsa-kit.git
```

**Or download** the ZIP: click **Code → Download ZIP** on the GitHub repo page, then unzip it anywhere on your machine.

### Step 2 — Configure the Script Engine folder

1. In Obsidian, open **Settings → Excalidraw → Script Engine**.
2. Set the **Script folder** to a path inside your vault (e.g. `Excalidraw/Scripts`). Obsidian will create the folder if it doesn't exist.

### Step 3 — Copy the scripts

Copy the five script folders from this repo's `scripts/` directory into the folder you configured above. Each script lives in its own subfolder and needs **both** the `.md` and `.svg` file:

```
Excalidraw/Scripts/
├── parse-datastructure/
│   ├── parse-datastructure.md
│   └── parse-datastructure.svg
├── blank-datastructure/
│   ├── blank-datastructure.md
│   └── blank-datastructure.svg
├── add-pointers/
│   ├── add-pointers.md
│   └── add-pointers.svg
├── toggle-state/
│   ├── toggle-state.md
│   └── toggle-state.svg
└── reset-state/
    ├── reset-state.md
    └── reset-state.svg
```

> The `.md` file contains the script code; the `.svg` file provides the icon shown in the Tools Panel. The SVG **must share the same base filename** as the `.md` — Excalidraw's Script Engine pairs them by name.

### Step 4 — Reload and open the Tools Panel

1. Restart Obsidian, or run **Reload app without saving** from the command palette.
2. Open any Excalidraw drawing.
3. If the **Obsidian Tools Panel** sidebar is not visible, click the **Obsidian menu** icon (the Obsidian logo) in the top-left of the drawing, or run **Open Obsidian tools panel** from the command palette.
4. Your scripts appear as icon buttons in the panel — click one to run it.

### Alternative — Manual paste (no Script Engine needed)

1. Open any `.md` file in this repo's `scripts/` folder.
2. Copy the JavaScript between the ` ```javascript ` fences.
3. In an Excalidraw drawing, open the command palette and run **Execute ExcalidrawAutomate Script**, then paste the code.

## Usage overview

### parse-datastructure

Select a text element whose content is a value list or 2-D array, then run the script. A suggester will ask which structure to draw:

- **Array** — `1, 2, 3, 4` → horizontal box-array
- **Linked List** — `1, 2, 3, 4` → boxes connected by arrows
- **Tree** — `1, 2, 3, null, 4` → BFS/LeetCode-format binary tree
- **Matrix** — `[[1,2],[3,4]]` → grid

> **How I use it:** When working through a LeetCode problem I copy the example input directly from the problem page, paste it as a text element on the canvas, select it, and run the script. The data structure appears instantly — no manual drawing. This is especially useful for trees and linked lists where drawing by hand eats time before you've even started thinking about the algorithm.

### blank-datastructure

Run the script with nothing selected. Pick **Array / Grid** or **Hash Map**, enter dimensions, and an empty structure is placed on the canvas ready to fill in.

> **How I use it:** When a problem involves an array or grid that I want to fill in myself, I use this to drop a clean indexed scaffold on the canvas in seconds. The index labels are already there so I can focus on tracing values, not drawing boxes.

### add-pointers

Run the script with nothing selected. Pick a pointer set and labeled arrows are placed on the canvas. Drag them onto the array/list cells you want to annotate.

> **How I use it:** After generating a structure I drop a pointer set and physically drag the arrows across cells to simulate each iteration of the algorithm. Stepping through two-pointer or sliding-window problems this way makes the movement concrete and easy to reason about.

### toggle-state

Select one or more elements and run. Each call cycles the background through:
`transparent → green → yellow → red → transparent`

Use this to mark visited / active / invalid cells while tracing an algorithm.

> **How I use it:** When tracing tree and backtracking problems I use colors to track node state — green for visited, yellow for the current node, red for pruned or invalid branches. Clicking through a DFS path visually shows exactly which nodes are on the call stack and which have been eliminated.

### reset-state

Select elements and run. Resets all backgrounds to transparent in one step.

> **How I use it:** Once I finish tracing one path or example I select all (Ctrl+A), run reset-state, and the canvas is clean for the next trace — no need to delete and redraw anything.

## Contributing

Pull requests and new scripts are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT — see [LICENSE](LICENSE).
