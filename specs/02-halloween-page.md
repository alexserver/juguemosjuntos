Create a responsive inner landing page for a product called "Lotería de Halloween", part of the "Juguemos Juntos" brand.

The design should be playful, colorful, and Halloween-themed — use orange, black, purple, and green tones. Keep illustrations simple, friendly, and suitable for printing (no gradients or complex textures).

Do not include header or footer — those are shared across all pages.

---

## IMPLEMENTATION PLAN

### Route & File Structure

**Page Location:** `src/pages/halloween.astro`
- Route will be accessible at `/halloween`
- Uses existing `Layout.astro` component
- Header and Footer are already included in the layout

### Color Palette Strategy

**✅ RECOMMENDED: Extend Global Theme**

Add Halloween-specific colors to `src/styles/global.css` within the existing `@theme` block:

```css
@theme {
  /* Existing brand colors */
  --color-brand-orange: #ff6b35;
  --color-brand-yellow: #f7b801;
  --color-brand-purple: #8b5fbf;
  --color-brand-turquoise: #00cfc1;

  /* Halloween theme colors (add these) */
  --color-halloween-green: #4ade80;     /* Bright green for Halloween */
  --color-halloween-black: #1a1a1a;     /* Dark black for contrast */
  --color-halloween-dark: #2d1b4e;      /* Deep purple-black */
}
```

**Why extend global theme:**
- ✅ Tailwind v4 theme variables are global by design
- ✅ Can reuse Halloween colors across other pages/components if needed
- ✅ Simpler to maintain than scoped themes
- ✅ Existing brand colors (orange, purple) can be reused for Halloween theme
- ✅ No conflicts with current color system

**Color Usage:**
- Orange: Use existing `brand-orange`
- Black: Use new `halloween-black`
- Purple: Use existing `brand-purple`
- Green: Use new `halloween-green`

### Component Structure

**Reusable Components** (create in `src/components/`):
1. **FeatureList.astro** - List item with icon for "Qué incluye" section
2. **HighlightCard.astro** - Similar to ValueCard but styled for Halloween theme (can reuse existing ValueCard)
3. **TestimonialCard.astro** - Card for customer testimonials with quote styling
4. **StepCard.astro** - Numbered step card for "Cómo jugar" instructions

**Note:** The existing `Button.astro` and `ValueCard.astro` components can be reused.

### Implementation Order

1. **Setup** (first)
   - Add Halloween color palette to `src/styles/global.css`
   - Ensure colors generate proper utilities

2. **New Components**
   - `src/components/FeatureList.astro` - For "Qué incluye" items
   - `src/components/TestimonialCard.astro` - For testimonial quotes
   - `src/components/StepCard.astro` - For numbered game instructions

3. **Page Creation** (`src/pages/halloween.astro`)
   - Hero section with product image
   - "Qué incluye" section (using FeatureList components)
   - "Por qué te encantará" section (using existing ValueCard or new HighlightCard)
   - Gallery section (3x3 grid of character cards)
   - "Cómo jugar" section (using StepCard components)
   - Testimonials section (using TestimonialCard components)
   - Final CTA section

4. **Styling**
   - Use Halloween color palette throughout
   - Add Halloween-themed backgrounds (e.g., subtle patterns, pumpkins)
   - Maintain playful, child-friendly aesthetic
   - Ensure print-friendly (no complex gradients)

5. **Images & Assets**
   - Add product images to `src/assets/images/halloween/`
   - Add character card images for gallery (9 cards)
   - Consider placeholder images if final assets aren't ready

6. **Responsive Design**
   - Mobile-first approach
   - Gallery grid: 1 column mobile, 2 columns tablet, 3 columns desktop
   - Test CTA button visibility on all screen sizes

7. **Update Navigation**
   - Update header CTA button to point to `/halloween`
   - Update links from homepage to point to `/halloween`

---

## PAGE CONTENT

The page structure and content (in Spanish) should be as follows:

1. HERO SECTION

- Large product image or illustration of the Halloween lotería cards.
- Headline: “¡Lotería de Halloween! 👻”
- Subtitle: “Una versión mágica y divertida del clásico juego mexicano. Perfecta para niños y fiestas familiares.”
- CTA button: “Cómprala ahora”.

2. SECCIÓN: QUÉ INCLUYE

- Title: “¿Qué incluye la Lotería de Halloween?”
- List with icons:
  - 24 personajes únicos (brujita, vampiro, momia, fantasma, etc.)
  - 12 tablas de juego
  - Instrucciones fáciles
  - Versión imprimible y ecológica

3. SECCIÓN: POR QUÉ TE ENCANTARÁ

- Title: “¿Por qué te encantará?”
- Four highlights with emojis:
  - 💀 Diversión en familia
  - 🧙 Diseños tiernos y no aterradores (para todas las edades)
  - 🎨 Arte exclusivo y colorido
  - 🧠 Estimula memoria y vocabulario en niños

4. SECCIÓN: GALERÍA DE CARTAS

- Title: “Conoce a los personajes”
- Grid of 9 example cards (3x3) with cute Halloween illustrations.
- Optional caption: “Cada personaje fue diseñado con amor y color para que los niños aprendan mientras juegan.”

5. SECCIÓN: CÓMO JUGAR

- Title: “Cómo jugar”
- Step-by-step instructions:
  1. Descarga e imprime las cartas.
  2. Reparte las tablas entre los jugadores.
  3. Canta las cartas al estilo tradicional: “¡Laaaa Brujitaaa!”
  4. ¡Gana quien complete su tabla!

6. SECCIÓN: TESTIMONIOS

- Title: “Lo que dicen las familias”
- 2–3 short quotes:
  - “A mis hijos les encantó, se rieron muchísimo con los personajes.” — María
  - “Una versión adorable del clásico, ideal para Halloween.” — Carlos

7. SECCIÓN: CTA FINAL

- Large centered CTA with fun Halloween background.
- Text: “🎃 ¡Consíguela ahora y vive la magia de Halloween en familia!”
- Button: “Comprar ahora”
