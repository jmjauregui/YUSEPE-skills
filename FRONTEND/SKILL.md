---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality. Use this skill when the user asks to build web components, pages, artifacts, posters, or applications (examples include websites, landing pages, dashboards, React components, HTML/CSS layouts, or when styling/beautifying any web UI). Generates creative, polished code and UI design that avoids generic AI aesthetics. ALWAYS use this skill before writing any frontend code — even for "quick" or "simple" UI requests — since these guidelines prevent the most common failure modes.
install: npx skills add emilkowalski/skill
license: Complete terms in LICENSE.txt
---

# Frontend Design Skill

This skill guides creation of distinctive, production-grade frontend interfaces. The #1 failure mode is **defaulting to generic patterns** — safe fonts, safe colors, safe layouts. This skill exists to prevent that.

---

## Step 1: Commit to an Aesthetic Direction (BEFORE writing code)

Answer these explicitly before touching code:

1. **Tone**: What is the single word that defines this design? (e.g. *austere*, *frenetic*, *warm*, *cold*, *playful*, *brutal*, *lush*)
2. **One unforgettable thing**: What will the user remember? A color? A font? A motion? A texture?
3. **Light or dark?** Don't default — choose based on emotional fit.
4. **Font pair**: Name both fonts before writing CSS. If you can't name them, you haven't decided yet.

**Then execute that direction without retreating to safe choices.**

---

## Step 2: Typography — The Most Common Failure Point

Generic fonts are the #1 sign of a generic design. Do NOT use:
- `Inter`, `Roboto`, `Arial`, `Helvetica`, `system-ui`, `-apple-system`, `Space Grotesk`, `DM Sans`

**Instead, load from Google Fonts or use web-safe alternatives with personality.** Examples of distinctive pairings:

| Display | Body | Mood |
|---|---|---|
| `Playfair Display` | `Crimson Pro` | Editorial, literary |
| `Bebas Neue` | `Barlow` | Industrial, bold |
| `Cormorant Garamond` | `Jost` | Luxury, refined |
| `Syne` | `Epilogue` | Contemporary, geometric |
| `Fraunces` | `Source Serif 4` | Warm, expressive |
| `Cabinet Grotesk` | `Satoshi` | Modern, clean (use sparingly) |
| `Unbounded` | `Space Mono` | Techy, futuristic |
| `Libre Baskerville` | `Libre Franklin` | Classic, trustworthy |

**Always import fonts at the top of the file.** For HTML:
```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Crimson+Pro:ital,wght@0,400;1,400&display=swap" rel="stylesheet">
```

**Size scale** — use a real type scale, not arbitrary px values:
```css
--text-xs: clamp(0.75rem, 1.5vw, 0.875rem);
--text-sm: clamp(0.875rem, 1.75vw, 1rem);
--text-base: clamp(1rem, 2vw, 1.125rem);
--text-lg: clamp(1.125rem, 2.5vw, 1.25rem);
--text-xl: clamp(1.25rem, 3vw, 1.5rem);
--text-2xl: clamp(1.5rem, 4vw, 2rem);
--text-3xl: clamp(2rem, 5vw, 3rem);
--text-4xl: clamp(2.5rem, 7vw, 4.5rem);
```

---

## Step 3: Color — Commit, Don't Hedge

**Anti-patterns to avoid:**
- Purple/violet gradients on white (`#6366f1`, `#8b5cf6` — the default AI palette)
- "Safe" blue-and-white corporate palettes
- Too many colors (5+ colors = no colors)
- Pure `#ffffff` backgrounds and `#000000` text without intention

**Patterns that work:**
```css
/* Dark: High-contrast industrial */
--bg: #0a0a0a;
--surface: #141414;
--border: #2a2a2a;
--text: #e8e8e8;
--accent: #c8f400;  /* one electric accent */

/* Light: Warm editorial */
--bg: #f5f0e8;
--surface: #fffdf7;
--border: #d4cabb;
--text: #1a1612;
--accent: #c0392b;  /* one strong accent */

/* Dark: Ocean depth */
--bg: #0d1b2a;
--surface: #1b2d3e;
--border: #2c4a5e;
--text: #cdd9e0;
--accent: #4fc3f7;
```

**Rule**: Pick 1 accent color and use it with discipline. It should appear on ~10-20% of elements, not everywhere.

---

## Step 4: Layout — Break the Grid

Generic layouts are symmetric, centered, and predictable. Good layouts are not.

**Techniques that create visual interest:**
```css
/* Overlapping elements */
.hero-text { position: relative; z-index: 2; margin-bottom: -3rem; }
.hero-image { position: relative; z-index: 1; }

/* Asymmetric grid */
.layout { display: grid; grid-template-columns: 1fr 2.5fr; gap: 0; }

/* Bleed elements */
.full-bleed { width: 100vw; margin-left: calc(-50vw + 50%); }

/* Diagonal section break */
.section::after {
  content: '';
  display: block;
  height: 80px;
  background: var(--next-section-bg);
  clip-path: polygon(0 100%, 100% 0, 100% 100%);
}
```

**Spatial density**: Choose deliberately — either generous whitespace (padding: 8rem+) or controlled density (tight grid, small gaps). Never end up in the middle by accident.

---

## Step 5: Backgrounds and Atmosphere

Never use a plain `background-color` as the final state. Add depth:

```css
/* Noise texture overlay */
.bg-textured::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
  pointer-events: none;
  z-index: 0;
}

/* Gradient mesh */
background: 
  radial-gradient(ellipse at 20% 50%, rgba(120,40,200,0.15) 0%, transparent 50%),
  radial-gradient(ellipse at 80% 20%, rgba(40,200,120,0.1) 0%, transparent 50%),
  var(--bg);

/* Subtle grid lines */
background-image: 
  linear-gradient(rgba(255,255,255,.03) 1px, transparent 1px),
  linear-gradient(90deg, rgba(255,255,255,.03) 1px, transparent 1px);
background-size: 40px 40px;
```

---

## Step 6: Motion — Purposeful, Not Decorative

**Page load**: One orchestrated entrance sequence is better than random scattered animations.

```css
/* Staggered reveal pattern */
.reveal { opacity: 0; transform: translateY(24px); animation: reveal 0.6s ease forwards; }
.reveal:nth-child(1) { animation-delay: 0.1s; }
.reveal:nth-child(2) { animation-delay: 0.2s; }
.reveal:nth-child(3) { animation-delay: 0.3s; }

@keyframes reveal {
  to { opacity: 1; transform: translateY(0); }
}
```

**Hover states**: Should feel intentional, not default.
```css
.card {
  transition: transform 0.25s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.25s ease;
}
.card:hover {
  transform: translateY(-4px) scale(1.01);
  box-shadow: 0 20px 60px rgba(0,0,0,0.3);
}
```

**Rules**:
- Use `cubic-bezier` instead of `ease` or `linear` for character
- Duration: UI feedback = 150-200ms, reveals = 400-700ms, ambient = 2000ms+
- Respect `prefers-reduced-motion`:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

---

## Step 7: Accessibility — Non-Negotiable Minimums

Even in creative designs, these must pass:

```css
/* Focus styles — never remove, always style */
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}

/* Minimum contrast: 4.5:1 for text, 3:1 for large text */
/* Test with: https://webaim.org/resources/contrastchecker/ */

/* Don't rely on color alone to convey meaning */
.error { color: var(--red); border-left: 3px solid var(--red); } /* + icon */
```

**Semantic HTML**:
```html
<!-- Use real elements, not divs with onclick -->
<button> not <div class="btn">
<nav> not <div class="navigation">
<main>, <header>, <footer>, <section>, <article>
<!-- Add aria-label for icon-only buttons -->
<button aria-label="Close menu">✕</button>
```

---

## Step 8: Performance Patterns

```css
/* Use CSS custom properties for theming — no JS needed */
:root { --color-accent: #c8f400; }

/* Prefer transforms over position for animation (GPU-accelerated) */
/* ✓ */ transform: translateX(10px);
/* ✗ */ left: 10px;

/* Contain layout/paint for complex animated elements */
.animated-card { contain: layout style; will-change: transform; }
```

For images in HTML artifacts:
- Use `loading="lazy"` on below-fold images
- Set explicit `width` and `height` to prevent layout shift
- Prefer CSS gradients and SVG over raster images when possible

---

## Step 9: Real Images — Never Design Without Them

**People buy from people. Placeholder images and grey boxes destroy trust and conversions.** Any web design with people, products, places, or lifestyle content MUST use real images.

### Always use Unsplash for real photos

Unsplash provides free, high-quality photos via a direct URL API — no API key needed.

**URL format:**
```
https://images.unsplash.com/photo-{PHOTO_ID}?w={WIDTH}&h={HEIGHT}&fit=crop&crop=faces,center&q=80
```

**Use `crop=faces` for portraits** — it automatically centers on faces.  
**Use `crop=center` for landscapes, products, food, architecture.**

### Curated photo IDs by category

Use these specific IDs — they are known to work and look professional:

**People / Portraits**
```
1534528-portrait-woman     → 1534528-YnB6kucaZaY  (professional woman)
1529429-portrait-man       → 1529429-x6WQeNkJ6Fo  (man portrait)
1438761-team               → 1438761-Zx1cJqGrBpQ  (team photo)
```

Use the pattern directly in `<img>` tags:
```html
<!-- Person, 400×400 square, face-cropped -->
<img 
  src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=400&h=400&fit=crop&crop=faces&q=80"
  alt="Team member portrait"
  width="400" height="400"
  loading="lazy"
/>
```

### Reliable photo URLs by use case

Copy these directly — they are tested and load correctly:

```html
<!-- HERO: Architecture / city -->
<img src="https://images.unsplash.com/photo-1486325212027-8081e485255e?w=1400&h=800&fit=crop&q=80" alt="..." />

<!-- HERO: People working / office -->
<img src="https://images.unsplash.com/photo-1522202176988-66273c2fd55f?w=1400&h=800&fit=crop&q=80" alt="..." />

<!-- HERO: Nature / landscape -->
<img src="https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=1400&h=800&fit=crop&q=80" alt="..." />

<!-- PERSON: Woman, professional -->
<img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=400&h=400&fit=crop&crop=faces&q=80" alt="..." />

<!-- PERSON: Man, professional -->
<img src="https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=400&h=400&fit=crop&crop=faces&q=80" alt="..." />

<!-- PERSON: Woman, casual -->
<img src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=400&h=400&fit=crop&crop=faces&q=80" alt="..." />

<!-- PERSON: Man, casual -->
<img src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=400&h=400&fit=crop&crop=faces&q=80" alt="..." />

<!-- TEAM: Group of people -->
<img src="https://images.unsplash.com/photo-1521737604893-d14cc237f11d?w=800&h=500&fit=crop&q=80" alt="..." />

<!-- PRODUCT: Coffee / food -->
<img src="https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?w=600&h=400&fit=crop&q=80" alt="..." />

<!-- PRODUCT: Tech / laptop -->
<img src="https://images.unsplash.com/photo-1496181133206-80ce9b88a853?w=600&h=400&fit=crop&q=80" alt="..." />

<!-- INTERIOR: Modern workspace -->
<img src="https://images.unsplash.com/photo-1497366216548-37526070297c?w=800&h=500&fit=crop&q=80" alt="..." />

<!-- INTERIOR: Restaurant / cafe -->
<img src="https://images.unsplash.com/photo-1555396273-367ea4eb4db5?w=800&h=500&fit=crop&q=80" alt="..." />

<!-- ABSTRACT: Texture / background -->
<img src="https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1200&h=800&fit=crop&q=80" alt="..." />
```

### For React components

```jsx
// Helper for consistent Unsplash URLs
const unsplash = (id, w = 800, h = 600, crop = 'center') =>
  `https://images.unsplash.com/photo-${id}?w=${w}&h=${h}&fit=crop&crop=${crop}&q=80`;

// Usage
<img src={unsplash('1534528741775-53994a69daeb', 400, 400, 'faces')} alt="Team member" />
```

### Rules for image usage

- **NEVER use placeholder.com, picsum, or grey boxes** — they signal "unfinished" to users
- **ALWAYS write descriptive `alt` text** — not "image" or "photo"
- **ALWAYS set `width` and `height`** — prevents layout shift
- **Match image content to context** — don't use a mountain photo for a SaaS product
- **For teams/testimonials**: use 3-6 different real face photos, never repeat the same person
- **For grids**: vary the crops (portrait, landscape, square) for visual rhythm

---

## Pre-Delivery Checklist

Before outputting the final code, verify each item:

- [ ] **Font**: Named a specific non-generic font pair. Google Fonts import is present.
- [ ] **Color**: Defined CSS custom properties. Not a purple gradient on white.
- [ ] **One memorable element**: Can name it (the oversized headline, the grain texture, the elastic hover, etc.)
- [ ] **Background**: More than a flat color — has texture, gradient, or visual depth.
- [ ] **Motion**: At least one entrance animation with `animation-delay` stagger. Has `prefers-reduced-motion`.
- [ ] **Focus styles**: `:focus-visible` is styled.
- [ ] **Semantic HTML**: No `div` used where a semantic element exists.
- [ ] **Images**: Any design with people/products/lifestyle uses real Unsplash photos. Zero placeholder boxes.
- [ ] **No Inter, no Roboto, no purple-on-white.**

If any item is unchecked, fix it before delivering.

---

## Anti-Examples vs Good Examples

### Typography
```css
/* ✗ GENERIC */
font-family: 'Inter', -apple-system, sans-serif;
font-size: 16px;

/* ✓ DISTINCTIVE */
font-family: 'Cormorant Garamond', Georgia, serif;
font-size: clamp(1rem, 2vw, 1.125rem);
letter-spacing: 0.01em;
```

### Color
```css
/* ✗ GENERIC (the AI default) */
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
color: white;

/* ✓ DISTINCTIVE */
--bg: #f2ede4;
--text: #1c1712;
--accent: #d4380d;
background: var(--bg);
color: var(--text);
```

### Buttons
```css
/* ✗ GENERIC */
.btn { background: #6366f1; color: white; border-radius: 8px; padding: 10px 20px; }

/* ✓ DISTINCTIVE (brutalist) */
.btn { background: var(--accent); color: var(--bg); border: 2px solid var(--text); 
       border-radius: 0; padding: 0.75rem 2rem; text-transform: uppercase; 
       letter-spacing: 0.1em; font-size: 0.75rem; font-weight: 700;
       transition: transform 0.15s, box-shadow 0.15s;
       box-shadow: 3px 3px 0 var(--text); }
.btn:hover { transform: translate(-2px, -2px); box-shadow: 5px 5px 0 var(--text); }
```

### Cards
```css
/* ✗ GENERIC */
.card { background: white; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 24px; }

/* ✓ DISTINCTIVE */
.card { background: var(--surface); border: 1px solid var(--border); 
        border-radius: 2px; padding: 2rem; position: relative;
        transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); }
.card::before { content: ''; position: absolute; inset: -1px; 
                background: var(--accent); z-index: -1; 
                transform: translate(4px, 4px); transition: transform 0.3s; }
.card:hover { transform: translate(-2px, -2px); }
.card:hover::before { transform: translate(6px, 6px); }
```
