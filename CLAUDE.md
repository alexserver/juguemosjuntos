# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based web application using the minimal starter template. Astro is a modern static site builder that supports file-based routing and multiple frontend frameworks.

## Development Commands

Run all commands from the project root:

- `npm run dev` - Start development server at `localhost:4321`
- `npm run build` - Build production site to `./dist/`
- `npm run preview` - Preview production build locally
- `npm run astro ...` - Run Astro CLI commands (e.g., `astro add`, `astro check`)

## Architecture

### Routing
Astro uses file-based routing. Files in `src/pages/` automatically become routes:
- `src/pages/index.astro` → `/`
- `src/pages/about.astro` → `/about`
- `src/pages/blog/[slug].astro` → `/blog/:slug` (dynamic route)

### Project Structure
- `src/pages/` - Route-based pages (`.astro` or `.md` files)
- `src/components/` - Reusable Astro/React/Vue/Svelte/Preact components
- `public/` - Static assets served as-is (images, fonts, etc.)
- `dist/` - Build output (excluded from git)

### TypeScript Configuration
The project uses Astro's strict TypeScript configuration (`astro/tsconfigs/strict`). Type definitions are generated in `.astro/types.d.ts` during development.

### Astro Component Structure
Astro components (`.astro` files) have two sections:
1. **Frontmatter** (between `---` delimiters) - Server-side JavaScript/TypeScript
2. **Template** - HTML-like markup with component slots and expressions
