# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # Start dev server at http://localhost:4321
pnpm build        # Type-check + build (astro check && astro build)
pnpm preview      # Preview production build
pnpm deploy:vercel      # Deploy to Vercel
pnpm deploy:cloudflare  # Build + deploy to Cloudflare Pages
```

Requires Node.js v22.x and pnpm.

## Architecture

### Data Flow
All portfolio content lives in `cv.json` — a single source of truth for bio, work history, education, projects, and skills. Components import it via the `@cv` path alias. Edit `cv.json` to update any portfolio content; never hardcode content in components.

### Two UI Modes
The portfolio has two distinct interfaces:
- **Standard** (`/`) — uses `src/layouts/Layout.astro`, renders section components from `src/components/sections/`
- **Neovim terminal** (`/neovim/*`) — uses `src/layouts/Neovim.astro`, renders a Neovim editor-style UI with `src/components/neovim/` components (Sidebar, StatusBar, KeyBindings)

### Theming
Themes are CSS custom properties in `src/globals.css`, toggled via `data-theme` on `<body>`. Tailwind uses these via the custom color palette in `tailwind.config.mjs` (nvim-bg, nvim-fg, etc. for the Neovim palette; `--color`, `--muted`, etc. for the standard theme).

### Path Aliases (tsconfig.json)
```
@/*           → src/*
@cv           → ./cv.json
@components/* → src/components/*
@layouts/*    → src/layouts/*
@icons/*      → src/icons/*
```

### Custom Cursor
`src/lib/contextCursor/` is a custom cursor library excluded from TypeScript strict checking. It's initialized in `src/scripts/main.ts` and activated via `data-ccursor` attributes on HTML elements.

### Astro Islands
React is used only for interactive components (e.g., `src/components/profileImage/`). Most UI is static Astro components. Keep React usage minimal — prefer Astro for anything that doesn't need client-side interactivity.

### Deployment Configs
- `astro.config.mjs` — Vercel serverless (primary)
- `astro.config.vercel.mjs` — alternate Vercel config
- `netlify.toml` — Netlify support
- Cloudflare via `wrangler` (see deploy script)
