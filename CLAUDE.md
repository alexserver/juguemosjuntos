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
- `src/data/` - JSON data files for products and characters
- `public/` - Static assets served as-is (images, fonts, etc.)
- `dist/` - Build output (excluded from git)

### TypeScript Configuration
The project uses Astro's strict TypeScript configuration (`astro/tsconfigs/strict`). Type definitions are generated in `.astro/types.d.ts` during development.

### Astro Component Structure
Astro components (`.astro` files) have two sections:
1. **Frontmatter** (between `---` delimiters) - Server-side JavaScript/TypeScript
2. **Template** - HTML-like markup with component slots and expressions

## Data Management

### Product Data
Product information (pricing, names) is stored in `src/data/products.json`:
```json
[
  {
    "id": "halloween",
    "name": "Lotería de Halloween",
    "price": 120,
    "currency": "MXN"
  },
  {
    "id": "christmas",
    "name": "Lotería de Navidad",
    "price": 120,
    "currency": "MXN"
  }
]
```

**Usage in pages:**
- Import: `import productsData from "../data/products.json"`
- Find product: `const product = productsData.find((p) => p.id === "halloween")`
- Format price: `` const formattedPrice = `$${product?.price} ${product?.currency}` ``
- Prices are displayed above CTA buttons with "+ envío" text to indicate shipping is additional

**To update prices:** Simply edit `src/data/products.json` and rebuild the site.

### Character Data
Character information is stored in separate JSON files:
- `src/data/characters.json` - Halloween characters (24 characters)
- `src/data/christmas-characters.json` - Christmas characters (24 characters)

Each character has:
```json
{
  "id": 1,
  "name": "Character Name",
  "filename": "01.png"
}
```

Characters are combined with their images in the page frontmatter for rendering.

## Environment Variables

The project uses environment variables for WhatsApp integration. Variables are stored in `.env` (local) and `.env.example` (template).

**Current environment variables:**
- `PUBLIC_WHATSAPP_NUMBER` - WhatsApp number without + or spaces (e.g., 5215512345678)
- `PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN` - Default message for Halloween product inquiries
- `PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS` - Default message for Christmas product inquiries

**Note:** Variables prefixed with `PUBLIC_` are accessible in both server and client code via `import.meta.env.PUBLIC_*`
