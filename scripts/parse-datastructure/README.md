# parse-datastructure

Parse a selected text element and render it as a visual data structure on the canvas.

## Supported modes

| Mode | Input format | Output |
|------|-------------|--------|
| Array | `1, 2, 3, 4` | Horizontal grid of boxes |
| Linked List | `1, 2, 3, 4` | Boxes connected by arrows |
| Tree | `1, 2, 3, null, 4, null, 5` | Binary tree (BFS / LeetCode format) |
| Matrix | `[[1,2,3],[4,5,6]]` | 2-D grid |

## How to use

1. Add a **Text** element to your drawing and type the input (e.g. `3, 1, 4, 1, 5`).
2. Select the text element.
3. Run **parse-datastructure** from the Tools Panel.
4. Choose the structure type from the suggester.

The rendered structure is placed directly below the text element and grouped automatically.

## Notes

- For **Tree** mode, `null` values skip a node (LeetCode-style BFS serialization).
- For **Matrix** mode, the text element must contain a valid 2-D JSON array, e.g. `[[1,2],[3,4]]`.
- The original text element is not modified or moved.
