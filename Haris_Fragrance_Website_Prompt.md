# 🌸 Haris Fragrance – Premium Perfume E-Commerce Website
### Senior Developer Build Specification | Version 2.0

---

## 🚨 CRITICAL BUILD DIRECTIVE — READ FIRST

> This document IS the complete build specification. There is no separate `PLAN.md` or `DESIGN.md` — **all design and planning instructions are embedded here**. Follow this specification exactly as written from top to bottom.

### ✅ Non-Negotiable Build Rules (Enforce Before Writing a Single Line of Code)

```
1.  Everything lives in ONE file: index.html
    No external .css files. No external .js files.
    All <style> blocks and <script> blocks go inside index.html.

2.  Target screen: 1080p (1920×1080) as the primary desktop baseline.
    Design, spacing, font sizes, and grid columns must look perfect at 1080p.
    Then scale down gracefully to tablet (768px) and mobile (375px).

3.  Category color tokens must be defined ONCE as CSS variables and reused
    everywhere — product cards, filter buttons, category showcase cards,
    and any badge or label must all reference the same token.
    NO hardcoded hex values repeated for the same category in multiple places.

4.  The category filter system is the heart of the product section.
    Both the filter bar buttons AND the category showcase cards (Section 5)
    must call the same filterProducts(category) function.
    They must always stay in sync — clicking either updates the product grid identically.

5.  The website must arrive pre-populated.
    On first load, ALL 18 products are rendered in the grid by default (no empty states,
    no "please select a category" messages). The site looks fully alive immediately.

6.  All product removal interactions (e.g., wishlist removes, cart clears) must
    use a smooth CSS fade-out + slide-up animation before the DOM element is removed.
    Never remove elements from the DOM instantly — always animate first, then remove.

7.  Modals are TWO separate screens with a transition between them:
    Screen 1 → Product Detail (opened by clicking any product card)
    Screen 2 → Checkout Form (opened ONLY from "Proceed to Checkout" inside Screen 1)
    Never combine them. Never skip Screen 1 to go directly to Screen 2.

8.  No external image URLs. No placeholder services (Unsplash, Picsum, Lorem Picsum).
    Every <img> src must be a local filename exactly matching the image files list.
    If an image fails to load, show the fallback placeholder — not a broken image icon.

9.  Zero console errors on Chrome DevTools after build.
    Test every interactive feature before considering the build complete.

10. The floating WhatsApp button (bottom-right, fixed) must be visible at ALL times,
    on ALL sections, on ALL screen sizes. It is never hidden, never clipped.
```

---

## 📌 Project Overview

Design and develop a **luxury, cinematic, and fully responsive e-commerce website** for **"Haris Fragrance"** — a premium Pakistani perfume brand offering an exclusive range of Eau de Parfum, Attar (Arabic Oils), Oud Collections, Oriental Blends, and Gift Sets.

The website must evoke an **ultra-premium fragrance house aesthetic** — think Tom Ford, Creed, and Jo Malone blended with rich Middle Eastern luxury sensibilities. The visual language should feel **editorial, sensorial, and aspirational**: dark, opulent, with gold accents and cinematic motion. Every scroll, hover, and interaction should reinforce the brand's identity as a **world-class fragrance house rooted in Karachi**.

---

## 🎨 Branding & Visual Identity

### Logo
- Display `logo.png` in the navigation bar (left-aligned on desktop, centered on mobile)
- On scroll, the navbar shrinks subtly (compact mode) with a glass-morphism blur background
- Logo should have a **slight golden glow effect** on hover

### Color Palette (CSS Variables)
```css
--color-bg-primary:     #0D0A07;   /* Deep Noir – main background */
--color-bg-secondary:   #1A1410;   /* Rich Dark Brown – card & section backgrounds */
--color-bg-card:        #211A13;   /* Warm Charcoal – product cards */
--color-gold-primary:   #C9A84C;   /* Heritage Gold – primary accent */
--color-gold-light:     #E8C97A;   /* Soft Gold – hover states, stars */
--color-gold-muted:     #8B7340;   /* Muted Gold – borders, dividers */
--color-cream:          #F2EAD8;   /* Ivory Cream – primary text */
--color-cream-muted:    #A89880;   /* Warm Gray – secondary text, descriptions */
--color-rose-accent:    #8C3A50;   /* Burgundy Rose – category accents */
--color-white:          #FFFFFF;
```

### ⚠️ Category Color Tokens (Define Once, Use Everywhere)

These 5 tokens must be declared in `:root` and used **consistently** across category cards, filter buttons, product badges, and any other category-labeled element. Never hardcode a category color twice.

```css
:root {
  /* ... other variables ... */
  --cat-edp:       #C9A84C;   /* Eau de Parfum  → Heritage Gold */
  --cat-attar:     #8C6A3F;   /* Attar / Oils   → Warm Bronze  */
  --cat-oud:       #5C3D2E;   /* Oud Collection → Deep Mahogany */
  --cat-oriental:  #8C3A50;   /* Oriental Blends→ Burgundy Rose */
  --cat-giftsets:  #4A6741;   /* Gift Sets      → Forest Sage  */
}
```

**Usage contract:**
- Filter button active state → `background: var(--cat-{category})`
- Category showcase card accent border → `border-color: var(--cat-{category})`
- Product card category badge → `background: var(--cat-{category})`
- If a new category is ever added, only ONE variable needs to be added here

---
```html
<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=Cinzel:wght@400;600;700&family=Jost:wght@300;400;500&family=Dancing+Script:wght@600&display=swap" rel="stylesheet">
```

| Use | Font | Weight | Notes |
|---|---|---|---|
| Hero Headline | `Cinzel` | 700 | All-caps, dramatic letter-spacing |
| Section Headings | `Cormorant Garamond` | 600 | Italic variant for elegance |
| Brand Taglines | `Dancing Script` | 600 | Cursive flourish accents |
| Body / Descriptions | `Jost` | 300–400 | Light, airy readability |
| Prices / Labels | `Cinzel` | 400 | Monospaced-feel numerals |

### Icons
- **Remix Icon** CDN for all UI icons
- **Custom SVG flame/drop icons** for category decoration

### Styling Framework
- **Tailwind CSS** via CDN (custom config for extended colors)
- Supplemented with a `<style>` block for custom animations and glass-morphism effects

---

## 🗂 Page Architecture & Sections

---

### 1. 🧭 Navigation Bar (Sticky + Smart Scroll)

**Desktop Layout:**
```
[Logo]    [Home]  [Collections]  [About]  [Our Story]  [Contact]    [🛒 Cart(0)]  [WhatsApp Button]
```

**Mobile Layout:**
- Hamburger menu (animated 3-line → X morphing icon)
- Full-screen slide-in drawer with nav links + social icons
- Cart icon always visible in top-right

**Features:**
- Transparent on hero → dark glass-morphism (`backdrop-filter: blur(20px)`) after 80px scroll
- **Active section indicator:** nav links highlight based on scroll position (Intersection Observer)
- WhatsApp CTA button: gold border, subtle pulse animation (`box-shadow` keyframe), WhatsApp leaf icon
- Hover: gold underline slides in from left (`::after` pseudo-element animation)
- **Mini cart counter badge** (gold circle) shows item count

---

### 2. 🎬 Hero Section (Full-Screen Cinematic)

**Layout:** Full viewport height (`100vh`), `hero.jpg` background

**Visual Treatment:**
- Multi-layer overlay: `linear-gradient(135deg, rgba(13,10,7,0.85) 0%, rgba(13,10,7,0.5) 50%, rgba(201,168,76,0.15) 100%)`
- Subtle animated **noise/grain texture overlay** for film-grain depth
- Floating **golden dust particle** effect (CSS-only `@keyframes` drifting dots)

**Content (centered or slightly left-aligned):**
```
[Decorative line] — EST. 2020 — [Decorative line]

HARIS FRAGRANCE                          ← Cinzel, 72-96px, letter-spacing: 0.3em
                                         ← Animated letter-by-letter fade-in

"Where Every Scent Tells a Story"        ← Dancing Script, italic, gold color

Your signature scent awaits. Handcrafted ← Jost 300, cream-muted, max-width: 520px
attars and exclusive parfums inspired 
by the Arabian nights and modern luxury.

[Explore Collections ▶]   [Our Story]   ← Two CTA buttons side by side
```

**Button Styles:**
- Primary: Gold background, dark text, `transform: translateY(-3px)` on hover, shimmer sweep animation
- Secondary: Gold border only, cream text, fills gold on hover

**Scroll Indicator:** Animated bouncing arrow at the bottom center

---

### 3. 📖 Brand Story – "The Art of Fragrance" Section

**Layout:** Full-width, alternating two-column on desktop, stacked on mobile

**Left Column – Text:**
```
[Small gold label]: OUR HERITAGE

The Art of Fragrance                     ← Cormorant Garamond, italic, 42px
Since 2020

[2–3 paragraphs of brand narrative]

✦ Handcrafted in Karachi, Pakistan
✦ Sourced from the world's finest ingredients
✦ Every bottle tells a unique olfactory journey

[Discover Our Process →]                 ← Ghost button
```

**Right Column – Image:**
- `Our Story.jpg` with rounded-2xl corners
- Gold border (`2px solid var(--color-gold-muted)`)
- Hover: subtle `scale(1.03)` zoom with `overflow: hidden`
- Decorative **gold corner brackets** (CSS-drawn) around the image frame

**Background:** Slight diagonal texture band separating this section

---

### 4. ✨ Brand Pillars – "Why Choose Haris Fragrance"

**Layout:** 4 cards in a horizontal row (desktop), 2×2 grid (tablet), vertical stack (mobile)

**Each Card:**
```
[Icon in gold circle]
─────────────────────
PILLAR TITLE           ← Cinzel, 14px, letter-spacing: 0.2em
Short description      ← Jost 300, cream-muted, 14px
```

**4 Pillars:**
| Icon | Title | Description |
|---|---|---|
| `ri-vip-crown-2-line` | Luxury Craftsmanship | Every fragrance is blended by master perfumers using age-old techniques passed down through generations |
| `ri-leaf-line` | Pure Ingredients | We source only the finest oud, rose, musk, and exotic botanicals from global origins |
| `ri-time-line` | Long-Lasting Sillage | Our high-concentration formulas ensure your scent lingers powerfully for 8–12+ hours |
| `ri-global-line` | Worldwide Shipping | Secure, beautifully packaged delivery across Pakistan and internationally |

**Card Style:**
- Background: `var(--color-bg-card)` with gold top border (`3px`)
- Hover: `translateY(-8px)` lift + gold `box-shadow` glow
- Icon circle: gold gradient background

---

### 5. 🗃 Collections – Category Showcase

**Layout:** Horizontal scrollable ribbon on mobile; 5-column grid on desktop

**Each Category Card:**
- Full-image card with dark gradient overlay at bottom
- Category name in Cinzel, white, bottom-left
- Hover: overlay lightens, "Explore →" button fades in
- Active/selected: gold border highlight

**5 Categories:**
| Category Name | Image File | Description Badge |
|---|---|---|
| Eau de Parfum | `EDP Collection.jpg` | Classic Luxury |
| Attar (Arabic Oils) | `Attar Collection.jpg` | Alcohol-Free |
| Oud Collection | `Oud Collection.jpg` | Signature Oud |
| Oriental Blends | `Oriental Collection.jpg` | Exotic Fusion |
| Gift Sets | `Gift Sets.jpg` | Perfect Gifting |

**Interaction:**
- Clicking a category card calls `filterProducts(categoryKey)` — **the exact same function** used by the filter bar buttons in Section 6
- Both the category card AND its corresponding filter button must visually activate simultaneously (same active CSS class applied to both)
- Active category card gets a gold shimmer border animation
- **There must be zero divergence** between what the category card triggers and what the filter button triggers — they are two UI surfaces calling one shared function

> 💡 **Implementation pattern:**
> ```javascript
> // Define ONE shared filter function
> function filterProducts(category) {
>   currentFilter = category;
>   // 1. Update filter bar button active states
>   document.querySelectorAll('.filter-btn').forEach(btn => {
>     btn.classList.toggle('active', btn.dataset.category === category);
>   });
>   // 2. Update category card active states
>   document.querySelectorAll('.category-card').forEach(card => {
>     card.classList.toggle('active', card.dataset.category === category);
>   });
>   // 3. Show/hide products with fade transition
>   renderFilteredProducts(category);
> }
> ```

---

### 6. 🛍 Premium Collection – Product Grid

**Filter Bar (Sticky within section):**
```
[ All ] [ Eau de Parfum ] [ Attar / Oils ] [ Oud Collection ] [ Oriental ] [ Gift Sets ]
```
- Active filter: uses `var(--cat-{category})` background, dark text — **must match the active color of the category card above**
- Inactive: Transparent, cream-muted text, gold border
- **Smooth filter transition:** products fade out (`opacity: 0`, `transform: translateY(10px)`) over 200ms, then filtered products fade back in with staggered `animation-delay` (40ms per card)
- Filter bar buttons and category showcase cards call the **same `filterProducts()` function** — they are always in sync
- On page load, `filterProducts('all')` is called automatically so all 18 products are visible from the first render

**Filter animation implementation:**
```javascript
function renderFilteredProducts(category) {
  const cards = document.querySelectorAll('.product-card');
  cards.forEach((card, index) => {
    const match = category === 'all' || card.dataset.category === category;
    if (!match) {
      card.style.opacity = '0';
      card.style.transform = 'translateY(10px)';
      setTimeout(() => card.style.display = 'none', 200);
    } else {
      card.style.display = 'block';
      // Staggered fade-in
      setTimeout(() => {
        card.style.opacity = '1';
        card.style.transform = 'translateY(0)';
      }, index * 40);
    }
  });
}
```

**Grid Layout:**
- Desktop: 3 columns
- Tablet (768px): 2 columns
- Mobile (<640px): 1 column (full-width cards)

**Product Card Design:**
```
┌─────────────────────────────┐
│  [Product Image 1:1 ratio]  │  ← Image fills square, zoom on hover
│  [NEW] or [BESTSELLER] badge│  ← Top-left corner tag
├─────────────────────────────┤
│  ★★★★★  (24 reviews)        │  ← Gold stars + review count, Jost 300
│                             │
│  Rose Oud Intense           │  ← Cormorant Garamond 600, 20px
│  50ml Eau de Parfum         │  ← Jost 300, cream-muted, 13px
│                             │
│  A captivating blend of...  │  ← Max 2 lines, truncated with ellipsis
│                             │
│  PKR 4,500                  │  ← Cinzel, gold color, 18px
│                             │
│  [Add to Cart] [♡]          │  ← Primary + Wishlist icon button
└─────────────────────────────┘
```

**Card Hover Effect:**
- Card: `translateY(-6px)` + gold `box-shadow`
- Image: `scale(1.08)` zoom (overflow hidden)
- "Add to Cart" button: slides up from bottom on hover (CSS transform)

**Clicking card OR "Add to Cart":** Opens **Product Detail Modal**

**Pre-Populated on First Load:**
All 18 products must be rendered in the grid when the page first opens. The grid must never appear empty. Implement by calling `filterProducts('all')` at the end of `DOMContentLoaded`. No skeleton loaders, no spinners — the cards render synchronously from the JS product data array.

**Card Removal Animation (Wishlist / Cart Clear):**
Any UI action that removes a product card from the visible DOM (e.g., removing from wishlist, clearing cart item) must follow this sequence — **never remove instantly**:

```css
/* Define once in <style> block */
@keyframes cardFadeOut {
  0%   { opacity: 1; transform: translateY(0) scale(1); max-height: 500px; }
  60%  { opacity: 0; transform: translateY(-12px) scale(0.96); }
  100% { opacity: 0; max-height: 0; padding: 0; margin: 0; overflow: hidden; }
}

.card-removing {
  animation: cardFadeOut 380ms cubic-bezier(0.4, 0, 0.2, 1) forwards;
  pointer-events: none;
}
```

```javascript
// Always use this pattern — never call .remove() directly
function removeCardWithAnimation(cardElement) {
  cardElement.classList.add('card-removing');
  cardElement.addEventListener('animationend', () => {
    cardElement.remove();
  }, { once: true });
}
```

---

### 7. 🏆 Bestsellers – Featured Showcase

**Layout:** 3 large featured cards in a row (desktop), stacked mobile

**Card Style – More Elaborate:**
- Larger image (taller aspect ratio, ~4:5)
- Decorative gold leaf corner ornament (SVG)
- `BEST SELLER` gold badge top-right
- Product name, 5-star rating, volume/concentration, description, price
- `View Details` button (outlined gold → solid gold on hover)

**3 Featured Products:**
| Product | Image |
|---|---|
| Royal Oud Intense | `Royal Oud Bestseller.jpg` |
| Rose Elixir Parfum | `Rose Elixir Bestseller.jpg` |
| Black Musk Attar | `Black Musk Bestseller.jpg` |

---

### 8. 💎 Our Promise Section

**Layout:** Two columns (text left, image right)

**Left Text:**
```
[Gold ornament line]
OUR COMMITMENT TO YOU              ← Cinzel label

A Promise of Purity & Luxury      ← Cormorant Garamond, italic

✦ 100% Authentic Ingredients
   No synthetic fillers, no shortcuts — ever.

✦ Satisfaction Guaranteed
   Unhappy? We make it right, every time.

✦ Secure Packaging
   Double-sealed, gift-ready boxes for every order.

✦ Discreet Delivery
   Private, professional packaging across Pakistan.
```

**Right:** `Our Promise.jpg` with the same ornamental gold frame treatment as About section

---

### 9. 📩 Newsletter / WhatsApp Section (New Section)

**Full-width dark band with gold accent:**
```
Stay in the Loop                   ← Cormorant Garamond, italic

Be the first to know about new launches, limited editions, and exclusive offers.

[  Enter your WhatsApp number  ]  [Join the VIP List →]
```
- WhatsApp number input → on submit, generates a pre-filled WhatsApp opt-in link
- Background: diagonal gold-to-transparent gradient

---

### 10. 🔻 Footer

**3-column layout on desktop, stacked mobile:**

**Column 1 – Brand:**
- Logo image
- Tagline: *"Luxury. Crafted. Worn."*
- WhatsApp button (green, pulsing)
- Brief brand description (2 lines)

**Column 2 – Quick Links:**
- Home, Collections, About, Our Story, Contact
- Heading in Cinzel, links in Jost 300

**Column 3 – Connect:**
- Social icons: Facebook, Instagram, TikTok, WhatsApp
- Icon style: gold circle outline, fills gold on hover
- Address: Karachi, Pakistan (optional)

**Footer Bottom Bar:**
```
© 2025 Haris Fragrance. All Rights Reserved.   |   Crafted with ♡ in Karachi
```
- Hairline gold top border
- Small Jost 300 text, centered

---

## 🗃 Complete Product Catalog

> All product images are local files placed in the same directory as `index.html`.

| # | Product Name | Price (PKR) | Rating | Reviews | Category | Image File |
|---|---|---|---|---|---|---|
| 1 | Rose Oud Intense | 4,500 | 5 | 42 | edp | Rose Oud Intense.jpg |
| 2 | Velvet Musk | 3,800 | 4 | 31 | edp | Velvet Musk.jpg |
| 3 | Midnight Amber | 5,200 | 5 | 56 | edp | Midnight Amber.jpg |
| 4 | Jasmine Noir | 3,500 | 4 | 27 | edp | Jasmine Noir.jpg |
| 5 | Pure Oud Attar | 6,500 | 5 | 63 | attar | Pure Oud Attar.jpg |
| 6 | Musk Al Tahara | 2,800 | 5 | 48 | attar | Musk Al Tahara.jpg |
| 7 | Rose Maliki Attar | 4,200 | 4 | 35 | attar | Rose Maliki Attar.jpg |
| 8 | Amber Dehn | 3,200 | 4 | 22 | attar | Amber Dehn.jpg |
| 9 | Royal Oud Intense | 8,500 | 5 | 74 | oud | Royal Oud Intense.jpg |
| 10 | Oud Al Shams | 7,200 | 5 | 58 | oud | Oud Al Shams.jpg |
| 11 | Black Oud | 6,800 | 4 | 41 | oud | Black Oud.jpg |
| 12 | Oud & Saffron | 9,000 | 5 | 82 | oud | Oud Saffron.jpg |
| 13 | Black Musk Oriental | 3,600 | 5 | 39 | oriental | Black Musk Oriental.jpg |
| 14 | Spice Route | 4,100 | 4 | 29 | oriental | Spice Route.jpg |
| 15 | Desert Dusk | 3,900 | 5 | 44 | oriental | Desert Dusk.jpg |
| 16 | Luxury Gift Set – Gold | 12,500 | 5 | 67 | giftsets | Gift Set Gold.jpg |
| 17 | Oud Trio Gift Box | 15,000 | 5 | 53 | giftsets | Oud Trio Gift Box.jpg |
| 18 | Attar Discovery Set | 8,800 | 4 | 38 | giftsets | Attar Discovery Set.jpg |

### Product Detail Data (for Modal):

Each product must include:
1. **Full Description:** 3–4 sentences describing olfactory notes, mood, and occasion
2. **Top / Middle / Base Notes:** (e.g., Top: Bergamot, Saffron | Middle: Rose, Oud | Base: Musk, Amber)
3. **Concentration:** EDP / Attar / Parfum
4. **Volume Options:** 30ml / 50ml / 100ml (for EDPs) or 6ml / 12ml (for Attars)
5. **Longevity:** e.g., "8–10 hours"
6. **Sillage:** e.g., "Heavy projection"

---

## 🎭 Interactive Modals (2-Screen Flow)

### Modal 1 – Product Detail

**Trigger:** Click anywhere on product card or "Add to Cart" button

**Layout (desktop: 2 columns; mobile: stacked):**

```
┌──────────────────────────────────────────────────────┐
│  [X Close]                                           │
│                                                      │
│  [Product Image]    │  Brand Name: HARIS FRAGRANCE   │
│   (Left Column)     │  Product: Rose Oud Intense     │
│                     │  ★★★★★ 42 Reviews              │
│                     │                                │
│                     │  [Top Notes] [Mid] [Base]      │
│                     │  (clickable tabs or display)   │
│                     │                                │
│                     │  Concentration: Eau de Parfum  │
│                     │  Longevity: 8–10 hours         │
│                     │                                │
│                     │  Volume: [30ml] [50ml✓] [100ml]│
│                     │  (toggle selector, PKR updates) │
│                     │                                │
│                     │  Qty: [ − ] [ 1 ] [ + ]        │
│                     │                                │
│                     │  PKR 4,500                     │
│                     │  [Proceed to Checkout ▶]       │
│                     │  [♡ Add to Wishlist]            │
└──────────────────────────────────────────────────────┘
```

**Details:**
- Volume selector dynamically updates price
- Quantity `+/−` buttons update subtotal in real-time
- Smooth modal open animation: fade-in + scale from 95% to 100%
- Close on: X button, backdrop click, or ESC key
- Scroll-lock on `<body>` when modal is open

---

### Modal 2 – Checkout

**Trigger:** "Proceed to Checkout" button in Product Detail Modal

**Transition:** Previous modal slides out-left → Checkout modal slides in-right (or cross-fade)

```
┌──────────────────────────────────────────────────────┐
│  ← Back    SECURE CHECKOUT    [Haris Fragrance Logo] │
├──────────────────────────────────────────────────────┤
│  ORDER SUMMARY                                       │
│  ─────────────────────────────────────────────────   │
│  Rose Oud Intense (50ml) × 2 ............. PKR 9,000 │
│  Delivery ................................. PKR 200   │
│  ─────────────────────────────────────────────────   │
│  TOTAL .................................... PKR 9,200 │
├──────────────────────────────────────────────────────┤
│  DELIVERY DETAILS                                    │
│                                                      │
│  Full Name *         [________________________]      │
│  Phone Number *      [________________________]      │
│  City *              [________________________]      │
│  Delivery Address *  [________________________]      │
│  Special Notes       [________________________]      │
│                                                      │
│  [📱 Send Order via WhatsApp  →]  ← Green button    │
│  🔒 Secure & Confidential                            │
└──────────────────────────────────────────────────────┘
```

**WhatsApp Message Format (URL-encoded):**
```
Hello Haris Fragrance! 🌸

🛍 *New Order Request*
━━━━━━━━━━━━━━━━━━━━
Product: Rose Oud Intense (50ml EDP)
Quantity: 2
Unit Price: PKR 4,500
Subtotal: PKR 9,000
Delivery: PKR 200
*Total: PKR 9,200*
━━━━━━━━━━━━━━━━━━━━
👤 Customer Details:
Name: [Full Name]
Phone: [Phone Number]
City: [City]
Address: [Delivery Address]
Notes: [Special Notes]
━━━━━━━━━━━━━━━━━━━━
Please confirm my order. Thank you!
```

**WhatsApp Number:** `923XXXXXXXXX` *(replace with real number)*

---

## 🎞 Animations & Motion Design

### Scroll Animations (Intersection Observer API)
| Element | Animation |
|---|---|
| Section headings | Fade-up from `translateY(30px)` |
| Product cards | Staggered fade-in with `animation-delay` (50ms per card) |
| Feature cards | Slide-in from alternating sides |
| Images | Fade-in with subtle scale from 0.95 → 1 |

### Hover & Micro-Interactions
| Element | Interaction |
|---|---|
| Product cards | `translateY(-8px)` + gold `box-shadow` glow |
| Product images | `scale(1.08)` with `overflow: hidden` |
| Nav links | Gold underline slides from left via `::after` |
| CTA buttons | `translateY(-3px)` + shimmer sweep animation |
| WhatsApp button | Subtle `box-shadow` pulse (`@keyframes pulse`) |
| Category cards | Overlay reveals "Explore" text on hover |
| Star ratings | Individual star fills on hover (optional) |

### Page Load Sequence
```
0ms    → Navbar fades in (Cinzel logo from left)
200ms  → Hero text letter-by-letter reveal (CSS `animation-delay`)
600ms  → Hero subheading fades up
900ms  → CTA buttons slide up
1200ms → Scroll indicator bounces in
```

---

## 📁 Required Image Files (All Local)

Place all files in the **same directory** as `index.html`:

```
index.html
├── logo.png
├── hero.jpg
├── Our Story.jpg
├── Our Promise.jpg
│
├── EDP Collection.jpg
├── Attar Collection.jpg
├── Oud Collection.jpg
├── Oriental Collection.jpg
├── Gift Sets.jpg
│
├── Rose Oud Intense.jpg
├── Velvet Musk.jpg
├── Midnight Amber.jpg
├── Jasmine Noir.jpg
├── Pure Oud Attar.jpg
├── Musk Al Tahara.jpg
├── Rose Maliki Attar.jpg
├── Amber Dehn.jpg
├── Royal Oud Intense.jpg
├── Oud Al Shams.jpg
├── Black Oud.jpg
├── Oud Saffron.jpg
├── Black Musk Oriental.jpg
├── Spice Route.jpg
├── Desert Dusk.jpg
├── Gift Set Gold.jpg
├── Oud Trio Gift Box.jpg
├── Attar Discovery Set.jpg
│
├── Royal Oud Bestseller.jpg
├── Rose Elixir Bestseller.jpg
└── Black Musk Bestseller.jpg
```

> ⚠️ **Important:** All `<img>` `src` attributes must use these **exact filenames**. No external URLs, no placeholder services (Unsplash, Picsum, etc.).

---

## ⚙️ Technical Architecture

### CDN Dependencies (in `<head>`)
```html
<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Remix Icons -->
<link href="https://cdn.jsdelivr.net/npm/remixicon@4.3.0/fonts/remixicon.css" rel="stylesheet">

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=Cinzel:wght@400;600;700&family=Jost:wght@300;400;500&family=Dancing+Script:wght@600&display=swap" rel="stylesheet">
```

### Tailwind Config Extension
```javascript
tailwind.config = {
  theme: {
    extend: {
      colors: {
        'brand-bg':     '#0D0A07',
        'brand-card':   '#211A13',
        'brand-gold':   '#C9A84C',
        'brand-gold-light': '#E8C97A',
        'brand-cream':  '#F2EAD8',
        'brand-muted':  '#A89880',
        'brand-rose':   '#8C3A50',
      },
      fontFamily: {
        'cinzel':      ['Cinzel', 'serif'],
        'cormorant':   ['Cormorant Garamond', 'serif'],
        'jost':        ['Jost', 'sans-serif'],
        'dancing':     ['Dancing Script', 'cursive'],
      }
    }
  }
}
```

### Single-File Architecture Rule

**Everything — HTML structure, `<style>` block, `<script>` block — lives inside `index.html`.**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- CDN links only (Tailwind, Remix Icons, Google Fonts) -->
  <style>
    /* ALL custom CSS here — animations, keyframes, glass-morphism, etc. */
    :root { /* All CSS variables */ }
    @keyframes cardFadeOut { ... }
    @keyframes shimmer { ... }
    /* etc. */
  </style>
</head>
<body>
  <!-- All HTML sections here -->

  <script>
    // ALL JavaScript here — wrapped in one IIFE or clearly separated module blocks
    // Product data array, filter logic, modal system, WhatsApp handler, scroll animations
    tailwind.config = { ... }; // Tailwind config also goes here
  </script>
</body>
</html>
```

> ⛔ Do NOT create `styles.css`, `app.js`, `products.js`, or any other external file. The deliverable is a single `index.html` that works when opened directly in a browser — no build step, no server required.

### JavaScript Modules (Vanilla JS – No Libraries)
```
├── State Management (top of <script>)
│   ├── const PRODUCTS = [ ...all 18 products as JS objects... ]
│   ├── let currentFilter = 'all'
│   ├── let activeProduct = null
│   └── let cartItems = []
│
├── Navigation Module
│   ├── Sticky scroll behavior → glass-morphism class toggle at 80px
│   ├── Active link highlighting via IntersectionObserver on sections
│   ├── Mobile hamburger drawer toggle (open/close/ARIA)
│   └── Smooth scroll to sections on nav link click
│
├── Product Filter Module (SHARED between Section 5 cards + Section 6 buttons)
│   ├── filterProducts(category) ← ONE function, called by ALL filter surfaces
│   ├── renderFilteredProducts(category) ← handles show/hide + stagger animation
│   └── Active state sync across both filter buttons AND category cards
│
├── Modal System (TWO-SCREEN FLOW)
│   ├── openProductModal(productId) ← injects product data, shows Screen 1
│   ├── openCheckoutModal() ← transitions from Screen 1 to Screen 2
│   ├── closeModal() ← handles both modals, restores body scroll
│   ├── Volume selector → dynamic price recalculation
│   ├── Quantity +/− → subtotal update
│   ├── Body scroll lock (overflow: hidden on <body> when modal open)
│   └── ESC key listener + backdrop click → closeModal()
│
├── WhatsApp Integration
│   ├── buildWhatsAppMessage(product, volume, qty, customerData) → formatted string
│   ├── encodeForWhatsApp(message) → encodeURIComponent
│   └── window.open('https://wa.me/923XXXXXXXXX?text=' + encoded)
│
├── Card Animation Module
│   ├── removeCardWithAnimation(element) ← always use, never .remove() directly
│   └── addCardEntrance(element, delay) ← for staggered grid load
│
└── Scroll Animation Module
    ├── IntersectionObserver on all [data-animate] elements
    ├── Adds 'in-view' class when element enters viewport
    └── CSS handles the actual animation via .in-view selector
```

---

## 📐 Responsive Breakpoints

| Breakpoint | Layout Rules |
|---|---|
| `< 640px` (Mobile) | 1-column product grid, full-width cards, hamburger nav, stacked sections |
| `640–1024px` (Tablet) | 2-column product grid, 2×2 feature grid, side-by-side about section |
| `> 1024px` (Desktop) | 3-column grid, 5-column categories, 4-column features, full navbar |

**Container:** `max-width: 1280px`, centered with `mx-auto px-4 sm:px-6 lg:px-8`

---

## 🏁 Definition of Done (Acceptance Checklist)

A build is considered **complete and deliverable** only when ALL of the following pass:

### 🗂 Single File & Structure
- [ ] The entire website is one `index.html` file — no external `.css` or `.js` files
- [ ] File opens correctly in Chrome by double-clicking (no localhost required)
- [ ] Zero console errors or warnings in Chrome DevTools

### 🖥 Layout & Responsiveness
- [ ] Renders perfectly at **1920×1080** (primary target — check this first)
- [ ] Renders correctly at 1440px, 1024px, 768px, and 375px
- [ ] All sections render correctly at 320px minimum width
- [ ] No horizontal scrollbar on any screen width
- [ ] Product grid: 3 columns at 1080p, 2 at tablet, 1 at mobile

### 🎨 Color System
- [ ] All 5 category color tokens are defined **once** in `:root` CSS variables
- [ ] Filter bar active button color **matches** the category showcase card accent color for that category — pixel-for-pixel the same token
- [ ] Product card category badge uses the same token as its filter button and category card
- [ ] No hardcoded hex color appears more than once for the same category anywhere in the file

### 🛍 Product Grid
- [ ] All 18 products are rendered and visible on first page load (no empty state)
- [ ] `filterProducts('all')` is called on `DOMContentLoaded`
- [ ] No "loading..." messages, no skeleton screens — cards render synchronously

### 🔽 Filter System (Critical — Test Thoroughly)
- [ ] Clicking a **filter bar button** filters the product grid AND highlights the matching category card
- [ ] Clicking a **category showcase card** filters the product grid AND activates the matching filter button
- [ ] Both surfaces call the **identical** `filterProducts()` function — confirmed by code review
- [ ] Filtered products animate out (fade + slide) and back in (staggered) — no instant show/hide
- [ ] "All Products" filter shows all 18 cards
- [ ] Each category filter shows only its products (count verified against product data table)
- [ ] Filter state is visually obvious — active button/card is always clearly highlighted

### 💫 Animations
- [ ] Section elements fade in on scroll entry (Intersection Observer)
- [ ] Product cards fade in with stagger on filter change (40ms delay per card)
- [ ] Any card removal uses `cardFadeOut` animation — DOM element removed only AFTER animation ends
- [ ] No card ever disappears from the DOM without animating first
- [ ] Hero letter-by-letter text reveal plays on page load
- [ ] Hover effects work on all cards, buttons, nav links

### 🎭 Modal System
- [ ] Clicking a product card → opens **Product Detail Modal** (Screen 1 only)
- [ ] "Proceed to Checkout" in Screen 1 → transitions to **Checkout Modal** (Screen 2)
- [ ] Clicking a product card NEVER goes directly to Checkout — always Screen 1 first
- [ ] Volume selector updates price in real-time
- [ ] Quantity +/− updates subtotal in real-time
- [ ] ESC key closes whichever modal is open
- [ ] Clicking backdrop closes modal
- [ ] `<body>` scroll is locked when any modal is open, restored on close
- [ ] Modal close animation plays before modal is hidden

### 📱 WhatsApp Integration
- [ ] "Send Order via WhatsApp" opens `wa.me` link with pre-filled message
- [ ] Message includes: product name, volume, quantity, unit price, subtotal, delivery charge, total, customer name, phone, city, address, and notes
- [ ] WhatsApp number is set (not placeholder `923XXXXXXXXX`)
- [ ] Floating WhatsApp button is fixed bottom-right, visible on all sections, all screen sizes

### 🖼 Images
- [ ] All `<img>` src values use exact local filenames from the image files list
- [ ] Zero external image URLs in the HTML file
- [ ] `onerror` fallback on every `<img>` shows a dark placeholder with product name
- [ ] Images below the fold have `loading="lazy"`

### ✨ Quality Bar
- [ ] Page feels premium, dark, and luxurious — consistent with the brand brief
- [ ] Lighthouse Performance ≥ 85 (desktop)
- [ ] All interactive elements have `aria-label` for accessibility
- [ ] `<title>` and `<meta name="description">` are set with brand-appropriate copy

---

## 💡 Senior Developer Notes & Common Pitfalls

1. **Single File is Non-Negotiable.** If you feel tempted to create a second file, put it inside the `<script>` or `<style>` block instead. The client opens `index.html` — that's it.

2. **1080p First.** Do not design for 1440px or 1280px and then scale down. Open DevTools, set viewport to exactly 1920×1080, and design for that. Then add responsive breakpoints downward.

3. **Category Color Drift is the #1 Bug.** The most common mistake is defining `#C9A84C` in the filter button, then separately defining `#C9A84C` in the category card, and then they get out of sync when one is updated. Use `var(--cat-edp)` everywhere. One token, zero drift.

4. **Filter Sync Must Be Architectural, Not Patched.** Do not write two separate event listener blocks — one for category cards and one for filter buttons — that try to stay in sync. Write ONE `filterProducts(category)` function and call it from both. Any other approach will introduce bugs.

5. **Always Pre-Populate.** Call `filterProducts('all')` or `renderProducts()` inside `DOMContentLoaded`. The page must never load to an empty product grid, even for a frame.

6. **Card Removal Animation — Never Skip It.** Even if removal feels rare, any UI that removes a card from DOM must use `removeCardWithAnimation()`. Direct `.remove()` calls are banned. The animation takes 380ms — it's worth it.

7. **Image Fallback is Required.** Every `<img>` tag must have:
   ```html
   <img src="Product Name.jpg" alt="Product Name"
        onerror="this.onerror=null; this.src=''; this.closest('.product-card').querySelector('.img-fallback').style.display='flex';">
   ```
   Pair with a hidden `.img-fallback` div inside each card that shows on error.

8. **Modal Body Scroll Lock — Both Directions.** On modal open: `document.body.style.overflow = 'hidden'`. On modal close: `document.body.style.overflow = ''`. If you forget the restore, the page becomes unscrollable after the first modal interaction.

9. **WhatsApp Button Z-Index.** The floating WhatsApp button must have `z-index: 9999`. If the mobile nav drawer or any modal overlaps it, the drawer/modal should be `z-index: 1000` and the button `z-index: 9999` so it stays on top. (Exception: when a modal is open, it's acceptable for the button to be behind the modal overlay.)

10. **Zero Console Errors is a Hard Requirement.** Before delivery, open DevTools → Console, reload the page, interact with every feature (filter all categories, open 3 different product modals, complete a checkout form). The console must be clean.

---

*Built for Haris Fragrance — Karachi, Pakistan | Version 2.0 Build Spec*
