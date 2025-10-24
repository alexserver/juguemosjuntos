Create a responsive landing page for a children's board game company called "Juguemos Juntos".

The design should be colorful, friendly, and playful — with rounded shapes, bright colors (orange, yellow, purple, turquoise), and a cheerful cartoon aesthetic.

---

## IMPLEMENTATION PLAN

### Component Structure

**Reusable Components** (create in `src/components/`):
1. **Header.astro** - Top navigation with logo, menu items, and CTA button
2. **Footer.astro** - Links, social media icons, contact info, and copyright
3. **ValueCard.astro** - Reusable card for mission/values section (receives icon, title, description as props)
4. **Button.astro** - Reusable button component with variants (primary, secondary, outline)

**Page-Specific Components** (optional, can be inline in index.astro):
- Hero section
- About section
- Games showcase
- Newsletter section

### Tailwind Configuration & Styling Best Practices

**1. Configure Tailwind v4 Theme Variables in `src/styles/global.css`:**

```css
@import "tailwindcss";

@theme {
  /* Custom brand colors */
  --color-brand-orange: #ff6b35;
  --color-brand-yellow: #f7b801;
  --color-brand-purple: #8b5fbf;
  --color-brand-turquoise: #00cfc1;

  /* Extended border radius for playful design */
  --radius-xl: 1rem;
  --radius-2xl: 1.5rem;
  --radius-3xl: 2rem;

  /* Playful font family */
  --font-playful: Quicksand, Nunito, Comic Sans MS, system-ui, sans-serif;
}
```

This creates utilities like:
- `bg-brand-orange`, `text-brand-purple`, etc.
- `rounded-xl`, `rounded-2xl`, `rounded-3xl`
- `font-playful`

**2. Styling Approach:**
- **Use Tailwind utility classes directly in `.astro` files** - This is the recommended approach for component-specific styles
- **Keep `global.css` minimal** - Only for Tailwind directives and base resets
- **Use `@layer base`** in `global.css` for global typography and body styles if needed
- **Avoid external CSS files** for component styles - keep styles co-located with components

**3. Component Styling Pattern:**
```astro
---
interface Props {
  variant?: 'primary' | 'secondary';
}
const { variant = 'primary' } = Astro.props;
---

<button class:list={[
  'px-6 py-3 rounded-3xl font-bold font-playful transition-all',
  variant === 'primary' && 'bg-brand-orange hover:brightness-110 text-white',
  variant === 'secondary' && 'bg-brand-purple hover:brightness-110 text-white',
]}>
  <slot />
</button>
```

### Implementation Order

1. **Setup** (first)
   - Update `src/styles/global.css` with `@theme` directive for custom colors, fonts, and border radius
   - Add any base styles or typography resets if needed

2. **Layout Components**
   - `src/components/Header.astro` - sticky header with navigation
   - `src/components/Footer.astro` - footer with links and social

3. **Reusable Components**
   - `src/components/Button.astro` - primary/secondary variants
   - `src/components/ValueCard.astro` - for mission section

4. **Page Sections** (build in `src/pages/index.astro` top to bottom)
   - Import Header and Footer
   - Hero section
   - About section ("Quiénes Somos")
   - Mission & Values section (using ValueCard components)
   - Games showcase section
   - Newsletter section

5. **Responsive Design**
   - Mobile-first approach using Tailwind breakpoints (sm, md, lg, xl)
   - Test navigation menu collapse on mobile
   - Ensure grid layouts stack properly on small screens

6. **Polish**
   - Add hover states and transitions
   - Ensure accessibility (aria-labels, semantic HTML)
   - Add placeholder images or SVG illustrations

---

## PAGE CONTENT

The page structure and content (in Spanish) should be as follows:

1. HEADER

- Logo “Juguemos Juntos”
- Navigation menu: “Inicio”, “Sobre Nosotros”, “Juegos”, “Contacto”
- Right-side button: “Conoce nuestra Lotería de Halloween 🎃”

2. HERO SECTION

- Big headline: “Juguemos juntos y aprendamos jugando 🎲💛”
- Subtitle: “Creamos juegos de mesa divertidos que unen a las familias y estimulan la imaginación de los niños.”
- CTA button: “Descubre nuestro primer juego”
- Background illustration: children playing board games together, bright and happy colors.

3. SECCIÓN: QUIÉNES SOMOS

- Title: “Nuestra historia”
- Text: “Juguemos Juntos nació de nuestra pasión por los juegos de mesa y los momentos en familia. Queremos que cada juego sea una oportunidad para reír, aprender y compartir.”
- Add an illustration or photo of the founders (optional).

4. SECCIÓN: NUESTRA MISIÓN Y VALORES

- Title: “Nuestra misión y valores”
- Four values with icons:
  1. 🎯 Diversión educativa — cada juego enseña algo nuevo.
  2. 💞 Conexión familiar — fortalecer lazos entre padres e hijos.
  3. 🌱 Creatividad — fomentar la imaginación y curiosidad.
  4. 🌎 Inclusión — juegos pensados para todos.

5. SECCIÓN: NUESTROS JUEGOS

- Title: “Nuestros juegos”
- Showcase one game: “Lotería de Halloween”
- Text: “Nuestro primer juego es una versión mágica y divertida del clásico mexicano, con personajes tiernos y coloridos.”
- Button: “Ver más” → link to the product landing page.

6. SECCIÓN: COMUNIDAD / NEWSLETTER

- Title: “¿Quieres enterarte de nuestros próximos juegos?”
- Short text: “Suscríbete para recibir noticias, lanzamientos y actividades divertidas.”
- Email input + button: “¡Sí, quiero jugar más!”

7. FOOTER

- Include links: “Inicio · Sobre Nosotros · Juegos · Contacto”
- Social media icons (Instagram, TikTok)
- Email: “contacto@juguemosjuntos.com”
- Copyright © 2025 Juguemos Juntos. Todos los derechos reservados.
