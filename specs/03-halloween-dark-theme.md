# Halloween Page - Dark Night Theme

Transform the Halloween page (`/halloween`) into a dark night theme with Halloween moon atmosphere while preserving the light theme for the home page and other pages.

---

## IMPLEMENTATION PLAN

### 🎨 **Color Palette**

**New Night Theme Colors** (add to `src/styles/global.css`):

```css
@theme {
  /* Existing colors remain unchanged */

  /* Night theme colors (add these) */
  --color-night-dark: #0a0118;        /* Deep dark purple-black */
  --color-night-purple: #1a0b2e;      /* Dark purple */
  --color-night-blue: #16213e;        /* Dark navy blue */
  --color-moon-cream: #fef5e7;        /* Creamy moon color */
  --color-moon-glow: #fff8dc;         /* Light cream glow */
  --color-halloween-orange-bright: #ff8c42; /* Brighter orange for dark bg */
}
```

**Color Usage Strategy:**
- **Backgrounds:** Gradients mixing black, purple, and blue tones
- **Text:**
  - Primary: Moon cream/white (`moon-cream`, `white`)
  - Accents: Bright orange (`halloween-orange-bright`)
  - Headings: Moon glow or bright orange (Mystery Quest font)
- **Cards:** Dark purple/blue with subtle transparency

### 🔧 **Component Updates Strategy**

**IMPORTANT:** All reusable component modifications must support BOTH themes to avoid breaking the home page.

#### **1. ValueCard.astro**

Add optional props for dark theme:
- `variant?: 'light' | 'dark'` (default: `'light'`)
- `dark`: `bg-night-purple/80` background, white/cream text
- `light`: Keep existing styling (white bg, gray text)

```astro
interface Props {
  icon: string;
  title: string;
  description: string;
  titleClass?: string;
  descriptionClass?: string;
  variant?: 'light' | 'dark'; // NEW
}
```

#### **2. TestimonialCard.astro**

Add optional props for dark theme:
- `variant?: 'light' | 'dark'` (default: `'light'`)
- `dark`: Dark purple background, cream text, orange quote marks
- `light`: Keep existing white background

```astro
interface Props {
  quote: string;
  author: string;
  quoteClass?: string;
  variant?: 'light' | 'dark'; // NEW
}
```

#### **3. FeatureList.astro**

Text color already customizable via `textClass` prop - no changes needed.
Just pass `textClass="text-moon-cream"` on Halloween page.

#### **4. Button.astro**

Already works well on dark backgrounds - no changes needed.
Existing orange/purple buttons have good contrast on dark.

### 📋 **Page-Specific Changes** (`src/pages/halloween.astro` only)

#### **Section 1: Hero Section**
```astro
<section class="bg-linear-to-br from-night-dark via-night-purple to-night-blue py-20 md:py-32">
```
- Title: Keep `font-mystery`, change to `text-halloween-orange-bright` or `text-moon-glow`
- Subtitle: Change to `text-moon-cream`
- Button: Keep as-is (orange stands out well)

#### **Section 2: "Qué Incluye"**
```astro
<section class="py-20 bg-night-dark">
```
- Heading: `text-moon-glow font-mystery`
- FeatureList items: Pass `textClass="text-moon-cream"`
- Icons: Keep colorful emojis (they pop on dark)

#### **Section 3: "Por Qué Te Encantará"**
```astro
<section class="py-20 bg-linear-to-br from-night-purple/60 to-night-dark">
```
- Heading: `text-halloween-orange-bright font-mystery`
- ValueCard: Pass `variant="dark"` prop
- Text: Already passing custom classes, update to cream/white

#### **Section 4: Gallery**
```astro
<section class="py-20 bg-linear-to-b from-night-blue/80 to-night-dark">
```
- Heading: `text-moon-glow font-mystery`
- Caption: `text-moon-cream`
- Character cards: Add subtle orange glow on hover

#### **Section 5: "Cómo Jugar"**
```astro
<section class="py-20 bg-halloween-dark text-white">
```
- Already dark! Maybe enhance:
  - Change to `bg-linear-to-br from-night-dark via-night-purple to-night-blue`
  - Heading: `text-halloween-orange-bright`
  - Keep step text white

#### **Section 6: Testimonials**
```astro
<section class="py-20 bg-linear-to-br from-night-purple/70 to-night-dark">
```
- Heading: `text-moon-glow font-mystery`
- TestimonialCard: Pass `variant="dark"` prop

#### **Section 7: Final CTA**
```astro
<section class="py-20 bg-linear-to-br from-night-dark via-night-purple to-brand-orange/20">
```
- Inner card: Dark background with glow effect
  - `bg-night-purple/80 border-2 border-halloween-orange-bright/50`
- Heading: `text-halloween-orange-bright font-mystery`
- Text: `text-moon-cream`
- Button: Keep orange (pops on dark)

### ✅ **Implementation Steps**

1. **Add night theme colors** to `src/styles/global.css` @theme block

2. **Update ValueCard.astro**
   - Add `variant` prop
   - Conditional styling for light/dark
   - Default to `'light'` to preserve home page

3. **Update TestimonialCard.astro**
   - Add `variant` prop
   - Conditional styling for light/dark
   - Default to `'light'`

4. **Update halloween.astro page** (section by section)
   - Update section backgrounds to dark gradients
   - Update heading colors (orange-bright or moon-glow)
   - Update text colors (moon-cream)
   - Pass `variant="dark"` to ValueCard and TestimonialCard
   - Pass `textClass="text-moon-cream"` to FeatureList

5. **Test home page** (`/`) to ensure no visual regressions

6. **Test Halloween page** (`/halloween`) for contrast and readability

7. **Optional enhancements**
   - Add text-shadow glow to headings
   - Add box-shadow glow to cards
   - Add subtle hover effects with orange glow

### 🎯 **Accessibility Checklist**

- ✅ Moon cream (#fef5e7) on dark purple (#1a0b2e) = ~13:1 ratio (Excellent)
- ✅ White on night-dark = ~18:1 ratio (Excellent)
- ✅ Orange-bright (#ff8c42) on night-dark = ~7:1 ratio (Good)
- ✅ All text meets WCAG AA standard (4.5:1 minimum)

### 🚫 **DO NOT MODIFY**

- Home page (`src/pages/index.astro`)
- Header component (shared across pages)
- Footer component (shared across pages)
- Any component defaults that would break the home page

### 🎨 **Optional Visual Enhancements**

```css
/* Heading glow effect */
.heading-glow {
  text-shadow: 0 0 20px rgba(255, 140, 66, 0.5);
}

/* Card glow effect */
.card-glow {
  box-shadow: 0 0 30px rgba(255, 140, 66, 0.3);
}

/* Moon glow text */
.moon-text {
  text-shadow: 0 0 10px rgba(254, 245, 231, 0.4);
}
```

Apply these classes on Halloween page only via Tailwind arbitrary values or inline styles.

---

## Summary

This plan transforms only the Halloween page into a spooky night theme while keeping the home page and all shared components working with their original light theme. Component changes are backward-compatible with sensible defaults.
