# Spec 06: Monorepo Restructure for Spanish/English Isolation

**Status:** Proposed
**Date:** 2025-11-06
**Author:** System Design

---

## Executive Summary

Transform the current i18n implementation from a unified Astro app with `/es/` and `/en/` routes into a simple monorepo containing two completely independent applications (`site-es` and `site-en`). Each app will be deployed to separate domains to provide complete market isolation while maintaining a single development context for efficient maintenance.

---

## Background

### Current Implementation

The project currently uses Astro's i18n routing with:
- Unified codebase with `/es/` and `/en/` route prefixes
- Shared components with language detection logic
- Language switcher allowing easy toggling between versions
- Single deployment to one domain
- Different business models per market:
  - Spanish (ES): Physical products with shipping (120 MXN)
  - English (EN): Digital PDF downloads (15 USD)

### Problem Statement

**Primary Concern: Piracy Risk**
- Mexican market has higher piracy rates than US market
- Easy discoverability of English version from Spanish site creates risk
- Language switcher makes it trivial to find digital PDF offering
- Concern that Mexican customers will buy digital PDF and resell physical copies

**Secondary Concerns:**
- Different business models require different logic/copy
- Uncertain future - may need to diverge features significantly
- Current i18n complexity adds overhead for simple content updates
- Maintenance burden of keeping both versions in sync when they serve different purposes

**Requirements:**
- ✅ Complete isolation between markets (no cross-discovery)
- ✅ Single development context (one VSCode, one Claude session)
- ✅ Easy maintenance for frequent content updates
- ✅ Flexibility to diverge features in the future
- ✅ Separate domains for each market

---

## Options Considered

### Option 1: Separate Repositories + Separate Domains

**Structure:** Two completely independent Git repositories

**Pros:**
- Complete isolation at code level
- Maximum flexibility to diverge
- Simplest code (no i18n logic)
- Independent deployments

**Cons:**
- ❌ Two VSCode windows
- ❌ Two Claude Code sessions
- ❌ Manual copy/paste between repos
- ❌ Duplicate maintenance effort
- ❌ Harder to keep visual consistency

**Verdict:** ❌ Rejected - Maintenance overhead too high for frequent content updates

---

### Option 2: Monorepo with Shared Package System

**Structure:** Single repo with `apps/` and `packages/` directories

```
juguemosjuntos/
├── apps/
│   ├── site-es/
│   └── site-en/
└── packages/
    ├── ui/           # Shared components
    └── data/         # Shared data files
```

**Pros:**
- DRY components (fix once, applies to both)
- Single source of truth for data
- Professional architecture
- Scalable to more markets

**Cons:**
- ❌ Complex setup (internal packages, build dependencies)
- ❌ Slower iteration (must build packages first)
- ❌ Overkill for 2 sites
- ❌ More coupling between apps

**Verdict:** ❌ Rejected - Too complex for current needs (can migrate to this later if needed)

---

### Option 3: Simple Monorepo (Two Independent Apps) ⭐

**Structure:** Single repo with two completely separate apps

```
juguemosjuntos/
├── apps/
│   ├── site-es/      # Complete Spanish app
│   └── site-en/      # Complete English app
├── specs/
├── package.json      # Workspace management
└── README.md
```

**Pros:**
- ✅ Complete isolation (apps can't import from each other)
- ✅ Single development context (one repo, one VSCode)
- ✅ Easy to copy changes between apps when desired
- ✅ Flexible sharing (copy components as needed)
- ✅ Can diverge freely without coordination
- ✅ Simple mental model (just two projects that coexist)
- ✅ Claude Code has full context of both apps
- ✅ Easy to split into separate repos later if needed

**Cons:**
- ❌ Some code duplication
- ❌ Changes must be applied to both apps manually
- ❌ Assets duplicated across apps

**Verdict:** ✅ **SELECTED** - Best balance of isolation, maintainability, and flexibility

---

## Architecture Design

### Directory Structure

```
juguemosjuntos/
├── apps/
│   ├── site-es/                    # Spanish Lotería Site
│   │   ├── src/
│   │   │   ├── pages/
│   │   │   │   ├── index.astro
│   │   │   │   ├── halloween.astro
│   │   │   │   └── christmas.astro
│   │   │   ├── components/
│   │   │   │   ├── Header.astro
│   │   │   │   ├── Footer.astro
│   │   │   │   ├── Button.astro
│   │   │   │   └── ... (all components)
│   │   │   ├── layouts/
│   │   │   │   └── Layout.astro
│   │   │   └── data/
│   │   │       ├── products.json
│   │   │       ├── characters.json
│   │   │       └── christmas-characters.json
│   │   ├── public/
│   │   │   ├── images/
│   │   │   └── fonts/
│   │   ├── astro.config.mjs
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── .env
│   │
│   └── site-en/                    # English Lotería Site
│       ├── src/
│       │   ├── pages/
│       │   │   ├── index.astro
│       │   │   ├── halloween.astro
│       │   │   └── christmas.astro
│       │   ├── components/
│       │   │   ├── Header.astro
│       │   │   ├── Footer.astro
│       │   │   ├── Button.astro
│       │   │   └── ... (all components)
│       │   ├── layouts/
│       │   │   └── Layout.astro
│       │   └── data/
│       │       ├── products.json
│       │       ├── characters.json
│       │       └── christmas-characters.json
│       ├── public/
│       │   ├── images/
│       │   └── fonts/
│       ├── astro.config.mjs
│       ├── package.json
│       ├── tsconfig.json
│       └── .env
│
├── specs/                          # Project specifications
│   ├── 01-landing-page.md
│   ├── 02-halloween-page.md
│   ├── 03-halloween-dark-theme.md
│   ├── 04-christmas-page.md
│   ├── 05-i18n.md
│   └── 06-monorepo-i18n.md         # This document
│
├── package.json                    # Root workspace config
├── .gitignore
├── README.md
└── CLAUDE.md
```

### Key Architectural Decisions

#### 1. Complete App Independence
- Each app has its own `package.json` with dependencies
- Each app has its own `astro.config.mjs` configuration
- Each app has its own build output
- No shared code at build time (apps cannot import from each other)

#### 2. Simplified Components
**Components to remove:**
- `LanguageSwitcher.astro` - No longer needed
- `src/utils/i18n.ts` - No language detection/translation system

**Components to simplify:**
- `Header.astro` - Remove language detection, hardcode navigation
- `Footer.astro` - Remove language detection, hardcode links

#### 3. Data Management
Each app maintains its own data files:

**site-es/src/data/products.json:**
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

**site-en/src/data/products.json:**
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

#### 4. Environment Variables

**site-es/.env:**
```
PUBLIC_WHATSAPP_NUMBER=5215512345678
PUBLIC_WHATSAPP_MESSAGE=Hola! Me interesa la Lotería. ¿Tienen disponible?
```

**site-en/.env:**
```
PUBLIC_WHATSAPP_NUMBER=5215512345678
PUBLIC_WHATSAPP_MESSAGE=Hi! I'm interested in the Lotería. Is it available?
```

#### 5. Routing Simplification
Each app uses root-level routes (no language prefix):
- Spanish: `/`, `/halloween`, `/christmas`
- English: `/`, `/halloween`, `/christmas`

No redirect logic needed - each app starts at its own root.

---

## Implementation Plan

### Phase 1: Setup Monorepo Structure (30 minutes)

**1.1 Create Directory Structure**
```bash
mkdir -p apps/site-es
mkdir -p apps/site-en
```

**1.2 Configure Root Workspace**

Create/update root `package.json`:
```json
{
  "name": "juguemosjuntos-monorepo",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
    "apps/*"
  ],
  "scripts": {
    "dev:es": "npm run dev --workspace=site-es",
    "dev:en": "npm run dev --workspace=site-en",
    "dev:all": "npm run dev --workspaces",
    "build:es": "npm run build --workspace=site-es",
    "build:en": "npm run build --workspace=site-en",
    "build:all": "npm run build --workspaces",
    "preview:es": "npm run preview --workspace=site-es",
    "preview:en": "npm run preview --workspace=site-en"
  }
}
```

### Phase 2: Migrate Spanish App (45 minutes)

**2.1 Initialize site-es App**
```bash
cd apps/site-es
# Copy package.json, astro.config.mjs, tsconfig.json from root
```

**2.2 Copy Spanish Content**
- Copy `src/pages/es/*.astro` → `apps/site-es/src/pages/*.astro`
- Remove `/es/` prefix from routes
- Update root `index.astro` to remove redirect (just show homepage)

**2.3 Copy Components**
- Copy all from `src/components/` → `apps/site-es/src/components/`
- Remove `LanguageSwitcher.astro`
- Simplify `Header.astro`:
  - Remove `getLangFromUrl()` logic
  - Remove `useTranslations()` logic
  - Hardcode Spanish navigation labels
  - Update hrefs to use root paths (remove `/es/`)
- Simplify `Footer.astro` similarly

**2.4 Copy Data**
- Copy `src/data/` → `apps/site-es/src/data/`
- Update `products.json` to keep only Spanish data (remove nested `es` object)
- Update `characters.json` to keep only Spanish names

**2.5 Copy Assets**
- Copy `public/` → `apps/site-es/public/`

**2.6 Configure Build**
- Update `astro.config.mjs`:
  - Remove `i18n` configuration entirely
  - Set `site: 'https://juguemosjuntos.mx'` (or chosen Spanish domain)
  - Set `outDir: '../../dist/site-es'` for organized builds

**2.7 Environment**
- Create `apps/site-es/.env` with Spanish WhatsApp config

### Phase 3: Migrate English App (45 minutes)

**3.1 Initialize site-en App**
```bash
cd apps/site-en
# Copy package.json, astro.config.mjs, tsconfig.json from root
```

**3.2 Copy English Content**
- Copy `src/pages/en/*.astro` → `apps/site-en/src/pages/*.astro`
- Remove `/en/` prefix from routes
- Update root `index.astro` to remove redirect

**3.3 Copy Components**
- Copy all from `src/components/` → `apps/site-en/src/components/`
- Remove `LanguageSwitcher.astro`
- Simplify `Header.astro`:
  - Hardcode English navigation labels
  - Update hrefs to use root paths (remove `/en/`)
- Simplify `Footer.astro` similarly

**3.4 Copy Data**
- Copy `src/data/` → `apps/site-en/src/data/`
- Update `products.json` to keep only English data
- Update `characters.json` to keep only English names

**3.5 Copy Assets**
- Copy `public/` → `apps/site-en/public/`

**3.6 Configure Build**
- Update `astro.config.mjs`:
  - Remove `i18n` configuration
  - Set `site: 'https://juguemosjuntos.com'` (or chosen English domain)
  - Set `outDir: '../../dist/site-en'`

**3.7 Environment**
- Create `apps/site-en/.env` with English WhatsApp config

### Phase 4: Clean Up Root (15 minutes)

**4.1 Remove Old Structure**
```bash
rm -rf src/
rm -rf public/
rm astro.config.mjs
rm tsconfig.json
```

**4.2 Update Root Files**
- Keep: `package.json` (workspace config only)
- Keep: `.gitignore`
- Keep: `specs/`
- Keep: `README.md` (update with monorepo instructions)
- Keep: `CLAUDE.md` (update with new architecture)

**4.3 Update .gitignore**
```
# Build outputs
dist/
apps/*/dist/

# Dependencies
node_modules/
apps/*/node_modules/

# Environment
.env
apps/*/.env
.env.production

# Astro
.astro/
apps/*/.astro/
```

### Phase 5: Testing (20 minutes)

**5.1 Install Dependencies**
```bash
npm install
```

**5.2 Test Spanish App**
```bash
npm run dev:es
# Visit http://localhost:4321
# Verify all pages work
# Check no language switcher appears
# Verify navigation works
```

**5.3 Test English App**
```bash
npm run dev:en
# Visit http://localhost:4321
# Verify all pages work
# Check no language switcher appears
# Verify navigation works
```

**5.4 Test Builds**
```bash
npm run build:all
# Verify dist/site-es/ contains Spanish build
# Verify dist/site-en/ contains English build
# Check build outputs are independent
```

**5.5 Verify Isolation**
- Search codebase for remaining i18n imports
- Ensure no cross-references between apps
- Verify each app's data files are independent
- Check that section IDs are language-appropriate

### Phase 6: Documentation (15 minutes)

**6.1 Update README.md**
Document:
- Monorepo structure
- Development commands
- Building for production
- Deployment instructions

**6.2 Update CLAUDE.md**
Document:
- New architecture (two independent apps)
- How to make changes across both apps
- Data file locations
- Build configuration

---

## Deployment Strategy

### Domain Setup

**Option A: Country-Specific TLDs (Recommended)**
- Spanish: `juguemosjuntos.mx` or `juguemosjuntos.com.mx`
- English: `juguemosjuntos.com` or `juguemosjuntos.us`

**Option B: Single Domain with Subdomains**
- Spanish: `es.juguemosjuntos.com`
- English: `en.juguemosjuntos.com`

**Option C: Completely Different Brands**
- Spanish: `juguemosjuntos.mx`
- English: `loteriaparty.com`

### Hosting Options

#### Netlify (Recommended)
```yaml
# netlify.toml for site-es
[build]
  base = "apps/site-es"
  command = "npm run build"
  publish = "dist"

# Deploy to juguemosjuntos.mx
```

```yaml
# netlify.toml for site-en
[build]
  base = "apps/site-en"
  command = "npm run build"
  publish = "dist"

# Deploy to juguemosjuntos.com
```

**Setup:**
1. Create two Netlify sites (one per domain)
2. Connect both to same GitHub repo
3. Configure different base directories
4. Set environment variables per site
5. Configure custom domains

#### Vercel
Similar approach:
- Two Vercel projects
- Same GitHub repo
- Different root directories (`apps/site-es` vs `apps/site-en`)
- Separate environment variables
- Custom domains

### Deployment Workflow

**Automated (Recommended):**
```yaml
# .github/workflows/deploy.yml
name: Deploy Sites

on:
  push:
    branches: [main]

jobs:
  deploy-spanish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy site-es
        # Deploy to Spanish hosting

  deploy-english:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy site-en
        # Deploy to English hosting
```

**Manual:**
```bash
# Build Spanish site
npm run build:es
# Upload dist/site-es/ to Spanish hosting

# Build English site
npm run build:en
# Upload dist/site-en/ to English hosting
```

---

## Maintenance Workflows

### Scenario 1: Update Product Pricing

**Task:** Change Halloween product price

**Workflow:**
```bash
# Open VSCode at repo root
# Edit apps/site-es/src/data/products.json
# Edit apps/site-en/src/data/products.json
# Commit both changes together
git commit -m "Update Halloween pricing for both markets"
```

**With Claude Code:**
```
User: "Update Halloween price to 150 MXN for Spanish and 18 USD for English"
Claude: [Edits apps/site-es/src/data/products.json]
        [Edits apps/site-en/src/data/products.json]
        "Done! Updated Halloween pricing in both markets."
```

**Time:** ~2 minutes (2x the single-app time, but trivial)

---

### Scenario 2: Fix UI Bug in Button Component

**Task:** Button hover color is wrong

**Workflow:**
```bash
# Fix in apps/site-es/src/components/Button.astro
# Test it looks good
# Copy fix to apps/site-en/src/components/Button.astro
```

**With Claude Code:**
```
User: "Fix the button hover color to #FF5722 in both apps"
Claude: [Edits apps/site-es/src/components/Button.astro]
        [Edits apps/site-en/src/components/Button.astro]
        "Done! Updated button hover color in both Spanish and English sites."
```

**Time:** ~5 minutes (Claude can edit both in one response)

---

### Scenario 3: Add New Seasonal Product Page

**Task:** Create "Día de Muertos" product page (Spanish only)

**Workflow:**
```bash
# Create apps/site-es/src/pages/dia-de-muertos.astro
# Add product to apps/site-es/src/data/products.json
# Add images to apps/site-es/public/images/
# Update navigation in apps/site-es/src/components/Header.astro
# No changes needed to English site
```

**Time:** ~30 minutes (same as current, plus this product is Spanish-only)

---

### Scenario 4: Add New Feature to Both Sites

**Task:** Add testimonials section to homepage

**Workflow:**
```bash
# Create apps/site-es/src/components/TestimonialsSection.astro
# Add to apps/site-es/src/pages/index.astro
# Test Spanish version
# Copy component to apps/site-en/src/components/TestimonialsSection.astro
# Update English copy in component
# Add to apps/site-en/src/pages/index.astro
```

**With Claude Code:**
```
User: "Add a testimonials section to both homepages"
Claude: [Creates apps/site-es/src/components/TestimonialsSection.astro]
        [Creates apps/site-en/src/components/TestimonialsSection.astro]
        [Edits apps/site-es/src/pages/index.astro]
        [Edits apps/site-en/src/pages/index.astro]
        "Done! Added testimonials section to both sites."
```

**Time:** ~45 minutes (vs ~30 minutes for single-language approach)
**Overhead:** ~1.5x, but worth it for complete isolation

---

### Scenario 5: Major Redesign

**Task:** Redesign entire header navigation

**Workflow:**
```bash
# Redesign in site-es first (Spanish is primary market)
# Test thoroughly
# Copy new Header.astro to site-en
# Adapt English copy
# Test English version
```

**Time:** ~3 hours for Spanish + 1 hour for English = 4 hours total
**Overhead:** ~1.3x compared to doing both simultaneously
**Benefit:** Can ship Spanish first, then English when ready

---

## Migration Checklist

### Pre-Migration
- [ ] Backup current codebase
- [ ] Review current routes and ensure all pages are documented
- [ ] Verify all environment variables are documented
- [ ] Test current build process
- [ ] Take screenshots of current sites for comparison

### During Migration
- [ ] Create `apps/site-es/` structure
- [ ] Create `apps/site-en/` structure
- [ ] Configure root workspace
- [ ] Migrate Spanish content
- [ ] Migrate English content
- [ ] Remove LanguageSwitcher from both apps
- [ ] Simplify Header and Footer in both apps
- [ ] Update data files in both apps
- [ ] Configure build for both apps
- [ ] Set up environment variables
- [ ] Clean up root directory

### Testing
- [ ] `npm install` succeeds
- [ ] `npm run dev:es` works
- [ ] `npm run dev:en` works
- [ ] All Spanish pages load correctly
- [ ] All English pages load correctly
- [ ] No language switcher visible
- [ ] Navigation works in both apps
- [ ] Images load in both apps
- [ ] WhatsApp buttons work with correct messages
- [ ] `npm run build:all` succeeds
- [ ] Both builds produce valid output
- [ ] No cross-references between apps

### Post-Migration
- [ ] Update README.md
- [ ] Update CLAUDE.md
- [ ] Update deployment configuration
- [ ] Set up separate domains
- [ ] Deploy Spanish site
- [ ] Deploy English site
- [ ] Verify both sites are isolated
- [ ] Test purchasing flow on both sites
- [ ] Monitor for any issues

---

## Rationale and Trade-offs

### Why Monorepo Over Separate Repos?

**Development Experience:**
- ✅ Single VSCode window sees both apps
- ✅ Claude Code has full context of both sites in one session
- ✅ Easy to compare implementations side-by-side
- ✅ Single git repository simplifies version control

**Maintenance Efficiency:**
- ✅ Copy/paste between apps is trivial (just different directories)
- ✅ Can apply changes to both apps in single commit
- ✅ No need to switch repositories or contexts
- ✅ Shared specs and documentation

**Flexibility:**
- ✅ Apps can diverge without coordination
- ✅ Can ship updates independently
- ✅ Easy to add shared packages later if needed
- ✅ Can split into separate repos later if desired

### Why Independent Apps Over Shared Packages?

**Simplicity:**
- ✅ No package build step required
- ✅ No internal package versioning
- ✅ No dependency management between packages
- ✅ Easier to understand for future developers

**Independence:**
- ✅ Apps truly cannot import from each other (enforced by structure)
- ✅ No accidental coupling through shared code
- ✅ Complete freedom to diverge
- ✅ Updates to one app cannot break the other

**Future-Proofing:**
- ✅ Can add shared packages later when complexity justifies it
- ✅ Start simple, add complexity only when needed
- ✅ Current overhead (2x maintenance) is acceptable for content updates

### Accepted Trade-offs

**Code Duplication:**
- ⚠️ Components duplicated across apps (~1000 lines)
- ⚠️ Assets duplicated (~50MB of images)
- ✅ **Acceptable:** Code is stable, assets rarely change

**Update Overhead:**
- ⚠️ Content updates need to be applied twice
- ⚠️ Bug fixes need to be applied twice
- ✅ **Acceptable:** Claude can edit both in one response, only ~2x time

**Storage:**
- ⚠️ Larger repository (2x the size)
- ✅ **Acceptable:** Modern drives are cheap, git compression is good

---

## Success Metrics

### Technical Success
- [ ] Both apps build successfully independently
- [ ] No import errors or cross-references
- [ ] Clean separation of concerns
- [ ] Simple, maintainable codebase

### Business Success
- [ ] Complete isolation prevents cross-discovery
- [ ] Spanish users cannot easily find English version
- [ ] Separate domains reinforce market segmentation
- [ ] Different business models can operate independently

### Developer Experience
- [ ] Single VSCode window for all development
- [ ] Easy to make changes across both apps
- [ ] Clear mental model of architecture
- [ ] Fast iteration on content updates

---

## Future Considerations

### When to Add Shared Packages

Consider migrating to shared packages when:
- Adding 3+ language/market variations
- Component library becomes complex (20+ components)
- Making frequent UI updates across all apps
- Building a design system

### When to Split into Separate Repos

Consider separating when:
- Teams work on different markets independently
- Deployment cadences diverge significantly
- Security/access control requires separation
- Apps have completely different tech stacks

### Potential Enhancements

**Phase 2 Improvements:**
- Shared component library (if maintenance overhead becomes too high)
- Shared design tokens (colors, typography, spacing)
- Shared testing infrastructure
- Shared CI/CD templates

**Phase 3 Improvements:**
- Automated screenshot comparison between apps
- Shared storybook for components
- Internationalization testing suite
- A/B testing framework

---

## Conclusion

The simple monorepo approach with two independent apps (`site-es` and `site-en`) provides the optimal balance for this project's unique requirements:

1. **Solves the piracy concern** through complete isolation
2. **Maintains development efficiency** with single context
3. **Allows business models to diverge** naturally
4. **Keeps complexity low** while providing flexibility
5. **Scales appropriately** for a 2-site deployment

The estimated 3 hours of migration effort and ~1.5x maintenance overhead are justified by the complete market isolation and development experience improvements.

---

## References

- Current i18n implementation: `specs/05-i18n.md`
- Astro i18n routing: https://docs.astro.build/en/guides/internationalization/
- npm workspaces: https://docs.npmjs.com/cli/v7/using-npm/workspaces
- Monorepo best practices: https://monorepo.tools/

---

**End of Specification**
