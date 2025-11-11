# Spec 08: Replace WhatsApp CTAs with Direct Product Links

## Overview
Replace the WhatsApp-based CTA buttons on theme pages with direct links to product purchase pages (e.g., Gumroad). Move WhatsApp contact to footer as a general support channel.

**Scope:** English site only (`apps/site-en/`)

## Current State

### CTA Buttons (2 per theme page)
- **Location:** Hero section and Final CTA section on each theme page
- **Current behavior:** Open WhatsApp with theme-specific pre-filled message
- **Implementation:**
  - Lines 56-58 in halloween.astro: Constructs WhatsApp link from env vars
  - Lines 100 & 353: Two Button components using `whatsappLink`
  - Similar pattern in christmas.astro and zoo.astro

### Environment Variables (.env)
```
PUBLIC_WHATSAPP_NUMBER=529845937808
PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN="Hi! I'm interested in buying..."
PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS="Hi! I'm interested in buying..."
PUBLIC_WHATSAPP_MESSAGE_ZOO="Hi! I'm interested in buying..."
```

### Product Data (data/products.json)
```json
[
  { "id": "halloween", "name": "Halloween Bingo", "price": 15, "currency": "USD" },
  { "id": "christmas", "name": "Christmas Bingo", "price": 15, "currency": "USD" },
  { "id": "zoo", "name": "Zoo Bingo", "price": 15, "currency": "USD" }
]
```

### Footer Contact Section
- Currently only shows email link
- No WhatsApp button

## Desired State

### 1. Product Data - Add CTA Links
**File:** `apps/site-en/src/data/products.json`

Add `cta_link` property to each product:
```json
[
  {
    "id": "halloween",
    "name": "Halloween Bingo",
    "price": 15,
    "currency": "USD",
    "cta_link": ""  // User will fill this value
  },
  {
    "id": "christmas",
    "name": "Christmas Bingo",
    "price": 15,
    "currency": "USD",
    "cta_link": ""  // User will fill this value
  },
  {
    "id": "zoo",
    "name": "Zoo Bingo",
    "price": 15,
    "currency": "USD",
    "cta_link": ""  // User will fill this value
  }
]
```

### 2. Environment Variables - Simplify to Single Support Message
**File:** `apps/site-en/.env`

Replace theme-specific messages with single support message:
```
PUBLIC_WHATSAPP_NUMBER=529845937808
PUBLIC_WHATSAPP_MESSAGE_SUPPORT="I need some help with my purchase"
```

**Remove:**
- `PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN`
- `PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS`
- `PUBLIC_WHATSAPP_MESSAGE_ZOO`

### 3. Footer - Add WhatsApp Support Button
**File:** `apps/site-en/src/components/Footer.astro`

**Add to frontmatter section (after imports):**
```astro
const whatsappNumber = import.meta.env.PUBLIC_WHATSAPP_NUMBER;
const whatsappSupportMessage = import.meta.env.PUBLIC_WHATSAPP_MESSAGE_SUPPORT;
const whatsappSupportLink = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(whatsappSupportMessage)}`;
```

**Add to Contact section (after email link, around line 63):**
```astro
<a
  href={whatsappSupportLink}
  target="_blank"
  rel="noopener noreferrer"
  class="text-purple-100 hover:text-brand-yellow transition-colors block"
>
  💬 WhatsApp Support
</a>
```

### 4. Theme Pages - Update CTA Buttons
**Files to modify:**
- `apps/site-en/src/pages/halloween.astro`
- `apps/site-en/src/pages/christmas.astro`
- `apps/site-en/src/pages/zoo.astro`

**Changes per file:**

#### Remove (lines 56-58):
```astro
const whatsappNumber = import.meta.env.PUBLIC_WHATSAPP_NUMBER;
const whatsappMessage = import.meta.env.PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN;
const whatsappLink = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(whatsappMessage)}`;
```

#### Add (after getting product data, around line 52):
```astro
const ctaLink = halloweenProduct?.cta_link || "#";
```

#### Update Button Components:
**Hero section CTA (around line 100):**
```astro
<Button variant="halloween" href={ctaLink} target="_blank">
  Get Digital Version
</Button>
```

**Final CTA section (around line 353):**
```astro
<Button
  variant="halloween"
  class="text-xl px-8 py-4"
  href={ctaLink}
  target="_blank"
>
  Get Digital Version
</Button>
```

## Implementation Checklist

### Phase 1: Data Structure
- [ ] Add `cta_link` property to all 3 products in `apps/site-en/src/data/products.json`
- [ ] Set initial values to empty strings (user will fill later)

### Phase 2: Environment Variables
- [ ] Remove `PUBLIC_WHATSAPP_MESSAGE_HALLOWEEN` from `.env`
- [ ] Remove `PUBLIC_WHATSAPP_MESSAGE_CHRISTMAS` from `.env`
- [ ] Remove `PUBLIC_WHATSAPP_MESSAGE_ZOO` from `.env`
- [ ] Add `PUBLIC_WHATSAPP_MESSAGE_SUPPORT` to `.env`

### Phase 3: Footer Component
- [ ] Add WhatsApp support button to `apps/site-en/src/components/Footer.astro`
- [ ] Place below email link in Contact section
- [ ] Use `PUBLIC_WHATSAPP_MESSAGE_SUPPORT` environment variable
- [ ] Test WhatsApp link opens correctly with support message

### Phase 4: Halloween Page
- [ ] Remove WhatsApp link construction code (lines 56-58)
- [ ] Add `ctaLink` variable from product data
- [ ] Update hero section CTA button to use `ctaLink`
- [ ] Update final CTA section button to use `ctaLink`

### Phase 5: Christmas Page
- [ ] Remove WhatsApp link construction code
- [ ] Add `ctaLink` variable from product data
- [ ] Update hero section CTA button to use `ctaLink`
- [ ] Update final CTA section button to use `ctaLink`

### Phase 6: Zoo Page
- [ ] Remove WhatsApp link construction code
- [ ] Remove unused `PUBLIC_WHATSAPP_MESSAGE_ZOO` reference
- [ ] Add `ctaLink` variable from product data
- [ ] Update hero section CTA button to use `ctaLink`
- [ ] Update final CTA section button to use `ctaLink`

### Phase 7: Testing
- [ ] Verify all CTA buttons use `cta_link` from product data
- [ ] Verify footer WhatsApp button appears on all pages
- [ ] Verify footer WhatsApp button uses support message
- [ ] Test with empty `cta_link` values (should default to "#")
- [ ] Build app successfully: `npm run build:en`

## User Action Required After Implementation

User needs to fill in the `cta_link` values in `apps/site-en/src/data/products.json`:

```json
{
  "id": "halloween",
  "cta_link": "https://gumroad.com/l/halloween-bingo"  // Example
}
```

## Benefits

1. **Streamlined Purchase Flow:** Direct links to product pages reduce friction
2. **Better Conversion:** Eliminates WhatsApp as intermediate step
3. **Scalability:** Easy to add more products without new env variables
4. **Maintained Support:** WhatsApp still available in footer for help
5. **Cleaner Code:** Product data centralized, fewer environment variables

## Notes

- Keep button text as "Get Digital Version" (no change needed)
- Keep all visual styling unchanged
- CTA buttons will open in new tab (`target="_blank"`)
- If `cta_link` is empty, default to "#" to prevent errors
- Spanish site (`apps/site-es/`) remains unchanged
