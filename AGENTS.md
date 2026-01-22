# AGENTS.md

This document provides guidance for AI coding agents working with this codebase.

## Project Overview

**Type**: Personal blog/website for Craig Dennis  
**Domain**: craigsdennis.dev  
**Framework**: Astro 5.x (static site generator)  
**Deployment**: Cloudflare Workers/Pages  
**Language**: TypeScript with Astro components

## Tech Stack

- **Framework**: Astro 5.x
- **Adapter**: @astrojs/cloudflare
- **Content**: MDX + Markdown via Astro Content Collections
- **Styling**: Custom CSS (no framework)
- **Deployment**: Cloudflare Workers via Wrangler

## Directory Structure

```
src/
├── assets/              # Images for blog posts (processed by Astro)
├── components/          # Reusable .astro components
├── content/blog/        # Markdown/MDX blog posts
├── layouts/             # Page layout templates
├── pages/               # File-based routing
├── styles/global.css    # Global styles and design system
├── consts.ts            # Site configuration constants
└── content.config.ts    # Content collection schema
public/                  # Static assets (favicon, fonts)
```

## Common Commands

```bash
npm run dev      # Start local dev server (localhost:4321)
npm run build    # Build production site to ./dist/
npm run preview  # Build and run with Wrangler locally
npm run deploy   # Build and deploy to Cloudflare
```

## Blog Post Conventions

### Frontmatter Schema

Blog posts in `src/content/blog/` require this frontmatter:

```yaml
---
title: 'Post Title'           # Required
description: 'Brief summary'  # Required
pubDate: 'Jan 20 2026'        # Required (auto-coerced to Date)
updatedDate: 'Jan 21 2026'    # Optional
heroImage: '../../assets/image.webp'  # Optional
---
```

### Image Handling

- Place blog images in `src/assets/`
- Reference images relatively from MDX: `../../assets/filename.webp`
- Use the `Image` component from `astro:assets` for optimized images

### Content Collection API

```typescript
import { getCollection } from 'astro:content';
const posts = await getCollection('blog');
```

## Code Style

- **Indentation**: Tabs
- **Quotes**: Single quotes in TypeScript, double in HTML attributes
- **Semicolons**: Yes
- **Component naming**: PascalCase (`BlogPost.astro`)
- **CSS classes**: kebab-case (`post-card`, `hero-content`)
- **Constants**: SCREAMING_SNAKE_CASE (`SITE_TITLE`)

## Astro Component Pattern

```astro
---
// TypeScript frontmatter (server-side)
interface Props {
  title: string;
}
const { title } = Astro.props;
---

<div class="container">
  <h1>{title}</h1>
  <slot />
</div>

<style>
  .container { /* scoped styles */ }
</style>
```

## Design System

The site uses a dark theme with a "retro terminal meets modern educator" aesthetic.

### Key CSS Variables

- Colors: `--bg-primary`, `--accent-cyan`, `--accent-amber`, `--accent-pink`
- Fonts: `--font-mono` (JetBrains Mono), `--font-sans` (Source Sans 3)
- Spacing: `--space-xs` through `--space-xl`

### Visual Elements

- Terminal-inspired prompts (`$` prefix)
- Card components with hover glow effects
- Staggered fade-in animations

## Environment Variables

- Store secrets in `.dev.vars` (gitignored)
- Use `.dev.vars.example` as a template
- Never commit actual secrets

## Important Notes

1. **Routing**: File-based in `src/pages/`, dynamic routes use `[...slug].astro`
2. **Build output**: `dist/` and `.astro/` are gitignored
3. **TypeScript**: Strict mode enabled via `astro/tsconfigs/strict`
4. **SSR**: Cloudflare adapter provides `Astro.locals` with runtime bindings
5. **No testing framework**: Relies on TypeScript checking and manual testing
