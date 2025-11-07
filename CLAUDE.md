# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a monorepo containing two independent Astro-based web applications:
- **site-es**: Spanish version for the Mexican market (physical products)
- **site-en**: English version for international market (digital PDFs)

Each app is completely independent with its own configuration, dependencies, and deployment.

## Development Commands

Run all commands from the project root:

### Working with Spanish App (site-es)
- `npm run dev:es` - Start development server at `localhost:4321`
- `npm run build:es` - Build production site to `apps/site-es/dist/`
- `npm run preview:es` - Preview production build locally

### Working with English App (site-en)
- `npm run dev:en` - Start development server at `localhost:4321`
- `npm run build:en` - Build production site to `apps/site-en/dist/`
- `npm run preview:en` - Preview production build locally

### Working with Both Apps
- `npm run dev:all` - Start both dev servers
- `npm run build:all` - Build both apps

## Architecture

### Monorepo Structure
```text
/
├── apps/
│   ├── site-es/              # Spanish app
│   │   ├── src/
│   │   │   ├── pages/        # Spanish pages (index, halloween, christmas)
│   │   │   ├── components/   # Spanish components
│   │   │   ├── layouts/      # Layout components
│   │   │   ├── data/         # Spanish data (products, characters)
│   │   │   ├── assets/       # Images and assets
│   │   │   └── styles/       # Global styles
│   │   ├── public/           # Public static assets
│   │   ├── astro.config.mjs  # Spanish app configuration
│   │   ├── package.json      # Spanish app dependencies
│   │   └── .env              # Spanish environment variables
│   └── site-en/              # English app (same structure)
├── specs/                    # Project specifications
└── package.json              # Root workspace configuration
```

### Routing (Per App)
Each app uses file-based routing independently:
- `src/pages/index.astro` → `/`
- `src/pages/halloween.astro` → `/halloween`
- `src/pages/christmas.astro` → `/christmas`

**Important:** Pages are at the root of each app's `src/pages/` directory (not in language subdirectories).

### Component Import Paths
From pages, use single-level relative imports:
```javascript
import Button from "../components/Button.astro";
import Header from "../components/Header.astro";
import productsData from "../data/products.json";
import logo from "../assets/images/logos/juguemos-juntos-logo.png";
```

### TypeScript Configuration
Each app has its own `tsconfig.json` using Astro's strict TypeScript configuration (`astro/tsconfigs/strict`). Type definitions are generated in `.astro/types.d.ts` during development.

## Data Management

### Product Data

**Spanish (apps/site-es/src/data/products.json):**
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

**English (apps/site-en/src/data/products.json):**
```json
[
  {
    "id": "halloween",
    "name": "Halloween Lotería",
    "price": 15,
    "currency": "USD"
  },
  {
    "id": "christmas",
    "name": "Christmas Lotería",
    "price": 15,
    "currency": "USD"
  }
]
```

**Usage in pages:**
```javascript
import productsData from "../data/products.json";

const product = productsData.find((p) => p.id === "halloween");
const formattedPrice = `$${product?.price} ${product?.currency}`;
```

**To update prices:** Edit the relevant app's `src/data/products.json` and rebuild.

### Character Data

Each app has its own character data with language-specific names:

**Spanish (apps/site-es/src/data/characters.json):**
```json
{
  "halloween": [
    { "id": 1, "name": "La Brujita", "filename": "01.png" },
    { "id": 2, "name": "El Fantasma", "filename": "02.png" }
  ],
  "christmas": [
    { "id": 1, "name": "Santa Claus", "filename": "01.png" }
  ]
}
```

**English (apps/site-en/src/data/characters.json):**
```json
{
  "halloween": [
    { "id": 1, "name": "The Little Witch", "filename": "01.png" },
    { "id": 2, "name": "The Ghost", "filename": "02.png" }
  ],
  "christmas": [
    { "id": 1, "name": "Santa Claus", "filename": "01.png" }
  ]
}
```

## Environment Variables

Each app has its own `.env` file:

**Spanish (apps/site-es/.env):**
```
PUBLIC_WHATSAPP_NUMBER=529845937808
PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN=Hola! Me interesa comprar la Lotería de Halloween...
PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS=Hola! Me interesa comprar la Lotería de Navidad...
```

**English (apps/site-en/.env):**
```
PUBLIC_WHATSAPP_NUMBER=529845937808
PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN=Hi! I'm interested in buying the Halloween Lotería...
PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS=Hi! I'm interested in buying the Christmas Lotería...
```

**Note:** Variables prefixed with `PUBLIC_` are accessible in both server and client code via `import.meta.env.PUBLIC_*`

## Making Changes

### Updating Both Apps
When a change applies to both apps (e.g., UI fixes, component updates):

1. **Make the change in one app first:**
   ```
   Edit apps/site-es/src/components/Button.astro
   ```

2. **Test it works:**
   ```
   npm run dev:es
   ```

3. **Copy to the other app:**
   ```
   Edit apps/site-en/src/components/Button.astro
   (adapt any language-specific text)
   ```

4. **Test the second app:**
   ```
   npm run dev:en
   ```

**Or use Claude Code to update both:**
When the user asks to update something in "both apps" or "both sites", make the change in both `apps/site-es` and `apps/site-en` in a single response.

### Updating One App
For language-specific or market-specific changes, edit only the relevant app:
- Spanish-only changes: Edit files in `apps/site-es/`
- English-only changes: Edit files in `apps/site-en/`

### Common Update Scenarios

**Update product pricing:**
- Edit `apps/site-es/src/data/products.json` for Spanish prices
- Edit `apps/site-en/src/data/products.json` for English prices

**Fix a UI bug:**
- Fix in both `apps/site-es/src/components/` and `apps/site-en/src/components/`

**Add a new page (e.g., "Día de Muertos" - Spanish only):**
- Create `apps/site-es/src/pages/dia-de-muertos.astro`
- Update navigation in `apps/site-es/src/components/Header.astro`
- No changes needed in English app

## Components

### Shared Components (duplicated in both apps)
- `Button.astro` - Reusable button component
- `Header.astro` - Site header with navigation (language-specific text)
- `Footer.astro` - Site footer with links (language-specific text)
- `ValueCard.astro` - Value proposition cards
- `TestimonialCard.astro` - Customer testimonials
- `FeatureList.astro` - Feature lists
- `StepCard.astro` - Step-by-step cards

### Simplified from Original i18n Implementation
The following have been removed:
- `LanguageSwitcher.astro` - No longer needed (complete isolation)
- `src/utils/i18n.ts` - No language detection or translation system
- Complex language routing logic

**Header and Footer are now simplified with hardcoded navigation:**
- Spanish: "Inicio", "Sobre Nosotros", "Juegos"
- English: "Home", "About Us", "Games"

## Deployment

Each app deploys to a separate domain:

**Spanish App (apps/site-es):**
- Domain: `juguemosjuntos.mx` or `juguemosjuntos.com.mx`
- Build command: `npm run build`
- Build directory: `dist`
- Base directory: `apps/site-es`
- Environment variables: From `apps/site-es/.env`

**English App (apps/site-en):**
- Domain: `juguemosjuntos.com` or `juguemosjuntos.us`
- Build command: `npm run build`
- Build directory: `dist`
- Base directory: `apps/site-en`
- Environment variables: From `apps/site-en/.env`

## Business Model Differences

**Important:** The two apps have different business models:

**Spanish (site-es):**
- Sells physical printed products
- Pricing: 120 MXN + shipping
- Copy emphasizes: "versión impresa" and "+ envío"

**English (site-en):**
- Sells digital PDF downloads
- Pricing: 15 USD
- Copy emphasizes: "Digital PDF ready to print at home - instant download!"

These differences are intentional for market segmentation and piracy prevention.

## Rationale

### Why Monorepo Instead of Unified i18n?
1. **Complete Isolation**: Prevents piracy concerns by making it impossible to discover the English (digital) version from the Spanish (physical) site
2. **Different Business Models**: Spanish sells physical products, English sells digital PDFs
3. **Single Development Context**: One VSCode window, one Claude Code session
4. **Easy Maintenance**: Can update both apps in one session
5. **Flexibility to Diverge**: Each market can evolve independently

### Accepted Trade-offs
- **Code Duplication**: Components and assets are duplicated (~2x storage)
- **Update Overhead**: Changes need to be applied to both apps (~1.5x time)
- **No Shared Packages**: Apps don't share code at runtime (enforces isolation)

These trade-offs are acceptable because:
- Content updates are quick even with duplication
- Claude Code can update both apps in one response
- Complete isolation is more important than DRY for this use case

## Troubleshooting

### Build Errors
If you get module resolution errors during build:
- Check import paths use `../` not `../../` (pages are at root of src/pages/)
- Verify assets exist in `src/assets/` directory
- Check data files exist in `src/data/` directory

### Development Server Port Conflicts
Both apps use port 4321 by default. To run both simultaneously:
- Start one: `npm run dev:es`
- Start the other in a different terminal (it will auto-assign a different port)
- Or use `npm run dev:all` to start both at once

### Environment Variables Not Working
- Ensure variables are prefixed with `PUBLIC_` for client-side access
- Restart dev server after changing `.env` files
- Check the correct `.env` file (each app has its own)
