# toggle-state

Cycle the background color of selected elements through a fixed set of highlight states.

## Color cycle

```
transparent → green (#b2f2bb) → yellow (#fff3bf) → red (#ffc9c9) → transparent
```

## How to use

1. Select one or more elements (rectangles, ellipses, etc.).
2. Run **toggle-state** from the Tools Panel.
3. Each call advances every selected element one step around the cycle.

## Typical workflow

Use alongside [parse-datastructure](../parse-datastructure/README.md) or [blank-datastructure](../blank-datastructure/README.md):

- **Green** — visited / in the window / valid
- **Yellow** — currently active / being compared
- **Red** — invalid / out-of-bounds / marked for removal
- **Transparent** — default / unvisited

## Notes

- Text elements, arrows, and lines are skipped — only filled shapes are affected.
- All selected elements advance together, so you can batch-reset a whole row.
