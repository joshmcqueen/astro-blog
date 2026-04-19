# joshmcqueen.com

Personal blog built with [Astro 6](https://astro.build). Deployed to [joshmcqueen.com](https://joshmcqueen.com) via Dokploy on every push to `main`.

## Commands

| Command | Action |
| :--- | :--- |
| `pnpm install` | Install dependencies |
| `pnpm dev` | Start dev server at `localhost:4321` |
| `pnpm build` | Build to `./dist/` |
| `pnpm preview` | Preview production build locally |

Node >= 22.12.0 required.

## Content

Blog posts live in `src/content/blog/` as `.md` or `.mdx` files. Frontmatter requires `title`, `description`, and `pubDate`; `updatedDate` and `heroImage` are optional.

## Deployment

Push to `main` → GitHub webhook → Dokploy builds the Docker image and swaps the container.

The Dockerfile uses a multi-stage build: `node:22-slim` to build the static site, `nginx:alpine` to serve it.

The site sits behind Cloudflare (DNS proxy + CDN). nginx access logs are disabled; only warn-level errors surface in Dokploy logs.
