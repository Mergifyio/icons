---
name: add-icon
description: Add a new icon to the @mergifyio/icons package. Use when the user asks to "add an icon", "ajouter une icône", "new icon to the package", or pastes an SVG / Figma node id with intent to add it. Walks through SVG normalization (currentColor), the package.json exports map, and README documentation. The actual npm publish happens later through a GitHub Release.
---

# Adding an icon to @mergifyio/icons

This package ships raw SVG files used across `dashboard`, `docs`, and
`mergify.com`. Each icon lives under `icons/`, is exposed via
`package.json#exports`, and is documented in the README.

## Inputs you need

Before starting, gather:

- **Icon name** — kebab-case, no extension (e.g. `runner`, `auto-merge`,
  `scheduled-freeze`). It will become the filename and the import path.
- **SVG source** — either a `.svg` file content the user pasted, or a node
  id from a design tool to export from. If using Figma, ask the user for
  the file key.
- **Suggested brand color** (optional) — the hex the design recommends for
  this icon, for the README table. Skip if not provided; the table column
  can hold a dash.

If anything is missing, ask the user before proceeding.

## Step-by-step checklist

Use TodoWrite to track these.

### 1. Get the SVG content

If the user pasted SVG content directly, use it as-is.

If they gave a Figma node id, export it via the Figma MCP tool:

```js
// use_figma JS body
const node = await figma.getNodeByIdAsync('<NODE_ID>');
const bytes = await node.exportAsync({ format: 'SVG', svgIdAttribute: false });
return figma.base64Encode(bytes);
```

Decode the base64 and you have the raw SVG.

### 2. Normalize colors to `currentColor`

The package convention is **all icons in `currentColor`** so consumers pick
the color via CSS. Replace hardcoded fills/strokes:

- `fill="#XXXXXX"` → `fill="currentColor"`
- `stroke="#XXXXXX"` → `stroke="currentColor"`

**Exceptions** — keep hardcoded colors when:
- The icon is a multi-color brand logo (e.g. Slack with its 4 brand colors).
  In that case, leave colors as-is and document this in the README.
- A specific path is meant to stay one color (rare). Ask the user if
  unsure.

Quick sed for the common case:

```sh
sed -i.bak -E 's/(stroke|fill)="#[0-9A-Fa-f]{3,8}"/\1="currentColor"/g' \
  icons/<name>.svg && rm icons/<name>.svg.bak
```

Verify there are no remaining hex colors:

```sh
grep -E '(stroke|fill)="#' icons/<name>.svg && echo "LEAK" || echo "clean"
```

### 3. Save the SVG

Write the normalized content to `icons/<name>.svg`.

Confirm the `viewBox` — most icons use `0 0 40 40`, but the `stacks` icon
uses `0 0 32 32`. Note it for the README table.

### 4. Add the export entry

Add a line under `exports` in `package.json`, keeping the alphabetical order:

```json
"./<name>.svg": "./icons/<name>.svg",
```

This lets consumers import as `@mergifyio/icons/<name>.svg` (without the
`icons/` segment).

### 5. Update the README table

Add a row to the table under `## Icons`, keeping it alphabetical-ish. Format:

```md
| `<name>.svg` | `#HEX` (color name) | `0 0 40 40` |
```

If no brand color was provided, use `—` or omit the row's color cell value.

### 6. Verify locally

```sh
# All exports resolve to existing files
node -e '
const fs = require("fs");
const pkg = JSON.parse(fs.readFileSync("package.json", "utf8"));
for (const [k, v] of Object.entries(pkg.exports)) {
  if (k === "./package.json") continue;
  if (!fs.existsSync(v.replace(/^\.\//, ""))) console.error("MISSING", k, v);
}
'
```

Expected: no output. If anything prints, fix the path.

### 7. Commit and open a PR

```sh
git checkout -b add-icon-<name>
git add icons/<name>.svg package.json README.md
git commit -m "feat: add <name> icon"
git push -u origin add-icon-<name>
gh pr create --title "feat: add <name> icon" --body "..."
```

After review and merge, ask the maintainer to **create a GitHub Release**
with the next semver tag (e.g. `0.2.0`). The release event triggers
`.github/workflows/release.yml`, which publishes the new version to npm via
OIDC Trusted Publishing — no token needed.

## Common pitfalls

- **Wrong filename casing.** Filenames must be kebab-case lowercase
  (`merge-queue.svg`). Linux CI is case-sensitive.
- **Forgetting the exports entry.** The SVG file alone isn't published —
  it needs to be listed in `package.json#exports` to be importable.
- **Hardcoded brand colors leaking through.** Always check for residual
  `#XXX` after normalization.
- **`width`/`height` baked in.** SVGs from Figma include `width="40"
  height="40"`. Leave them — consumers can override via attributes
  (`<svg width={...}>`) or the SVG renders at its native size by default.
- **Forgetting the GitHub Release.** Merging the PR adds the icon to
  `main` but doesn't publish anything. The maintainer needs to cut a
  GitHub Release with a semver tag for the new icon to land on npm.
