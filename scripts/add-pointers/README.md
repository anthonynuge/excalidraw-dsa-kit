# add-pointers

Place labeled pointer arrows on the canvas. Useful for annotating algorithm state on arrays and linked lists.

## Available pointer sets

| Set | Labels | Typical use |
|-----|--------|-------------|
| i, j | green i / red j | Two-index problems |
| L, R | green L / red R | Binary search, two-pointer |
| slow, fast | green slow / red fast | Cycle detection |
| cur, prev | green cur / red prev | Linked list traversal |

## How to use

1. Run **add-pointers** from the Tools Panel (nothing needs to be selected).
2. Pick a pointer set from the suggester.
3. The arrows are placed on the canvas. Drag each arrow so it points at the cell it represents.

## Notes

- Each pointer is a down-pointing arrow with a label beneath it.
- The two pointers in each set are pre-colored green and red to make them visually distinct.
- Arrows are grouped per pointer (arrow + label move together).
