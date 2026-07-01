# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A [Slidev](https://sli.dev/) presentation (conference talk) titled "CJS, ESM, WTF ??" — a ~50min talk about JavaScript module systems (CommonJS vs ES Modules) given at Lyon JS. The slide content and speaker notes are written in **French**.

## Commands

Package manager is **pnpm** (`pnpm-lock.yaml`, `.npmrc` with `shamefully-hoist`). Use `pnpm`, not npm.

- `pnpm dev` — start the live dev server with hot reload and open the browser (http://localhost:3030)
- `pnpm build` — build the static SPA into `dist/`
- `pnpm export` — export slides to PDF/PNG via `slidev export`

There is no test or lint setup.

## Architecture

The entire talk lives in **`slides.md`** — a single Markdown file where slides are separated by `---`. This is the file you'll edit for almost all content changes. Key conventions used throughout:

- **Speaker notes** are HTML comments (`<!-- ... -->`) placed at the end of a slide block, in French.
- **Click animations** use Slidev's `v-click` / `v-click="[start, end]"` / `v-click.hide` directives on HTML elements to reveal/hide content step by step.
- **MDC syntax is enabled** (`mdc: true` in frontmatter) — allows attaching classes/attributes to Markdown inline.
- Layout is driven by **UnoCSS utility classes** (Tailwind-like, e.g. `absolute top-30px`, `flex gap-5`) applied directly in the Markdown/HTML.
- Per-slide frontmatter (the block after a `---` separator) sets `layout:`, `class:`, transitions, etc.

Supporting pieces around `slides.md`:

- **`theme/`** — a **vendored, locally-modified copy of the Slidev default theme** (`theme: ./theme` in frontmatter). Custom layouts live in `theme/layouts/*.vue` (cover, section, quote, fact, statement, intro) and styles in `theme/styles/`. Edit here to change layout/visual behavior rather than overriding inline.
- **`components/`** — Vue components auto-imported into slides by Slidev (e.g. `Counter.vue`, usable as `<Counter />` in `slides.md`).
- **`assets/`** — images referenced from slides via `./assets/...` (resolved against the base URL by `theme/layoutHelper.ts`).
- **`public/`** — static files served at the site root (e.g. `/lego_computer_3-1.png`, referenced without `./`).
- **`snippets/`** — standalone `.ts` source used as embeddable code samples in slides.
- **`setup/shiki.ts`** — Shiki syntax-highlighting config (catppuccin themes).
- **`pages/`** — additional slide files that can be pulled into `slides.md` via `src:` (the default `imported-slides.md` is example boilerplate).

The `react` / `react-dom` / `@codesandbox/sandpack-react` / `sandpack-vue3` / `veaury` dependencies are present to embed interactive code sandboxes; `veaury` bridges React components into the Vue-based Slidev runtime.

## Deployment

Three deploy targets are configured; the primary one is GitHub Pages:

- **GitHub Pages** (`.github/workflows/deploy.yml`) — builds on push to `main` with `nr build --base /<repo-name>/`. The `--base` flag matters: assets break if the base path is wrong for the host.
- **Netlify** (`netlify.toml`) and **Vercel** (`vercel.json`) — both build to `dist/` with SPA rewrites to `index.html`.
