# Contributing

Contributions are welcome — new scripts, bug fixes, and doc improvements all count.

## Adding a new script

1. Fork the repo and create a branch: `git checkout -b script/my-script-name`
2. Add `scripts/MyScript.md` — the script file must follow the Excalidraw Script Engine format:

   ```
   /*
   ```javascript
   */

   // ... your script code ...

   /*
   ```
   */
   ```

3. Add `scripts/MyScript.svg` — a simple SVG icon (100×90 viewBox recommended) for the toolbar.
4. Add `docs/MyScript.md` with a description, usage instructions, and any input/output format notes.
5. Add a row to the table in `README.md`.
6. Open a pull request with a short description of what the script does.

## Script guidelines

- Scripts should work with the current stable Excalidraw plugin release.
- Use `ea.*` and `utils.*` APIs only — no `require()` or Node built-ins.
- Keep scripts focused on a single task.
- Test on at least one vault before submitting.

## Reporting issues

Open a GitHub issue with:
- Excalidraw plugin version
- Obsidian version
- Steps to reproduce
- Expected vs. actual behavior
