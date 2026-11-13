# @mergifyio/icons

Mergify's product icons as raw SVG files. Each icon ships in `currentColor`
so the consumer controls the color through CSS — see the suggested brand
color per product below.

## Icons

| File | Suggested brand color | viewBox |
|---|---|---|
| `merge-queue.svg` | `#2AA77E` (teal) | `0 0 40 40` |
| `merge-protections.svg` | `#2086C5` (blue) | `0 0 40 40` |
| `ci-insights.svg` | `#5C68F0` (indigo) | `0 0 40 40` |
| `test-insights.svg` | `#9C43E5` (purple) | `0 0 40 40` |
| `stacks.svg` | `#E61E71` (rose) | `0 0 32 32` |
| `quarantined-healthy.svg` | `#17B26A` (success green) | `0 0 24 24` |
| `quarantined-flaky.svg` | `#F79009` (warning orange) | `0 0 24 24` |
| `quarantined-broken.svg` | `#F04438` (error red) | `0 0 24 24` |
| `scope.svg` | — | `0 0 24 24` |
| `all-scopes.svg` | — | `0 0 24 24` |
| `split-x.svg` | `#F04438` (error red) | `0 0 24 24` |
| `split-check.svg` | `#17B26A` (success green) | `0 0 24 24` |
| `split-dash.svg` | `#F79009` (warning orange) | `0 0 24 24` |
| `git-merge-x.svg` | — | `0 0 24 24` |

`scope.svg` marks one scope: four tiles in a square, one filled and three
outlined. `all-scopes.svg` fills all four tiles, for something that
touches every scope.

### Status glyphs

`split-x`, `split-check`, `split-dash` and `git-merge-x` fill gaps in
[Lucide](https://lucide.dev), so they are drawn the way Lucide draws its
icons: 24-unit grid, stroke width 2, round caps and joins, stroke in
`currentColor`. They sit next to Lucide icons at any size.

- `split-x`: the pull request a bisection found to be the cause of the failure.
- `split-check`: a pull request the bisection cleared.
- `split-dash`: a pull request removed before it was tested.
- `git-merge-x`: a merge conflict. It has no suggested color: a conflict
  that delays a pull request and one that removes it do not call for the
  same one.

The `split-*` icons are derived from Lucide's `split`, and `git-merge-x`
from its `git-merge`, under the ISC License: see
[`LICENSE-LUCIDE`](LICENSE-LUCIDE).

## Install

```sh
pnpm add @mergifyio/icons
```

## Usage

The package ships raw SVG files. Each consumer imports them through its
bundler's SVG loader.

### Vite (dashboard)

```ts
// As a React component (with vite-plugin-svgr or similar):
import MergeQueueIcon from '@mergifyio/icons/merge-queue.svg?react';

<MergeQueueIcon width={40} height={40} />

// As a URL:
import url from '@mergifyio/icons/merge-queue.svg';

<img src={url} alt="Merge Queue" />

// As inline string (?raw):
import svg from '@mergifyio/icons/merge-queue.svg?raw';
```

### Astro (docs / mergify.com)

```astro
---
import MergeQueue from '@mergifyio/icons/merge-queue.svg';
---
<MergeQueue width={40} height={40} />
```

(Astro 5+ has [native SVG component support](https://docs.astro.build/en/guides/images/#svgs).)

### Plain HTML / Markdown

```html
<img src="/node_modules/@mergifyio/icons/merge-queue.svg" alt="Merge Queue" />
```

## Adding new icons

1. Drop the new SVG under `icons/` with a kebab-case filename
   (e.g. `runner.svg`).
2. Add an entry to the `exports` field in `package.json`.
3. Document the icon in this README's table.
4. Open a PR. After merge, a maintainer cuts a GitHub Release with the
   next semver tag — that publishes the new version to npm.

## License

Apache-2.0, except the parts of `split-x.svg`, `split-check.svg`,
`split-dash.svg` and `git-merge-x.svg` taken from Lucide, which stay under
the ISC License ([`LICENSE-LUCIDE`](LICENSE-LUCIDE)).
