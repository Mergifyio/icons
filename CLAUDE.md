# @mergify/icons

This repo publishes Mergify's product icons as raw SVG files for use across
the dashboard, docs, and marketing site.

## Structure

- `icons/*.svg` — the source SVGs, kebab-case filenames.
- `package.json` — the `exports` map aliases each icon as
  `@mergify/icons/<name>.svg`, hiding the `icons/` segment from consumers.
- `README.md` — public usage docs and the icon table.
- `.changeset/` — pending release entries (Changesets).
- `.github/workflows/release.yml` — auto-publishes to npm on merge to `main`
  when a changeset is present.

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
- **Versioning**: never bump the version manually. Add a changeset
  (`pnpm changeset`); the GitHub Action handles publishing the next time a
  changeset lands on `main`.

## Adding a new icon

The repository ships a Claude Code skill that walks through it end-to-end:
`.claude/skills/add-icon/SKILL.md`. Trigger it with "add icon" or
"ajouter une icône" in a Claude Code session.

Manual checklist if working without the skill:

1. Drop the SVG in `icons/<name>.svg` and normalize colors to `currentColor`.
2. Add an entry under `exports` in `package.json` (alphabetical order).
3. Add a row to the README icon table (suggested brand color + viewBox).
4. Run `pnpm changeset` and describe the icon.
5. Commit, push, open a PR.
