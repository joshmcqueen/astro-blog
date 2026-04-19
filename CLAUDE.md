# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev       # Start dev server at localhost:4321
pnpm build     # Build production site to ./dist/
pnpm preview   # Preview production build locally
pnpm astro     # Run Astro CLI directly (e.g., pnpm astro add <integration>)
```

Node >=22.12.0 required. No test suite is configured.

## Architecture

This is an **Astro 6** blog using the Content Collections API (v2 loader syntax).

**Routing:** File-based. Pages in `src/pages/` map directly to routes. Blog post slugs derive from filenames via `[...slug].astro`.

**Content:** Blog posts live in `src/content/blog/` as `.md` or `.mdx` files. The collection schema (defined in `src/content.config.ts`) requires `title`, `description`, and `pubDate`; `updatedDate` and `heroImage` are optional. Posts are loaded via glob and queried with `getCollection('blog')`. Slugs are derived from `post.id` (the filename without extension).

**Layouts/Components:** `src/layouts/BlogPost.astro` wraps individual posts. Shared UI lives in `src/components/` (BaseHead, Header, Footer, etc.). Global styles are in `src/styles/`.

**Site config:** Update `src/consts.ts` for `SITE_TITLE` / `SITE_DESCRIPTION`. Production URL is `https://joshmcqueen.com` (set in `astro.config.mjs`).

**Integrations:** MDX (`@astrojs/mdx`), Sitemap (`@astrojs/sitemap`), RSS (`src/pages/rss.xml.js`), and image optimization via `sharp`. The Atkinson font is served locally via `fontProviders.local()` with CSS variable `--font-atkinson`.

## Deployment

Docker multi-stage build (Node 22-slim build → nginx:alpine serve). Pushing to `main` triggers a GitHub webhook to Dokploy, which rebuilds the container. `Dockerfile` and `nginx.conf` are in the repo root.

The site sits behind Cloudflare (DNS proxy + CDN). nginx access logs are disabled (`access_log off;`) to avoid noise in Dokploy; only warn-level errors are logged. Real client IPs arrive via `CF-Connecting-IP` header, not the TCP remote address.
