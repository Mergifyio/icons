# @mergify/icons

Mergify's product icons as raw SVG files — designed by Frank, sourced from
the [Mergify Design System Brandbook](https://www.figma.com/design/JUN85JkKXEkHwuQ063fA5b/Mergify-Design-System?node-id=11733-6614).

These represent each Mergify product across `dashboard`, `docs`, and
`mergify.com`. Each SVG ships in `currentColor` so the consumer controls the
color through CSS — see the suggested brand color per product below.

## Icons

| File | Suggested brand color | viewBox |
|---|---|---|
| `merge-queue.svg` | `#2AA77E` (teal) | `0 0 40 40` |
| `merge-protections.svg` | `#2086C5` (blue) | `0 0 40 40` |
| `ci-insights.svg` | `#5C68F0` (indigo) | `0 0 40 40` |
| `test-insights.svg` | `#9C43E5` (purple) | `0 0 40 40` |
| `stacks.svg` | `#E61E71` (rose) | `0 0 32 32` |

## Install

```sh
pnpm add @mergify/icons
```

## Usage

The package ships raw SVG files. Each consumer imports them through its
bundler's SVG loader.

### Vite (dashboard)

```ts
// As a React component (with vite-plugin-svgr or similar):
import MergeQueueIcon from '@mergify/icons/merge-queue.svg?react';

<MergeQueueIcon width={40} height={40} />

// As a URL:
import url from '@mergify/icons/merge-queue.svg';

<img src={url} alt="Merge Queue" />

// As inline string (?raw):
import svg from '@mergify/icons/merge-queue.svg?raw';
```

### Astro (docs / mergify.com)

```astro
---
import MergeQueue from '@mergify/icons/merge-queue.svg';
---
<MergeQueue width={40} height={40} />
```

(Astro 5+ has [native SVG component support](https://docs.astro.build/en/guides/images/#svgs).)

### Plain HTML / Markdown

```html
<img src="/node_modules/@mergify/icons/merge-queue.svg" alt="Merge Queue" />
```

## Adding new icons

1. Drop the new SVG at the repo root with a kebab-case filename
   (e.g. `runner.svg`).
2. Add an entry to the `exports` field in `package.json`.
3. Document the icon in this README's table.
4. Add a changeset (`pnpm changeset`) and open a PR — release happens
   automatically on merge.

## License

Apache-2.0
