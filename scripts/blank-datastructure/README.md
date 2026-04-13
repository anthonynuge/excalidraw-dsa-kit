# blank-datastructure

Insert a blank data structure scaffold directly on the canvas — no input text required.

## Supported modes

| Mode | Prompt | Output |
|------|--------|--------|
| Array / Grid | `cols` or `cols,rows` | Empty grid with index labels |
| Hash Map | number of rows | Two-column table (Key / Value) |

## How to use

1. Run **blank-datastructure** from the Tools Panel (nothing needs to be selected).
2. Pick **Array / Grid** or **Hash Map** from the suggester.
3. Enter the dimensions when prompted.

The structure is placed at the center of the current viewport and grouped automatically.

## Examples

- Array of 6 elements: enter `6`
- 3×4 grid: enter `3,4`
- HashMap with 5 rows: enter `5`

## Notes

- Array mode adds column index labels (and row index labels for 2-D grids).
- HashMap mode renders a dark header row with **Key** / **Value** labels.
