# AGENTS.md

## Cursor Cloud specific instructions

This is an Astro 6 static blog (`joshmcqueen.com`). See `README.md` and `CLAUDE.md` for the architecture and the standard `pnpm` commands.

- Package manager is `pnpm` (see `pnpm-lock.yaml`); Node >= 22.12.0 is required and already installed.
- Dev server: `pnpm dev` serves at `http://localhost:4321`. It hot-reloads content in `src/content/blog/`, so adding/editing a `.md`/`.mdx` post shows up live without a restart. Add `--host` if you need it reachable on the VM network interface.
- Build/preview: `pnpm build` outputs static HTML to `./dist/`; `pnpm preview` serves that build.
- No test suite and no lint/format tooling are configured (no ESLint/Prettier/`astro check` dependency). "Testing" a change means building and/or viewing it on the dev server.
- The `pnpm install` warning about ignored build scripts (`esbuild`, `sharp`) is harmless: `sharp` ships prebuilt binaries, and image optimization during `pnpm build` succeeds without approving those scripts. Do not run the interactive `pnpm approve-builds`.
