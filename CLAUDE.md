# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev        # Start dev server at localhost:4321
pnpm build      # Build production site to ./dist/
pnpm preview    # Preview production build locally
```

No lint or test scripts are configured.

## Architecture

This is an **Astro 5** static portfolio site deployed to **Cloudflare Workers** via `@astrojs/cloudflare`. Styling uses **Tailwind CSS 4** with the `@tailwindcss/vite` plugin (not the legacy PostCSS integration).

### Page & Component Structure

- `src/pages/index.astro` — homepage entry point
- `src/pages/blogs/*.md` — blog posts using `BlogLayout.astro`
- `src/components/home/MainSection.astro` — orchestrates all homepage sections in order: Intro → Professional → Data Science Projects → SWE Projects → Blog
- `src/components/home/section/` — individual homepage section components
- `src/components/Header.astro` — navigation with inline mobile menu toggle script
- `src/components/sprite/cat_one.astro` — logo/mascot component

### Layouts

- `src/layout/Head.astro` — SEO metadata, Open Graph, Twitter cards, JSON-LD schema, and global style imports
- `src/layout/Main.astro` — body layout wrapper
- `src/layout/BlogLayout.astro` — layout for markdown blog posts

### Styling

- `src/styles/global.css` — Tailwind 4 `@theme` directive defines a custom coffee/cappuccino color palette (`cappuccino-*`, `cream`, `coffee`, `sugar`) and custom animations (`typing`, `blink`, `fadeIn`, `bounce`)
- Font: Roboto Mono throughout

### Icons

Icons use `astro-icon` with the `mdi` (Material Design Icons) namespace. Reference icons as `mdi:icon-name`.

### Deployment

Cloudflare adapter is configured in `astro.config.mjs`. Wrangler config is in `wrangler.jsonc` (project name: `my-portfolio`, assets served from `./dist/`).
