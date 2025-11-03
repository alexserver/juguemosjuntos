# Christmas Lottery Page - Implementation Plan

Create a Christmas-themed lottery page similar to the Halloween page, with festive Christmas colors and themed assets.

---

## PHASE 1: ASSET REORGANIZATION

### Current Structure
```
src/assets/images/
  - characters/          # Halloween characters (01.png - 09.png)
  - misc/               # Halloween misc images (arte.png, disenio.png, diversion.png, memoria.png)
  - logos/              # Shared logos (loteria-halloween-logo.png, juguemos-juntos-logo.png)
```

### Target Structure
```
src/assets/images/
  - halloween/
      - characters/     # Move from src/assets/images/characters/
      - misc/          # Move from src/assets/images/misc/
  - christmas/
      - characters/     # New Christmas character images
      - misc/          # New Christmas misc images
  - logos/             # Keep as-is (shared across pages)
      - loteria-halloween-logo.png
      - loteria-christmas-logo.png  # NEW
      - juguemos-juntos-logo.png
```

### Asset Migration Steps

1. **Create new directory structure:**
   ```bash
   mkdir -p src/assets/images/halloween/characters
   mkdir -p src/assets/images/halloween/misc
   mkdir -p src/assets/images/christmas/characters
   mkdir -p src/assets/images/christmas/misc
   ```

2. **Move Halloween assets:**
   ```bash
   # Move character images
   mv src/assets/images/characters/*.png src/assets/images/halloween/characters/

   # Move misc images
   mv src/assets/images/misc/*.png src/assets/images/halloween/misc/

   # Remove empty directories
   rmdir src/assets/images/characters
   rmdir src/assets/images/misc
   ```

3. **Update import paths in halloween.astro:**
   - FROM: `import char01 from "../assets/images/characters/01.png";`
   - TO: `import char01 from "../assets/images/halloween/characters/01.png";`

   - FROM: `import arteImg from "../assets/images/misc/arte.png";`
   - TO: `import arteImg from "../assets/images/halloween/misc/arte.png";`

---

## PHASE 2: CHRISTMAS COLOR PALETTE

### Color Scheme
Add to `src/styles/global.css` @theme block:

```css
@theme {
  /* Existing colors remain unchanged */

  /* Christmas theme colors */
  --color-christmas-red: #c41e3a;          /* Classic Christmas red */
  --color-christmas-green: #165b33;        /* Deep forest green */
  --color-christmas-gold: #d4af37;         /* Festive gold */
  --color-christmas-cream: #fffef9;        /* Warm cream/ivory */
  --color-christmas-dark-red: #8b0000;     /* Dark burgundy red */
  --color-christmas-pine: #0f4c2a;         /* Dark pine green */
  --color-christmas-snow: #f8f9fa;         /* Snow white */
  --color-christmas-silver: #c0c0c0;       /* Silver accent */
  --color-christmas-light-gold: #ffd700;   /* Bright gold for accents */
}
```

### Color Usage Strategy
- **Backgrounds:** Gradients mixing deep greens, reds, and dark tones
- **Text:**
  - Primary: Christmas cream/snow (`christmas-cream`, `christmas-snow`)
  - Accents: Gold (`christmas-gold`, `christmas-light-gold`)
  - Headings: Red or gold with festive font
- **Cards:** Deep green/red with subtle transparency
- **Buttons:** Gold or red with hover effects

---

## PHASE 3: CHRISTMAS PAGE STRUCTURE

### Page Route
- **File:** `src/pages/christmas.astro`
- **URL:** `/christmas`

### Components to Reuse
All components from Halloween page can be reused with `variant="dark"` prop:
- `Layout.astro`
- `Header.astro`
- `Footer.astro`
- `Button.astro`
- `FeatureList.astro`
- `ValueCard.astro` (with `variant="dark"`)
- `TestimonialCard.astro` (with `variant="dark"`)

### Character Data
- **File:** `src/data/christmas-characters.json`
- **Format:** Same as Halloween characters
  ```json
  [
    { "id": 1, "name": "Santa Claus", "filename": "01.png" },
    { "id": 2, "name": "El Reno", "filename": "02.png" },
    { "id": 3, "name": "El Muneco de Nieve", "filename": "03.png" },
    { "id": 4, "name": "El Elfo", "filename": "04.png" },
    { "id": 5, "name": "El Angel", "filename": "05.png" },
    { "id": 6, "name": "El Arbol de Navidad", "filename": "06.png" },
    { "id": 7, "name": "La Campana", "filename": "07.png" },
    { "id": 8, "name": "El Regalo", "filename": "08.png" },
    { "id": 9, "name": "La Estrella", "filename": "09.png" }
  ]
  ```

### Page Sections (mirroring Halloween structure)

#### 1. Hero Section
```astro
<section class="bg-linear-to-br from-christmas-pine via-christmas-dark-red to-christmas-green py-20 md:py-32">
```
- **Title:** "Loteria de Navidad!" with festive emoji (tree)
- **Logo:** Christmas lottery logo
- **Button:** "Comprala ahora" (gold variant)
- **Text colors:** `text-christmas-gold` for title, `text-christmas-cream` for subtitle

#### 2. "Que Incluye" Section
```astro
<section class="py-20 bg-christmas-pine">
```
- **Heading:** `text-christmas-light-gold`
- **Features:**
  - 24 personajes unicos navidenos
  - 12 cartillas de juego
  - Version impresa y ecologica
- **Text:** `text-christmas-cream`

#### 3. "Por Que Te Encantara" Section
```astro
<section class="py-20 bg-linear-to-br from-christmas-dark-red/60 to-christmas-pine">
```
- **Heading:** `text-christmas-gold`
- **ValueCard images:** Christmas-themed (need 4 images in `christmas/misc/`)
  - `diversion.png` - Family fun
  - `disenio.png` - Festive designs
  - `arte.png` - Exclusive art
  - `memoria.png` - Educational
- **Cards:** Pass `variant="dark"`

#### 4. "Galeria de Personajes" Section
```astro
<section class="py-20 bg-linear-to-b from-christmas-green/80 to-christmas-pine">
```
- **Heading:** `text-christmas-light-gold`
- **Character cards:** Display Christmas characters with gold borders
- **Card labels:** `bg-christmas-cream`

#### 5. "Como Jugar" Section
```astro
<section class="py-20 bg-linear-to-br from-christmas-pine via-christmas-dark-red to-christmas-green text-white">
```
- **Heading:** `text-christmas-gold`
- **Steps:** Same as Halloween (4 numbered steps)
- **Step circles:** `bg-christmas-red` or `bg-christmas-gold`

#### 6. "Testimonios" Section
```astro
<section class="py-20 bg-linear-to-br from-christmas-dark-red/70 to-christmas-pine">
```
- **Heading:** `text-christmas-light-gold`
- **TestimonialCard:** Pass `variant="dark"`
- **Sample quotes:**
  - "Perfecta para nuestra celebracion navidena, a los ninos les encanto."
  - "Un regalo ideal para compartir en familia durante las fiestas."

#### 7. "CTA Final" Section
```astro
<section class="py-20 bg-linear-to-br from-christmas-pine via-christmas-dark-red to-christmas-gold/20">
```
- **Inner card:** `bg-christmas-dark-red/80 border-2 border-christmas-gold/50`
- **Heading:** `text-christmas-gold`
- **Title:** "Consiguela ahora y celebra la Navidad en familia!"
- **Text:** `text-christmas-cream`
- **Button:** Gold/red variant

---

## PHASE 4: ASSET REQUIREMENTS

### Christmas Character Images
**Location:** `src/assets/images/christmas/characters/`

Need 9 character images (01.png - 09.png):
1. Santa Claus (Papa Noel)
2. Reindeer (Reno)
3. Snowman (Muneco de Nieve)
4. Elf (Elfo)
5. Angel (Angel)
6. Christmas Tree (Arbol de Navidad)
7. Bell (Campana)
8. Present/Gift (Regalo)
9. Star (Estrella)

**Characteristics:**
- Cute, non-scary style (family-friendly)
- Colorful and vibrant
- Consistent art style
- Square/portrait orientation
- High resolution (at least 500x500px)

### Christmas Misc Images
**Location:** `src/assets/images/christmas/misc/`

Need 4 value card images:
1. `diversion.png` - Family fun illustration
2. `disenio.png` - Design/festive decorations
3. `arte.png` - Art/creativity themed
4. `memoria.png` - Memory/education themed

**Characteristics:**
- Christmas-themed icons or illustrations
- Consistent with character art style
- Simple and recognizable
- Good contrast on dark backgrounds

### Christmas Logo
**Location:** `src/assets/images/logos/`

Need 1 logo image:
- `loteria-christmas-logo.png`

**Characteristics:**
- Christmas color scheme (red, green, gold)
- "Loteria de Navidad" text
- Festive design elements
- Same dimensions as Halloween logo for consistency

---

## PHASE 5: ENVIRONMENT VARIABLES

### WhatsApp Configuration
Add to `.env` file:

```env
# Existing
PUBLIC_WHATSAPP_NUMBER=your_number_here
PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN=Hola! Me interesa la Loteria de Halloween

# NEW
PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS=Hola! Me interesa la Loteria de Navidad
```

### Usage in christmas.astro
```javascript
const whatsappNumber = import.meta.env.PUBLIC_WHATSAPP_NUMBER;
const whatsappMessage = import.meta.env.PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS;
const whatsappLink = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(whatsappMessage)}`;
```

---

## PHASE 6: NAVIGATION UPDATES

### Header Component
Update `src/components/Header.astro` to include Christmas link:

```astro
<nav>
  <a href="/">Inicio</a>
  <a href="/halloween">Halloween</a>
  <a href="/christmas">Navidad</a> <!-- NEW -->
</nav>
```

### Home Page - Games Section
Update `src/pages/index.astro` in the `<!-- Games Section -->` (around line 88):

**Current Structure:** Single game card centered
**New Structure:** Grid with two game cards side by side

#### Changes to Make:

1. **Update container max-width:**
   - FROM: `max-w-4xl`
   - TO: `max-w-6xl`

2. **Add grid layout:**
   ```astro
   <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
   ```

3. **Halloween Game Card (existing):**
   - Keep existing card but update styling
   - Add hover effect: `hover:shadow-2xl transition-shadow duration-300`
   - Unhide the title: Remove `hidden` class from h3
   - Adjust heading size: `text-2xl md:text-3xl`
   - Adjust description size: `text-base md:text-lg`

4. **Christmas Game Card (NEW):**
   ```astro
   <div class="bg-linear-to-br from-brand-purple/10 to-brand-turquoise/10 rounded-3xl p-8 md:p-12 shadow-xl hover:shadow-2xl transition-shadow duration-300">
     <div class="text-center">
       <div class="flex justify-center mb-6">
         <div class="h-32 md:h-40 flex items-center justify-center">
           <span class="text-6xl md:text-7xl">🎄</span>
         </div>
       </div>
       <h3 class="text-2xl md:text-3xl font-bold text-brand-purple mb-4">
         Loteria de Navidad
       </h3>
       <p class="text-base md:text-lg text-gray-700 mb-8">
         Celebra la Navidad en familia con esta version festiva del
         clasico juego mexicano, con personajes navidenos encantadores.
       </p>
       <Button variant="secondary" href="/christmas">Ver mas</Button>
     </div>
   </div>
   ```

**Note:** Christmas card uses a tree emoji (🎄) as placeholder until the Christmas logo is created. Once `loteria-christmas-logo.png` is available, import and use it like the Halloween logo.

---

## IMPLEMENTATION CHECKLIST

### Asset Organization
- [ ] Create `src/assets/images/halloween/characters/` directory
- [ ] Create `src/assets/images/halloween/misc/` directory
- [ ] Create `src/assets/images/christmas/characters/` directory
- [ ] Create `src/assets/images/christmas/misc/` directory
- [ ] Move Halloween character images to new location
- [ ] Move Halloween misc images to new location
- [ ] Update all import paths in `halloween.astro`
- [ ] Test Halloween page to ensure images still load

### Christmas Theme Setup
- [ ] Add Christmas color palette to `src/styles/global.css`
- [ ] Create `src/data/christmas-characters.json`
- [ ] Add Christmas WhatsApp message to `.env`

### Christmas Assets
- [ ] Create/obtain 9 Christmas character images
- [ ] Create/obtain 4 Christmas misc images (value cards)
- [ ] Create/obtain Christmas logo
- [ ] Place all assets in appropriate directories

### Christmas Page Development
- [ ] Create `src/pages/christmas.astro`
- [ ] Implement Hero section with Christmas theme
- [ ] Implement "Que Incluye" section
- [ ] Implement "Por Que Te Encantara" section
- [ ] Implement "Galeria de Personajes" section
- [ ] Implement "Como Jugar" section
- [ ] Implement "Testimonios" section
- [ ] Implement "CTA Final" section
- [ ] Configure WhatsApp integration

### Navigation & Integration
- [ ] Update Header component with Christmas link
- [ ] Update home page to showcase Christmas game
- [ ] Test all navigation links

### Testing & QA
- [ ] Test Christmas page on desktop
- [ ] Test Christmas page on mobile
- [ ] Verify all images load correctly
- [ ] Test WhatsApp button functionality
- [ ] Check color contrast and accessibility
- [ ] Verify responsive layout
- [ ] Test on different browsers
- [ ] Ensure Halloween page still works correctly
- [ ] Ensure home page still works correctly

---

## ACCESSIBILITY CONSIDERATIONS

### Color Contrast Ratios (WCAG AA: 4.5:1 minimum)
- Christmas cream (#fffef9) on Christmas pine (#0f4c2a) = approx 14:1 (Excellent)
- Christmas gold (#d4af37) on Christmas pine (#0f4c2a) = approx 6:1 (Good)
- Christmas snow on Christmas dark red = approx 12:1 (Excellent)

### Best Practices
- Use semantic HTML
- Include alt text for all images
- Ensure keyboard navigation works
- Maintain focus indicators
- Test with screen readers

---

## OPTIONAL ENHANCEMENTS

### Visual Effects
```css
/* Gold shimmer effect */
.gold-shimmer {
  text-shadow: 0 0 20px rgba(212, 175, 55, 0.5);
}

/* Card glow effect */
.christmas-glow {
  box-shadow: 0 0 30px rgba(212, 175, 55, 0.3);
}

/* Snow effect animation (optional) */
.snowfall {
  /* Add CSS animation for falling snow */
}
```

### Interactive Features
- Add festive animations on hover
- Include subtle background patterns (snowflakes, stars)
- Add smooth scroll animations between sections
- Include festive sound effects (optional, muted by default)

---

## TIMELINE ESTIMATE

1. **Asset Reorganization:** 1-2 hours
   - Moving files
   - Updating import paths
   - Testing

2. **Christmas Theme Setup:** 1 hour
   - Adding colors to CSS
   - Creating character data JSON
   - Environment variables

3. **Asset Creation/Acquisition:** Variable (depends on design resources)
   - If creating from scratch: 8-16 hours
   - If using stock/purchased: 2-4 hours
   - If AI-generated: 4-8 hours

4. **Christmas Page Development:** 4-6 hours
   - Page structure
   - Component integration
   - Styling and theming
   - WhatsApp integration

5. **Navigation & Integration:** 1-2 hours
   - Header updates
   - Home page updates

6. **Testing & QA:** 2-3 hours
   - Cross-browser testing
   - Responsive testing
   - Accessibility testing

**Total Estimated Time:** 19-32 hours (excluding asset creation)

---

## NOTES

- Keep the same playful, family-friendly tone as Halloween page
- Christmas theme should feel warm, festive, and inviting
- Maintain consistency with existing brand colors where appropriate
- Ensure all pages (home, Halloween, Christmas) work independently
- Consider seasonal availability (Christmas page could be featured more prominently during November-December)
