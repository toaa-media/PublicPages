---
description: migrate a landing page to the Cyber-Premium design theme (based on SharpVault)
---

# 🎨 Page Theme Migration Workflow — Cyber-Premium

This workflow guides you through migrating any HTML landing page in the PublicPages project to the **Cyber-Premium** design system used by `sharp-vault/landing-page.html`. Follow every phase in order. Do not skip steps.

---

## Phase 0 — Preparation

### 0.1 Identify target & source
- **Source** (theme reference): `sharp-vault/landing-page.html`
- **Target** (page to migrate): the page specified by the user, e.g. `smartscan/landing-page.html`

### 0.2 Read the source theme
View the first 400 lines of the source to extract:
- `:root` CSS variables (colors, surfaces, borders)
- Font imports (`Outfit`, `Plus Jakarta Sans`)
- Global utilities (`.glass`, `.glass-pill`, `.btn-glow`, `.navbar`, background grid)
- Animation keyframes (`breathe`, `orbitSpin`, `glowPulse`, `phoneBob`, `chip1Float`, `chip2Float`, `scroll`)

### 0.3 Read the target page structure
View the **full** target page and note each section that exists:
- `<head>` / `<style>` block
- `<nav>` / navbar HTML
- `<section class="hero">` and phone mockup
- Features, How It Works, Tools
- Testimonials slider
- FAQ
- CTA / Download
- `<footer>`

---

## Phase 1 — CSS Variable & Foundation

### 1.1 Replace `:root` variables
Replace the entire `:root` block with the Cyber-Premium palette:

```css
:root {
    --bg: #030305;
    --bg-grid: rgba(255, 255, 255, 0.02);
    --surface: rgba(255, 255, 255, 0.04);
    --surface-hover: rgba(255, 255, 255, 0.07);
    --border: rgba(255, 255, 255, 0.08);
    --border-hover: rgba(255, 255, 255, 0.15);

    --primary: #00F0FF;
    --primary-glow: rgba(0, 240, 255, 0.2);
    --secondary: #7000FF;
    --accent: #FF0055;

    --text-main: #FFFFFF;
    --text-muted: #A1A1AA;
    --text-dark: #71717A;
}
```

### 1.2 Replace font imports
```html
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700;800&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### 1.3 Update `body` styles
```css
body {
    font-family: 'Plus Jakarta Sans', sans-serif;
    background-color: var(--bg);
    color: var(--text-main);
    line-height: 1.6;
    overflow-x: hidden;
    background-image:
        linear-gradient(var(--bg-grid) 1px, transparent 1px),
        linear-gradient(90deg, var(--bg-grid) 1px, transparent 1px);
    background-size: 40px 40px;
    background-position: center top;
    -webkit-font-smoothing: antialiased;
}

h1, h2, h3, h4, .font-display { font-family: 'Outfit', sans-serif; }
```

### 1.4 Remove legacy background elements
Delete any `liquid-canvas`, `liquid-orb`, `grain-overlay` divs and their CSS classes. They are replaced by the CSS grid background.

---

## Phase 2 — Global Utilities

Add the following utility classes, replacing any old equivalents:

### 2.1 `.glass` and `.glass-pill`
```css
.glass {
    background: var(--surface);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border: 1px solid var(--border);
}
.glass-pill {
    background: var(--surface);
    backdrop-filter: blur(16px);
    border: 1px solid var(--border);
    border-radius: 100px;
}
```

### 2.2 Button styles (`.btn`, `.btn-primary`, `.btn-secondary`, `.btn-glow`)
Copy the full button block from the reference (Phase 0.2).

### 2.3 Text gradient helpers
```css
.text-gradient { ... }          /* white → grey */
.text-gradient-primary { ... }  /* cyan → blue */
```
Always include both `-webkit-background-clip` and `background-clip` for linting compliance.

---

## Phase 3 — Navbar

Replace the existing navbar HTML with the **floating glass capsule** pattern:

```html
<nav class="navbar glass-pill" id="navbar">
    <a href="#" class="logo">
        <img src="[APP_ICON]" alt="[APP_NAME]" class="logo-icon" style="width: 44px; height: 44px; border-radius: 14px;">
        [APP_NAME]
    </a>
    <ul class="nav-links">
        <!-- nav links -->
    </ul>
    <a href="#download" class="btn btn-glow">Get Started</a>
</nav>
```

Add corresponding CSS:
- Fixed, top-centered, max-width 1000px, border-radius 100px
- `.navbar.scrolled` darkens the glass on scroll

Uncomment / add the scroll JS:
```js
const navbar = document.getElementById('navbar');
window.addEventListener('scroll', () => {
    navbar.classList.toggle('scrolled', window.scrollY > 60);
});
```

---

## Phase 4 — Hero Section

### 4.1 HTML structure
```html
<section class="hero">
    <div class="hero-blob"></div>
    <div class="hero-container">
        <div class="hero-content">
            <div class="hero-badge">[EMOJI] [TAGLINE]</div>
            <h1 class="hero-title">[HEADLINE]<br><span class="gradient">[GRADIENT WORD]</span></h1>
            <p class="hero-desc">[DESCRIPTION]</p>
            <div class="hero-actions">
                <a href="#download" class="btn btn-primary">Download Free</a>
                <a href="#features" class="btn btn-secondary">Explore Features</a>
            </div>
            <div class="hero-stats">
                <!-- stat items -->
            </div>
        </div>
        <div class="hero-visual">
            <div class="phone-orbit phone-orbit-1"></div>
            <div class="phone-orbit phone-orbit-2"></div>
            <div class="phone-glow"></div>
            <div class="phone">
                <div class="phone-screen">
                    <div class="phone-notch"></div>
                    <img src="[APP_SCREENSHOT]" alt="[APP_NAME] App" style="width: 100%; height: 100%; object-fit: cover;">
                </div>
            </div>
            <!-- 3 float-pill elements -->
        </div>
    </div>
</section>
```

### 4.2 CSS
- Copy `hero`, `hero-blob`, `hero-container`, `hero-badge`, `hero-title .gradient`, `hero-desc`, `hero-actions`, `hero-stats` from the reference.
- Copy `.phone`, `.phone-screen`, `.phone-notch`, `.phone-orbit`, `.phone-glow`, `.float-pill`, `.pill-1/2/3` and all animation keyframes.

---

## Phase 5 — Content Sections

For each content section (Features, How It Works, Tools, Testimonials, FAQ):

### 5.1 Section headers
Replace old badge/title elements with the unified pattern:
```html
<div class="section-header">
    <div class="section-badge glass-pill">[EMOJI] [SECTION NAME]</div>
    <h2 class="section-title">[TITLE]</h2>
    <p class="section-desc">[SUBTITLE]</p>
</div>
```

### 5.2 Feature cards
Use `.feature-card` with `.glass` for the card, `.feature-icon` for the emoji, `h3` for the title. Add a glow `<div>` as the last child with `position: absolute`.

### 5.3 Step cards
Use `.step-card` + `.step-number` (gradient circle with the step number). 3-column grid on desktop.

### 5.4 Tool cards
Use `.tool-card` + `.tool-emoji` (large emoji). 4-column grid on desktop.

### 5.5 Testimonials
Keep the infinite-scroll JS pattern. Use `.testimonial-card` with `width: 400px`. Preserve the `initTestimonials()` JS function.

### 5.6 FAQ
Preserve the `initFAQ()` JS function and `faqData` array. Update styling for `.faq-item`, `.faq-question`, `.faq-icon`, `.faq-answer`.

---

## Phase 6 — CTA & Footer

### 6.1 CTA box
Use `.cta-box` (glass card, border-radius 48px). Include:
- `h2` + `p` for copy
- `.btn.btn-glow` download button
- QR code image via `https://api.qrserver.com/v1/create-qr-code/?size=140x140&data=[STORE_URL]`

### 6.2 Footer
Use `.footer-grid` with `1.5fr 1fr 1fr` columns. Include:
- `.footer-brand` with logo + description
- Two `.footer-col` columns: Product links, Legal links
- `.footer-bottom` with copyright + tagline

---

## Phase 7 — Responsiveness

Add media queries in this order:
```css
@media (max-width: 1024px) { /* 2-column features/tools */ }
@media (max-width: 900px)  { /* 1-column hero, stacked nav, hide pills */ }
@media (max-width: 600px)  { /* 1-column everything, smaller CTA padding */ }
```

---

## Phase 8 — JavaScript

Ensure all JS at the bottom of the page includes:
1. **Smooth scroll** — `querySelectorAll('a[href^="#"]')` click handler
2. **Navbar scroll** — `.scrolled` class toggle
3. **Intersection Observer** — fade-in animation for cards and section headers
4. **Counter animation** — for `.stat-value` elements
5. **`initTestimonials()`** — dynamic card generation
6. **`initFAQ()`** — dynamic FAQ accordion

---

## Phase 9 — Cleanup & Validation

### 9.1 DOM audit
- Verify there is exactly **one** `<body>` tag and **one** `</body>` tag.
- Verify there is no orphaned/duplicated `<section>` or `<footer>` content.
- Remove any orphaned JavaScript (e.g., unused parallax orb scripts).

### 9.2 CSS lint check
- All `background-clip: text` declarations **must** include both:
  - `-webkit-background-clip: text;`
  - `background-clip: text;`
- Verify no `var(--variable)` references to deleted old variables.

### 9.3 Final review
Open the page in the browser (via `browser_subagent`) and check:
- [ ] Navbar is fixed and floating correctly
- [ ] Hero phone mockup animates (bob + orbit rings)
- [ ] Float pills are visible on desktop
- [ ] Section cards fade in on scroll
- [ ] FAQ accordion opens/closes
- [ ] Testimonials slider is scrolling
- [ ] Responsive layout is correct at 900px and 600px widths
- [ ] No console errors

---

## Next Steps Menu

After completing the migration, present these options:

1. 🧪 **Test** — Run a visual audit in the browser
2. 🚀 **Deploy** — Push to GitHub Pages
3. 🔧 **Adjust** — Refine colors, copy, or animations
4. 📋 **Next Page** — Migrate another page using this workflow
