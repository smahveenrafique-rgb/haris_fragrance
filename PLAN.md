# Haris Fragrance - Premium Perfume E-Commerce Website Implementation Plan

## Project Overview
A luxury, cinematic, and fully responsive e-commerce website for Haris Fragrance - a premium Pakistani perfume brand. The site evokes an ultra-premium fragrance house aesthetic (Tom Ford, Creed, Jo Malone) with Middle Eastern luxury sensibilities.

**CRITICAL**: This is a single-file implementation - ALL code (HTML, CSS, JS) lives in `index.html`. No external `.css` or `.js` files.

---

## Build Architecture (Single File)

### Structure
```
index.html
├── <head>
│   ├── CDN links (Tailwind, Remix Icons, Google Fonts)
│   ├── <style> - All custom CSS (animations, glass-morphism, CSS variables)
│   └── <script> - Tailwind config + All JavaScript
└── <body>
    ├── Navigation Bar (sticky, glass-morphism)
    ├── Hero Section (cinematic, full-screen)
    ├── Brand Story Section
    ├── Brand Pillars Section
    ├── Collections Category Showcase
    ├── Product Grid with Filter Bar
    ├── Bestsellers Showcase
    ├── Our Promise Section
    ├── Newsletter/WhatsApp Section
    ├── Footer
    └── Modals (hidden by default)
        ├── Product Detail Modal (Screen 1)
        └── Checkout Modal (Screen 2)
```

---

## Critical Requirements (Non-Negotiable)

### 1. Single File Architecture
- Everything in ONE file: `index.html`
- No external `.css` files, no external `.js` files
- All `<style>` and `<script>` blocks inside index.html

### 2. Target Screen
- Primary: 1080p (1920×1080) desktop
- Scale down to tablet (768px) and mobile (375px)

### 3. Category Color Tokens (Define ONCE in :root)
```css
:root {
  --cat-edp:       #C9A84C;   /* Eau de Parfum  → Heritage Gold */
  --cat-attar:     #8C6A3F;   /* Attar / Oils   → Warm Bronze  */
  --cat-oud:       #5C3D2E;   /* Oud Collection → Deep Mahogany */
  --cat-oriental:  #8C3A50;   /* Oriental Blends→ Burgundy Rose */
  --cat-giftsets:  #4A6741;   /* Gift Sets      → Forest Sage  */
}
```
Usage: Filter buttons, category cards, product badges ALL use these tokens.

### 4. Category Filter System (SHARED FUNCTION)
- Filter bar buttons AND category showcase cards call SAME `filterProducts(category)` function
- Both surfaces stay in sync - clicking either updates product grid identically
- Uses ONE function, NOT two separate event listeners

### 5. Pre-Populated Product Grid
- All 18 products rendered on first load
- Call `filterProducts('all')` in DOMContentLoaded
- No empty states, no "select a category" messages

### 6. Product Removal Animation
- Any card removal uses fade-out + slide-up animation BEFORE DOM removal
- Never remove instantly - always animate first (380ms animation)

### 7. Two-Screen Modal Flow
- Screen 1: Product Detail (opens on product card click)
- Screen 2: Checkout Form (opens from "Proceed to Checkout" in Screen 1)
- NEVER combine or skip Screen 1

### 8. Image Handling
- Use local filenames from prompt specification
- Every `<img>` has onerror fallback to show placeholder (not broken icon)

### 9. Zero Console Errors
- Test every interactive feature before delivery

### 10. Floating WhatsApp Button
- Fixed bottom-right, visible at ALL times, on ALL sections
- z-index: 9999 to stay above other elements

---

## Page Sections

### 1. Navigation Bar
- Sticky, transparent on hero → glass-morphism after 80px scroll
- Logo (left desktop, center mobile), nav links, cart icon
- WhatsApp CTA button with pulse animation
- Mobile: hamburger menu with full-screen drawer

### 2. Hero Section (Full-Screen)
- 100vh with background image
- Multi-layer overlay with gradient
- Letter-by-letter text animation
- Floating golden dust particles (CSS keyframes)
- Two CTA buttons + scroll indicator

### 3. Brand Story - "The Art of Fragrance"
- Two-column layout (text left, image right)
- Gold border with corner brackets on image
- Brand narrative + heritage points

### 4. Brand Pillars - "Why Choose Haris Fragrance"
- 4 cards: Luxury Craftsmanship, Pure Ingredients, Long-Lasting Sillage, Worldwide Shipping
- Icons, gold top border, hover lift effect

### 5. Collections - Category Showcase
- 5 categories: EDP, Attar, Oud, Oriental, Gift Sets
- Each card calls `filterProducts()` on click
- Horizontal scroll on mobile, 5-column grid desktop
- Active state sync with filter bar

### 6. Premium Collection - Product Grid
- Filter bar: All, Eau de Parfum, Attar/Oils, Oud, Oriental, Gift Sets
- 3-column grid (desktop), 2-column (tablet), 1-column (mobile)
- 18 products with cards: image, badge, rating, name, description, price, buttons

### 7. Bestsellers - Featured Showcase
- 3 featured products (Royal Oud Intense, Rose Elixir, Black Musk Attar)
- Larger cards with gold leaf ornaments
- BEST SELLER badge

### 8. Our Promise Section
- Two-column: text left, image right
- Promise points with checkmarks

### 9. Newsletter/WhatsApp Section
- WhatsApp number input
- Pre-filled WhatsApp opt-in link

### 10. Footer
- 3-column: Brand, Quick Links, Connect
- Social icons, copyright bar

---

## Interactive Features

### Product Filtering
- Categories: all, edp, attar, oud, oriental, giftsets
- Filter animation: fade out → fade in with stagger
- Active filter button uses category color token

### Product Detail Modal (Screen 1)
- 2-column layout (desktop): image left, details right
- Volume selector (30ml/50ml/100ml) with dynamic price
- Quantity selector (+/-) with subtotal update
- Notes tabs: Top/Middle/Base
- Proceed to Checkout button → opens Screen 2

### Checkout Modal (Screen 2)
- Order summary with line items
- Customer form: Name, Phone, City, Address, Notes
- Send via WhatsApp button with pre-encoded message

### WhatsApp Integration
- Pre-filled order message format
- Includes: product, volume, qty, prices, customer details
- Opens wa.me link

---

## Animations

### Page Load Sequence
- 0ms: Navbar fades in
- 200ms: Hero text letter-by-letter
- 600ms: Subheading fades up
- 900ms: CTA buttons slide up
- 1200ms: Scroll indicator bounces

### Scroll Animations (Intersection Observer)
- Section headings: fade-up
- Product cards: staggered fade-in (40ms delay)
- Images: fade-in with scale

### Hover Effects
- Product cards: translateY(-6px) + gold shadow
- Product images: scale(1.08)
- Nav links: gold underline slide
- CTA buttons: translateY(-3px) + shimmer
- WhatsApp button: pulse

### Card Removal Animation
- 380ms fade-out with scale and translate
- DOM element removed after animation ends

---

## Color Palette (CSS Variables)

```css
:root {
  --color-bg-primary:     #0D0A07;
  --color-bg-secondary:   #1A1410;
  --color-bg-card:        #211A13;
  --color-gold-primary:   #C9A84C;
  --color-gold-light:     #E8C97A;
  --color-gold-muted:     #8B7340;
  --color-cream:          #F2EAD8;
  --color-cream-muted:    #A89880;
  --color-rose-accent:    #8C3A50;
  --color-white:          #FFFFFF;

  /* Category Color Tokens */
  --cat-edp:       #C9A84C;
  --cat-attar:     #8C6A3F;
  --cat-oud:       #5C3D2E;
  --cat-oriental:  #8C3A50;
  --cat-giftsets:  #4A6741;
}
```

---

## Typography

| Element | Font | Weight |
|---------|------|--------|
| Hero Headline | Cinzel | 700 |
| Section Headings | Cormorant Garamond | 600 |
| Brand Taglines | Dancing Script | 600 |
| Body Text | Jost | 300-400 |
| Prices/Labels | Cinzel | 400 |

---

## Responsive Breakpoints

| Breakpoint | Width | Layout |
|------------|-------|--------|
| Desktop | > 1024px | 3-column grid, full nav |
| Tablet | 640-1024px | 2-column grid, 2x2 features |
| Mobile | < 640px | 1-column, hamburger nav |

---

## Acceptance Checklist

### Structure
- [ ] Single index.html file
- [ ] Zero console errors
- [ ] Opens directly in browser

### Layout
- [ ] Renders at 1920×1080 (primary)
- [ ] Responsive at all breakpoints
- [ ] No horizontal scroll

### Colors
- [ ] Category tokens defined once in :root
- [ ] Filter button matches category card color

### Products
- [ ] 18 products visible on first load
- [ ] filterProducts('all') called on load

### Filtering
- [ ] Filter bar and category cards use SAME function
- [ ] Both surfaces sync on click
- [ ] Smooth fade transition animation

### Modals
- [ ] Screen 1 opens on product click
- [ ] Screen 2 opens from Screen 1 only
- [ ] Volume/quantity update price in real-time

### WhatsApp
- [ ] Floating button always visible
- [ ] Order message pre-filled correctly

### Quality
- [ ] Premium luxury aesthetic
- [ ] All hover/animations work

---

## Implementation Priority

1. **Foundation**: Setup HTML, CDN links, CSS variables, Tailwind config
2. **Structure**: Build all 10 sections with proper layout
3. **Products**: Add 18 products with filtering system
4. **Interactivity**: Build two-screen modal system
5. **WhatsApp**: Integrate order submission
6. **Polish**: Animations, hover effects, scroll behaviors
7. **Testing**: Verify all acceptance criteria

---

**Document Version**: 2.0
**Based on**: Haris_Fragrance_Website_Prompt.md Build Specification
**Project**: Haris Fragrance - Premium Perfume E-Commerce Website