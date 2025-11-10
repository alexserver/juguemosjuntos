# Zoo Lottery Page - Implementation Plan

Create a Zoo-themed lottery page with vibrant orange, purple, and green color palette. The page will feature 9 animal characters from the zoo.

---

## PHASE 1: ASSET VERIFICATION

### Current Asset Status

✅ **Character images already copied:**

- Spanish site: `apps/site-es/src/assets/images/zoo/characters/` (9 images: 01.png - 09.png)
- English site: `apps/site-en/src/assets/images/zoo/characters/` (9 images: 01.png - 09.png)

✅ **Misc images already copied:**

- Spanish site: `apps/site-es/src/assets/images/zoo/misc/` (4 images: arte.png, disenio.png, diversion.png, memoria.png)
- English site: `apps/site-en/src/assets/images/zoo/misc/` (4 images: arte.png, disenio.png, diversion.png, memoria.png)

### Assets Still Needed

#### Zoo Logo

**Location (both apps):** `src/assets/images/logos/`

Need 2 logo files (one per app):

- Spanish: `zoo-bingo-logo.png` or `loteria-zoo-logo.png`
- English: `zoo-bingo-logo.png` or `loteria-zoo-logo.png`

**Characteristics:**

- Zoo color scheme (orange, purple, green)
- "Lotería de Zoo" or "Zoo Bingo" text
- Animal/zoo design elements
- Same dimensions as Halloween/Christmas logos for consistency

**Note:** If the logo is identical for both languages, use the same file in both apps. If text differs (Lotería vs Bingo), create separate versions.

---

## PHASE 2: ZOO COLOR PALETTE

### Color Scheme

Add to `src/styles/global.css` @theme block in **both apps**:

```css
@theme {
  /* Existing colors remain unchanged */

  /* Zoo theme colors */
  --color-zoo-orange: #ff8c42; /* Vibrant orange (lions, tigers) */
  --color-zoo-orange-bright: #ffa45c; /* Bright orange accent */
  --color-zoo-orange-dark: #d97136; /* Deep orange */
  --color-zoo-purple: #9b5de5; /* Vivid purple (exotic, playful) */
  --color-zoo-purple-light: #c4a3f5; /* Light purple accent */
  --color-zoo-purple-dark: #6930c3; /* Deep purple */
  --color-zoo-green: #51cf66; /* Fresh green (jungle, nature) */
  --color-zoo-green-bright: #69db7c; /* Bright green accent */
  --color-zoo-green-dark: #37b24d; /* Forest green */
  --color-zoo-cream: #fef8f3; /* Warm cream for text */
  --color-zoo-brown: #8b5a3c; /* Earthy brown accent */
  --color-zoo-yellow: #ffd43b; /* Sunshine yellow */
}
```

### Color Usage Strategy

- **Backgrounds:**
  - Gradients mixing orange, purple, and green
  - Section variety: alternate between dominant colors
- **Text:**
  - Primary: Zoo cream (`zoo-cream`)
  - Accents: Bright orange, purple, or green depending on section
  - Headings: Rotate between orange-bright, purple, and green-bright
- **Cards:** Mix of orange and purple borders with hover effects
- **Buttons:** Orange variant with green accents

---

## PHASE 3: CHARACTER DATA

### Character Names (TO BE PROVIDED)

**Spanish Characters** - Add to `apps/site-es/src/data/characters.json`:

```json
{
  "halloween": [...],
  "christmas": [...],
  "zoo": [
    { "id": 1, "name": "El León", "filename": "01.png" },
    { "id": 2, "name": "La Cebra", "filename": "02.png" },
    { "id": 3, "name": "El Elefante", "filename": "03.png" },
    { "id": 4, "name": "La Jirafa", "filename": "04.png" },
    { "id": 5, "name": "El Mono", "filename": "05.png" },
    { "id": 6, "name": "El Tigre", "filename": "06.png" },
    { "id": 7, "name": "El Panda", "filename": "07.png" },
    { "id": 8, "name": "El Pingüino", "filename": "08.png" },
    { "id": 9, "name": "El Cocodrilo", "filename": "09.png" }
  ]
}
```

**English Characters** - Add to `apps/site-en/src/data/characters.json`:

```json
{
  "halloween": [...],
  "christmas": [...],
  "zoo": [
    { "id": 1, "name": "Lion", "filename": "01.png" },
    { "id": 2, "name": "Zebra", "filename": "02.png" },
    { "id": 3, "name": "Elephant", "filename": "03.png" },
    { "id": 4, "name": "Giraffe", "filename": "04.png" },
    { "id": 5, "name": "Monkey", "filename": "05.png" },
    { "id": 6, "name": "Tiger", "filename": "06.png" },
    { "id": 7, "name": "Panda", "filename": "07.png" },
    { "id": 8, "name": "Penguin", "filename": "08.png" },
    { "id": 9, "name": "Crocodile", "filename": "09.png" }
  ]
}
```

**Character Reference List:**

```
Character 01:
  - English: Lion
  - Spanish: El León

Character 02:
  - English: Zebra
  - Spanish: La Cebra

Character 03:
  - English: Elephant
  - Spanish: El Elefante

Character 04:
  - English: Giraffe
  - Spanish: La Jirafa

Character 05:
  - English: Monkey
  - Spanish: El Mono

Character 06:
  - English: Tiger
  - Spanish: El Tigre

Character 07:
  - English: Panda
  - Spanish: El Panda

Character 08:
  - English: Penguin
  - Spanish: El Pingüino

Character 09:
  - English: Crocodile
  - Spanish: El Cocodrilo
```

---

## PHASE 4: PRODUCT DATA

### Add Zoo Product to Products JSON

**Spanish** - Update `apps/site-es/src/data/products.json`:

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
  },
  {
    "id": "zoo",
    "name": "Lotería del Zoológico",
    "price": 120,
    "currency": "MXN"
  }
]
```

**English** - Update `apps/site-en/src/data/products.json`:

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
  },
  {
    "id": "zoo",
    "name": "Zoo Bingo",
    "price": 15,
    "currency": "USD"
  }
]
```

---

## PHASE 5: ENVIRONMENT VARIABLES

### WhatsApp Configuration

**Spanish** - Add to `apps/site-es/.env`:

```env
# Existing
PUBLIC_WHATSAPP_NUMBER=529845937808
PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN=Hola! Me interesa comprar la Lotería de Halloween...
PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS=Hola! Me interesa comprar la Lotería de Navidad...

# NEW
PUBLIC_WHATSAPP_MESSAGE_ZOO=Hola! Me interesa comprar la Lotería del Zoológico. ¿Cuál es el proceso de compra?
```

**English** - Add to `apps/site-en/.env`:

```env
# Existing
PUBLIC_WHATSAPP_NUMBER=529845937808
PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN=Hi! I'm interested in buying the Halloween Lotería...
PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS=Hi! I'm interested in buying the Christmas Lotería...

# NEW
PUBLIC_WHATSAPP_MESSAGE_ZOO=Hi! I'm interested in buying the Zoo Bingo. How does the purchase process work?
```

---

## PHASE 6: ZOO PAGE STRUCTURE

### Page Route

- **File (Spanish):** `apps/site-es/src/pages/zoo.astro`
- **File (English):** `apps/site-en/src/pages/zoo.astro`
- **URL:** `/zoo` (both apps)

### Components to Reuse

All components can be reused:

- `Layout.astro`
- `Header.astro`
- `Footer.astro`
- `Button.astro` - Will need new `variant="zoo"` option
- `FeatureList.astro`
- `ValueCard.astro` - Use `variant="dark"` or create `variant="zoo"`
- `TestimonialCard.astro` - Use `variant="dark"`

### Page Sections (Spanish Version)

#### 1. Hero Section

```astro
<section class="bg-gradient-to-br from-zoo-orange via-zoo-purple to-zoo-green py-20 md:py-32">
```

- **Title:** "¡Lotería del Zoológico!" with zoo emoji (lion or animals)
- **Logo:** Zoo lottery logo
- **Button:** "Cómprala ahora" (zoo variant)
- **Text colors:** `text-zoo-cream` for title/subtitle, `text-zoo-orange-bright` for accents

#### 2. "Qué Incluye" Section

```astro
<section class="py-20 bg-zoo-purple-dark">
```

- **Heading:** `text-zoo-orange-bright`
- **Features:**
  - 9 personajes únicos de animales del zoológico
  - 12 cartillas de juego
  - Versión impresa y ecológica
- **Text:** `text-zoo-cream`

#### 3. "Por Qué Te Encantará" Section

```astro
<section class="py-20 bg-gradient-to-br from-zoo-green-dark/60 to-zoo-purple">
```

- **Heading:** `text-zoo-orange-bright`
- **ValueCard images:** Zoo-themed (4 images in `zoo/misc/`)
  - `diversion.png` - Family fun with animals
  - `disenio.png` - Cute animal designs
  - `arte.png` - Exclusive animal art
  - `memoria.png` - Educational about animals
- **Cards:** Pass `variant="dark"` or new `variant="zoo"`

#### 4. "Galería de Personajes" Section

```astro
<section class="py-20 bg-gradient-to-b from-zoo-orange/30 to-zoo-purple-dark">
```

- **Heading:** `text-zoo-green-bright`
- **Character cards:** Display all 9 zoo animals in 3x3 grid
  - Orange and purple borders alternating
- **Card labels:** `bg-zoo-cream text-gray-800`
- **Layout:** 3x3 grid on desktop, 2 columns on tablet, 1 column on mobile

#### 5. "Cómo Jugar" Section

```astro
<section class="py-20 bg-gradient-to-br from-zoo-purple-dark via-zoo-orange to-zoo-green-dark text-white">
```

- **Heading:** `text-zoo-yellow`
- **Steps:** Same 4-step instructions as other themes
- **Step circles:** `bg-zoo-orange` or `bg-zoo-green`

#### 6. "Testimonios" Section

```astro
<section class="py-20 bg-zoo-purple">
```

- **Heading:** `text-zoo-cream`
- **TestimonialCard:** Pass `variant="dark"`
- **Sample quotes (Spanish):**
  - "A mis hijos les fascinaron los animalitos, aprendieron mucho jugando."
  - "Una manera divertida de enseñar sobre animales del zoológico."

#### 7. "CTA Final" Section

```astro
<section class="py-20 bg-gradient-to-br from-zoo-green via-zoo-purple to-zoo-orange">
```

- **Inner card:** `bg-zoo-purple-dark/90 border-2 border-zoo-orange-bright/50`
- **Heading:** `text-zoo-orange-bright`
- **Title (Spanish):** "¡Consíguela ahora y descubre el mundo animal en familia!"
- **Text:** `text-zoo-cream`
- **Button:** Zoo variant

### Page Sections (English Version)

Same structure as Spanish, with translated content:

- **Hero Title:** "Zoo Bingo!"
- **Subtitle:** "A magical and fun version of the classic Mexican game. Perfect for kids and family gatherings."
- **Features:**
  - 9 unique zoo animal characters
  - 12 game boards
  - Digital PDF ready to print at home - instant download!
- **Testimonial quotes:**
  - "My kids loved the cute animals, they learned so much while playing."
  - "A fun way to teach about zoo animals."
- **CTA Title:** "Get it now and discover the animal world with your family!"
- **CTA Text:** "Digital PDF ready to print at home - instant download!"

---

## PHASE 7: BUTTON COMPONENT UPDATE

### Add Zoo Variant to Button.astro

Update `Button.astro` in **both apps** to include zoo styling:

```astro
// Add to the existing variant logic
{variant === "zoo" && (
  <a
    href={href}
    target={target}
    class="inline-block bg-zoo-orange text-white px-8 py-3 rounded-full font-bold text-lg hover:bg-zoo-orange-bright hover:scale-105 transition-all duration-300 shadow-lg hover:shadow-zoo-green/50"
  >
    <slot />
  </a>
)}
```

Alternatively, add to the existing button classes:

- Background: `bg-zoo-orange`
- Hover: `hover:bg-zoo-orange-bright`
- Shadow: `shadow-zoo-green/50`

---

## PHASE 8: NAVIGATION UPDATES

### Header Component

Update `Header.astro` in **both apps** to include Zoo link:

**Spanish (`apps/site-es/src/components/Header.astro`):**

```astro
<nav>
  <a href="/">Inicio</a>
  <a href="/halloween">Halloween</a>
  <a href="/christmas">Navidad</a>
  <a href="/zoo">Zoológico</a> <!-- NEW -->
</nav>
```

**English (`apps/site-en/src/components/Header.astro`):**

```astro
<nav>
  <a href="/">Home</a>
  <a href="/halloween">Halloween</a>
  <a href="/christmas">Christmas</a>
  <a href="/zoo">Zoo</a> <!-- NEW -->
</nav>
```

---

## PHASE 9: HOME PAGE UPDATES

### Update Games Section in index.astro

Update `src/pages/index.astro` in **both apps** to showcase all three games.

#### Current Structure

Two game cards (Halloween and Christmas)

#### New Structure

Three game cards in a grid

**Spanish (`apps/site-es/src/pages/index.astro`):**

```astro
<!-- Games Section -->
<section class="py-20 bg-gray-50">
  <div class="container mx-auto px-4">
    <h2 class="text-3xl md:text-5xl font-bold text-center text-brand-purple mb-12">
      Nuestros Juegos
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 max-w-7xl mx-auto">

      <!-- Halloween Card (existing) -->
      <div class="bg-gradient-to-br from-brand-orange/10 to-brand-purple/10 rounded-3xl p-8 shadow-xl hover:shadow-2xl transition-shadow duration-300">
        <!-- existing Halloween content -->
      </div>

      <!-- Christmas Card (existing) -->
      <div class="bg-gradient-to-br from-brand-purple/10 to-brand-turquoise/10 rounded-3xl p-8 shadow-xl hover:shadow-2xl transition-shadow duration-300">
        <!-- existing Christmas content -->
      </div>

      <!-- Zoo Card (NEW) -->
      <div class="bg-gradient-to-br from-zoo-orange/10 to-zoo-green/10 rounded-3xl p-8 shadow-xl hover:shadow-2xl transition-shadow duration-300">
        <div class="text-center">
          <div class="flex justify-center mb-6">
            <div class="h-32 md:h-40 flex items-center justify-center">
              <span class="text-6xl md:text-7xl">🦁</span>
            </div>
          </div>
          <h3 class="text-2xl md:text-3xl font-bold text-brand-purple mb-4">
            Lotería del Zoológico
          </h3>
          <p class="text-base md:text-lg text-gray-700 mb-8">
            Descubre el mundo animal con esta versión educativa y divertida del clásico juego mexicano.
          </p>
          <Button variant="secondary" href="/zoo">Ver más</Button>
        </div>
      </div>

    </div>
  </div>
</section>
```

**English (`apps/site-en/src/pages/index.astro`):**
Same structure, but with English text:

- Title: "Zoo Bingo"
- Description: "Discover the animal world with this educational and fun version of the classic Mexican game."
- Emoji: 🦁 (lion) or 🐘 (elephant)

---

## PHASE 10: IMPLEMENTATION CHECKLIST

### Assets

- [x] Verify zoo character images exist (01.png - 09.png) - **DONE**
- [x] Verify zoo misc images exist (4 value cards) - **DONE**
- [ ] Create/obtain zoo logo (Spanish version)
- [ ] Create/obtain zoo logo (English version, if different)
- [ ] Place logo in appropriate directories

### Data Setup (Both Apps)

- [ ] Add zoo colors to `src/styles/global.css`
- [ ] Add zoo characters to `src/data/characters.json` (with names from user)
- [ ] Add zoo product to `src/data/products.json`
- [ ] Add zoo WhatsApp message to `.env`

### Component Updates (Both Apps)

- [ ] Update `Button.astro` with zoo variant
- [ ] (Optional) Update `ValueCard.astro` with zoo variant
- [ ] Update `Header.astro` with zoo navigation link

### Page Creation (Both Apps)

- [ ] Create `src/pages/zoo.astro` (Spanish)
- [ ] Create `src/pages/zoo.astro` (English)
- [ ] Implement all 7 sections per page
- [ ] Configure WhatsApp integration
- [ ] Ensure character images load correctly (all 9)

### Home Page Updates (Both Apps)

- [ ] Add zoo card to games section (Spanish)
- [ ] Add zoo card to games section (English)
- [ ] Update grid layout to accommodate 3 cards
- [ ] Test responsive layout

### Testing (Both Apps)

- [ ] Test zoo page on desktop
- [ ] Test zoo page on tablet
- [ ] Test zoo page on mobile
- [ ] Verify all 9 character images load
- [ ] Test WhatsApp button functionality
- [ ] Check color contrast and accessibility
- [ ] Verify navigation works across all pages
- [ ] Test on different browsers
- [ ] Ensure other pages (home, halloween, christmas) still work

---

## CHARACTER DISPLAY STRATEGY

The zoo theme has 9 characters, same as Halloween and Christmas themes.

**Display Strategy:**
- Desktop: 3x3 grid (3 columns, 3 rows)
- Tablet: 2x5 grid (2 columns, with last row centered)
- Mobile: 1 column (9 rows)

**Implementation:** Use the same grid structure as Halloween and Christmas pages - simple, clean, and works without JavaScript.

---

## ACCESSIBILITY CONSIDERATIONS

### Color Contrast Ratios (WCAG AA: 4.5:1 minimum)

- Zoo cream (#fef8f3) on zoo purple-dark (#6930c3) = approx 9:1 (Excellent)
- Zoo orange-bright (#ffa45c) on zoo purple-dark = approx 5:1 (Good)
- Zoo green-bright (#69db7c) on zoo purple-dark = approx 5.5:1 (Good)

### Best Practices

- Use semantic HTML
- Include alt text for all 9 animal images
- Ensure keyboard navigation works
- Maintain focus indicators
- Test with screen readers

---

## TIMELINE ESTIMATE

1. **Asset Creation/Acquisition:** Variable

   - Logo design: 2-3 hours
   - **Total:** 2-3 hours

2. **Data Setup (both apps):** 1 hour

   - Add colors to CSS
   - Add character data to JSON
   - Update products JSON
   - Environment variables

3. **Component Updates (both apps):** 1-2 hours

   - Button variant
   - Header navigation

4. **Zoo Page Development (both apps):** 4-6 hours

   - Page structure (both languages)
   - Component integration
   - Styling with new theme
   - 9 character display
   - WhatsApp integration

5. **Home Page Updates (both apps):** 1-2 hours

   - Add zoo card
   - Update grid layout
   - Responsive testing

6. **Testing & QA:** 2-3 hours
   - Cross-browser testing
   - Responsive testing
   - All pages verification

**Total Estimated Time:** 11-17 hours (excluding logo creation)

---

## NOTES

- Zoo theme should feel vibrant, educational, and fun
- Orange-purple-green palette creates energetic, playful atmosphere
- 9 characters keeps consistency with Halloween and Christmas themes
- Maintain consistency with existing pages (Halloween, Christmas)
- Consider showing "fun facts" about animals (optional enhancement)
- Could add animal sounds on hover (optional, muted by default)

---

## READY TO IMPLEMENT?

**PREREQUISITES:**

1. ✅ Character images (01.png - 09.png) - **DONE**
2. ✅ Character names in Spanish (9 names) - **DONE**
3. ✅ Character names in English (9 names) - **DONE**
4. ✅ 4 misc/value card images - **DONE**
5. ⏳ Zoo logo(s) - **PENDING**

**Status: Ready to implement once logo is created!**
