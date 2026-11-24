---
name: apple-style-web
description: Build websites and web pages in a clean, premium product-showcase style inspired by the Apple website — big bold headlines, generous whitespace, full-width alternating light/dark sections, product-first imagery, frosted-glass nav, pill buttons and subtle scroll animations. Use whenever the user asks to build, design, restyle or polish a website, landing page, product page or HTML/CSS/React UI, unless they ask for a different style.
---

# Apple-style Web Design

Make every page feel calm, premium and product-focused. The content is the hero; the UI gets out of the way.
Mimic the **design language only** — never copy Apple's logo, product names, photos, copy text or trademarks, and never present the site as Apple's.

## Core principles

1. **Whitespace is the design.** Big vertical padding, few elements per screen, one message per section.
2. **Typography leads.** Huge, tight, bold headlines; short subheads; small amount of body text.
3. **Product/visual first.** Each section = headline + one-line tagline + 1–2 links/buttons + one large image or visual.
4. **Restraint in color.** Near-white / near-black backgrounds, one accent blue for links and buttons.
5. **Centered and symmetric** for hero/showcase sections; grid of cards for feature lists.
6. **Subtle motion.** Fade/slide-up on scroll, gentle hover scale. Never flashy.

## Design tokens (always start with these)

```css
:root {
  /* Colors */
  --bg: #ffffff;
  --bg-alt: #f5f5f7;          /* light grey section / card */
  --bg-dark: #000000;
  --bg-dark-alt: #1d1d1f;
  --text: #1d1d1f;            /* primary text, never pure black */
  --text-secondary: #6e6e73;
  --text-on-dark: #f5f5f7;
  --text-on-dark-secondary: #86868b;
  --accent: #0071e3;          /* buttons */
  --link: #0066cc;            /* text links on light */
  --link-on-dark: #2997ff;    /* text links on dark */
  --border: #d2d2d7;

  /* Type */
  --font: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text",
          "Helvetica Neue", "Inter", "Segoe UI", Arial, sans-serif;

  /* Layout */
  --max-w: 980px;             /* text/content width */
  --max-w-wide: 1440px;       /* grids / tiles */
  --nav-h: 44px;
  --radius-card: 18px;
  --radius-tile: 28px;
  --radius-pill: 980px;

  /* Motion */
  --ease: cubic-bezier(0.28, 0.11, 0.32, 1);
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #000000;
    --bg-alt: #1d1d1f;
    --text: #f5f5f7;
    --text-secondary: #86868b;
    --link: #2997ff;
    --border: #424245;
  }
}

* { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--font);
  background: var(--bg);
  color: var(--text);
  font-size: 17px;
  line-height: 1.47;
  letter-spacing: -0.022em;
  -webkit-font-smoothing: antialiased;
}
img { max-width: 100%; display: block; }
```

## Typography scale

| Role | Size (desktop → mobile) | Weight | Line-height | Letter-spacing |
|---|---|---|---|---|
| Hero headline | 80px → 48px | 700 | 1.05 | -0.015em |
| Section headline | 56px → 40px | 600 | 1.07 | -0.005em |
| Tile headline | 40px → 32px | 600 | 1.1 | 0 |
| Subhead / tagline | 28px → 21px | 400 | 1.14 | 0.007em |
| Body | 17px | 400 | 1.47 | -0.022em |
| Small / footnote | 12px | 400 | 1.33 | -0.01em |

Use `clamp()` for fluid sizes, e.g. `font-size: clamp(48px, 7vw, 80px);`.
Optional gradient headline for "pro" sections:
`background: linear-gradient(90deg,#2997ff,#a259ff,#ff4f8b); -webkit-background-clip:text; color:transparent;`

## Components

### Global nav (frosted glass, sticky)
```css
.nav {
  position: sticky; top: 0; z-index: 100; height: var(--nav-h);
  background: rgba(255,255,255,0.8);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-bottom: 1px solid rgba(0,0,0,0.08);
}
.nav-inner { max-width: var(--max-w); margin: 0 auto; height: 100%;
  display: flex; align-items: center; justify-content: space-between; padding: 0 22px; }
.nav a { font-size: 12px; color: var(--text); opacity: .8; text-decoration: none; transition: opacity .3s; }
.nav a:hover { opacity: 1; }
```
Dark version: `background: rgba(22,22,23,0.8)`. Logo on the left (simple icon or wordmark of the user's brand), 6–8 small links centered, icons (search/bag/menu) right. On mobile (<834px) collapse links into a hamburger that opens a full-screen overlay with large 28px links.

### Optional sub-nav
Second sticky bar under the nav: product name (21px, 600) left, small links + small pill "Buy"/"Get started" button right.

### Buttons & links
```css
.btn { display: inline-block; padding: 11px 22px; border-radius: var(--radius-pill);
  background: var(--accent); color: #fff; font-size: 17px; text-decoration: none;
  transition: background .3s var(--ease); }
.btn:hover { background: #0077ed; }
.btn-secondary { background: transparent; color: var(--accent); border: 1px solid var(--accent); }
.btn-secondary:hover { background: var(--accent); color: #fff; }
.link { color: var(--link); text-decoration: none; }
.link:hover { text-decoration: underline; }
.link::after { content: " ›"; }          /* chevron "Learn more ›" */
```
Pair them: primary "Buy / Get started" + secondary "Learn more ›".

### Hero section
```html
<section class="hero">
  <h1>Product Name</h1>
  <p class="tagline">One short, confident sentence.</p>
  <div class="ctas"><a class="btn" href="#">Get started</a><a class="link" href="#">Learn more</a></div>
  <img src="hero.png" alt="">
</section>
```
```css
.hero { text-align: center; padding: 96px 22px 0; overflow: hidden; }
.hero h1 { font-size: clamp(48px,7vw,80px); font-weight: 700; line-height: 1.05; }
.hero .tagline { font-size: clamp(21px,3vw,28px); color: var(--text-secondary); margin-top: 8px; }
.hero .ctas { display: flex; gap: 24px; justify-content: center; align-items: center; margin-top: 24px; }
.hero img { margin: 48px auto 0; max-width: 1200px; width: 100%; }
```

### Alternating full-width sections
Stack sections that alternate `--bg` / `--bg-alt` / `--bg-dark`. Each: `padding: 120px 22px;` (mobile `80px 16px`), centered text, image below. A dark section uses `--text-on-dark` and `--link-on-dark`.

### Promo tile grid (homepage style)
```css
.tiles { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; padding: 12px; }
.tile { background: var(--bg-alt); border-radius: 0; min-height: 580px; text-align: center;
  padding: 52px 22px 0; overflow: hidden; display: flex; flex-direction: column; align-items: center; }
.tile img { margin-top: auto; }
@media (max-width: 834px) { .tiles { grid-template-columns: 1fr; } .tile { min-height: 480px; } }
```

### Feature cards
```css
.cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px; max-width: var(--max-w-wide); margin: 0 auto; padding: 0 22px; }
.card { background: var(--bg-alt); border-radius: var(--radius-tile); padding: 40px;
  transition: transform .4s var(--ease), box-shadow .4s var(--ease); }
.card:hover { transform: scale(1.02); box-shadow: 0 8px 30px rgba(0,0,0,.08); }
.card h3 { font-size: 28px; font-weight: 600; line-height: 1.14; }
.card p  { color: var(--text-secondary); margin-top: 8px; }
```
Variant: horizontal scrolling card gallery (`display:flex; overflow-x:auto; scroll-snap-type:x mandatory;` with `scroll-snap-align:start`, hidden scrollbar, round arrow buttons bottom-right).

### Big-number / spec highlights
Large stat (`font-size: 64px; font-weight: 700;`) with a small caption underneath in `--text-secondary`, arranged in a 3–4 column grid.

### Footer
Background `--bg-alt`, all text 12px, `--text-secondary`. Top: small footnotes. Middle: 5 columns of link lists with bold 12px column titles (collapse into accordions on mobile). Bottom: thin `--border` line, copyright left, legal links right separated by `|`.

## Motion

```css
.reveal { opacity: 0; transform: translateY(40px);
  transition: opacity 1s var(--ease), transform 1s var(--ease); }
.reveal.in { opacity: 1; transform: none; }
@media (prefers-reduced-motion: reduce) { .reveal { opacity: 1; transform: none; transition: none; } }
```
```js
const io = new IntersectionObserver(es => es.forEach(e => {
  if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); }
}), { threshold: 0.15 });
document.querySelectorAll('.reveal').forEach(el => io.observe(el));
```
Optional extras (use sparingly): hero image scale-in on scroll, sticky "scrollytelling" section where text changes while an image stays pinned, autoplay muted looping video with a small round pause button.

## Copywriting style
- Headlines: 1–4 words. Confident, often a pun or short phrase ("Fast. Really fast.").
- Taglines: one sentence, no jargon.
- Lots of periods for rhythm. Avoid exclamation marks.
- CTAs: "Learn more", "Buy", "Get started", "Watch the film".

## Responsive rules
- Breakpoints: 1068px (large → medium), 734px (medium → small).
- Side gutter 22px desktop, 16px mobile; no horizontal page scroll.
- Hero headline never below 40px; images scale to full width.
- Touch targets ≥ 44px.

## Accessibility
- Contrast ≥ 4.5:1 (secondary grey `#6e6e73` on white passes; don't go lighter for body text).
- Real `<nav>`, `<main>`, `<section>`, `<footer>`, alt text on images, visible `:focus-visible` ring (`outline: 2px solid var(--accent); outline-offset: 2px;`).
- Respect `prefers-reduced-motion` and `prefers-color-scheme`.

## Checklist before finishing
- [ ] Design tokens defined and used (no random hex values).
- [ ] Sticky frosted nav + clean footer.
- [ ] Hero: huge headline, tagline, two CTAs, big visual.
- [ ] Sections alternate backgrounds with generous padding.
- [ ] Pill buttons, "Learn more ›" links, rounded cards.
- [ ] Scroll reveal animation with reduced-motion fallback.
- [ ] Works at 375px width.
- [ ] No Apple logos, product names, images or copied text.
