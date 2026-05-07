# @mergify/icons

This repo publishes Mergify's product icons as raw SVG files for use across
the dashboard, docs, and marketing site.

## Structure

- `icons/*.svg` — the source SVGs, kebab-case filenames.
- `package.json` — the `exports` map aliases each icon as
  `@mergify/icons/<name>.svg`, hiding the `icons/` segment from consumers.
- `README.md` — public usage docs and the icon table.
- `.github/workflows/release.yml` — publishes to npm via OIDC Trusted
  Publishing when a GitHub Release is published.
- `.mergify.yml` — merge protections (2 approvals required, auto-request
  reviews from `@devs`, squash-merge via the default queue).

## Conventions

- **`currentColor` everywhere.** Each SVG ships with `stroke="currentColor"`
  and/or `fill="currentColor"` so consumers pick the color via CSS.
  Multi-color brand logos are the only allowed exception.
- **Filenames** are kebab-case lowercase (`merge-queue.svg`, not
  `Merge-Queue.svg`). Linux CI is case-sensitive.
- **No build step.** The package ships the SVG files as-is; consumers
  handle them through their own bundler (Vite `?react`, Astro native SVG,
  plain `<img>`, etc.).
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/)
  (`feat: add X icon`, `fix: viewBox on Y`, etc.).

## Releasing

The npm publish is triggered by a **GitHub Release**, not by merging to
`main`. There's no `NPM_TOKEN`; npmjs.com authenticates the GitHub Action
via OIDC Trusted Publishing (configured once on the package settings page).

Steps:

1. Make sure `main` is in the state you want to ship.
2. Create a GitHub Release with a semver tag — e.g. `0.1.0`, `0.2.0`,
   `1.0.0`. The `v` prefix is optional (the workflow strips it).
3. The release event triggers `.github/workflows/release.yml`, which sets
   the version in `package.json` from the tag, then runs `pnpm publish
   --provenance --access public`.

Use the GitHub Release notes to describe what changed — they're the
public changelog. Bumping `package.json#version` manually before the
release is unnecessary; the workflow does it from the tag.

## Adding a new icon

The repository ships a Claude Code skill that walks through it end-to-end:
`.claude/skills/add-icon/SKILL.md`. Trigger it with "add icon" or
"ajouter une icône" in a Claude Code session.

Manual checklist if working without the skill:

1. Drop the SVG in `icons/<name>.svg` and normalize colors to `currentColor`.
2. Add an entry under `exports` in `package.json` (alphabetical order).
3. Add a row to the README icon table (suggested brand color + viewBox).
4. Commit, push, open a PR.
5. After merge, create a GitHub Release with the new version tag to publish.
