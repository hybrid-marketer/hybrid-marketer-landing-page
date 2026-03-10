# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a zero-dependency static marketing landing page for the Hybrid Marketer agency. The entire site lives in a single `index.html` file — no build step, no framework, no package manager.

**To preview:** Open `index.html` directly in a browser, or serve with any static file server (e.g., `python3 -m http.server 8080`).

## Architecture

Everything is contained in `index.html`:

- **CSS** — inline `<style>` tag with section comments (`/* ─── HERO ─── */`) for navigation
- **HTML** — semantic sections in page order: nav → hero → ticker → pain → solution → services → proof → cta → faq → footer
- **JS** — inline `<script>` tag at bottom, two behaviors: FAQ accordion and nav highlight on scroll

### Design System

CSS custom properties defined at `:root`:

```css
--color-dark:   #0E0E0E
--color-cream:  #FCF9F1
--color-brown:  #4E433F
--color-light:  #F2F2F2
--color-white:  #FFFFFF

--font-headline: 'Rubik'
--font-mono:     'IBM Plex Mono'

--tracking-headline: 0.12em
--lh-headline:       1.18
```

Fluid typography uses `clamp()` (e.g., `clamp(36px, 6vw, 72px)`) — avoid hardcoded `px` values for headline sizes.

Single responsive breakpoint: `@media (max-width: 900px)`.

### Button Variants

Three button classes exist, all extend `.btn`:

- `.btn-primary` — cream fill, dark text; inverts to outline on hover
- `.btn-outline` — transparent fill, cream border; inverts to cream fill on hover
- `.btn-dark` — dark fill, cream text; shifts to brown fill on hover

### Key Patterns

- **Section alternation:** Dark and cream backgrounds alternate for visual rhythm
- **State via class:** `.faq-item.open` toggles FAQ answers; nav links update opacity via `IntersectionObserver`
- **Grids:** Pain = 3-column, Services = 2-column, Testimonials (proof) = 3-column — all collapse to 1-column on mobile
- **Cards:** Thin border separation achieved via gap (not individual borders); `.hero-card` uses `::after` for the offset shadow border; `.service-card` has a `.light` variant (`--color-light` bg, dark text) for alternating grid cells
- **Ticker:** Infinite scroll via CSS `translateX` animation, `aria-hidden="true"`
- **CTA form:** Email input (`.cta-input`) + button in a flex row (`.cta-form`); collapses to column on mobile
- **Hero visual:** Hidden entirely on mobile (`display: none`) — not just reflowed

## Daily Plan

Two windows per weekday: **7:00–8:05 AM** (65 min) and **9:20–10:30 PM** (70 min).
Each block has one job. When the time is up, stop — even mid-task. Pick up exactly where you left off next session.

> **ADHD rule:** If you feel yourself drifting — close all tabs except `index.html` and the one reference link listed. No browsing, no "just checking."

---

### DAY 1

**▸ 7:00–8:05 AM — Fix the dead links (65 min)**

1. Open `index.html`, search for `service-link` — find all 4 `<span class="service-link">` elements
2. Change each `<span>` to `<a href="#cta">` and close with `</a>`
3. Open browser, hover each card — confirm cursor changes to pointer and link works
4. Save.

✓ DONE = 4 service links are real anchors pointing to `#cta`

**▸ 9:20–10:30 PM — Set up Formspree (70 min)**

1. Go to formspree.io → create free account → New Form → copy the endpoint URL
2. In `index.html`, find `.cta-form` — change the button `href="#"` to a real submit
3. Wrap input + button in `<form action="YOUR_URL" method="POST">`; give input `name="email"`
4. Add 10 lines of JS: on submit, `fetch()` the form, show "✓ We'll be in touch." in place of the form on success
5. Test with your own email — confirm you receive it

✓ DONE = form submits, you get an email, success message shows

---

### DAY 2

**▸ 7:00–8:05 AM — Create the OG image (65 min)**

1. Open Canva (or Figma) → New design → Custom size: 1200 × 630px
2. Background: `#FCF9F1` (cream). Add text in Rubik Bold, color `#0E0E0E`:
   - Line 1 (large): STOP BEING THE BEST KEPT SECRET
   - Line 2 (small, mono): hybridmarketer.co
3. Export as PNG → save to project folder as `og-image.png`

✓ DONE = file exists at `hybrid-marketer-landing/og-image.png`

**▸ 9:20–10:30 PM — Add OG tag + deploy to Netlify (70 min)**

1. In `index.html` `<head>`, add after existing OG tags:

   ```html
   <meta property="og:image" content="https://YOUR-DOMAIN/og-image.png" />
   ```

2. Go to netlify.com → "Add new site" → "Deploy manually" → drag the project folder
3. Netlify gives you a `.netlify.app` URL — copy it, update `og:image` content with real URL
4. Paste the URL into [opengraph.xyz](https://www.opengraph.xyz) — verify preview looks right

✓ DONE = site is live, OG preview shows the image

---

### DAY 3

**▸ 7:00–8:05 AM — Scroll-aware nav (65 min)**

1. In `index.html` JS block, add scroll listener:

   ```js
   window.addEventListener('scroll', () => {
     document.querySelector('nav').classList.toggle('scrolled', window.scrollY > 80);
   });
   ```

2. In CSS `/* ─── NAV ─── */`, add:

   ```css
   nav.scrolled { padding: 12px 0; background: rgba(14,14,14,0.97); }
   nav { transition: padding 0.3s, background 0.3s; }
   ```

3. Reload → scroll down → confirm nav tightens

✓ DONE = nav visibly compresses on scroll

**▸ 9:20–10:30 PM — Scroll-reveal animations (70 min)**

1. In CSS, add:

   ```css
   .reveal { opacity: 0; transform: translateY(24px); transition: opacity 0.5s, transform 0.5s; }
   .reveal.visible { opacity: 1; transform: none; }
   ```

2. Add `class="reveal"` to: `.pain-header`, `.solution-header`, `.services-header`, `.proof-header`, `.faq-header`, and each `.step`, `.testimonial`, `.pain-card`
3. In JS, add one `IntersectionObserver` that adds `.visible` when elements enter viewport (`threshold: 0.15`)
4. Reload → scroll slowly — confirm sections fade in

✓ DONE = sections animate in on scroll, no jarring load

---

### DAY 4

**▸ 7:00–8:05 AM — A/B headline + scarcity copy (65 min)**

1. A/B: in JS, add 3 lines — if `location.search` includes `?v=b`, swap `.hero-headline` inner HTML to: `Your Competitors Aren't Better —<br/>They're Just<br/><em>More Visible</em>`
2. Test: open `index.html?v=b` in browser — confirm variant shows
3. Scarcity: above `.cta-form` in HTML, add:

   ```html
   <p class="label" style="margin-bottom:16px;opacity:0.6;">Only 3 audit spots open this month</p>
   ```

4. Check it renders correctly — adjust only if it breaks layout

✓ DONE = `?v=b` shows alternate headline; scarcity line appears above form

**▸ 9:20–10:30 PM — Positioning Sprint page: scaffold (70 min)**

1. Duplicate `index.html` → rename `positioning-sprint.html`
2. Delete sections: ticker, pain, solution, proof — keep: nav, hero, services (repurpose as "What You Get"), cta, faq, footer
3. Update hero: eyebrow = "Productized Offer", H1 = "Your Message, Sharpened in 5 Days", sub = who it's for + outcome
4. Save — open in browser — confirm structure loads with no broken styles

✓ DONE = `positioning-sprint.html` opens with correct layout skeleton

---

### DAY 5

**▸ 7:00–8:05 AM — Positioning Sprint page: content (65 min)**

1. "What You Get" grid (3 cards): Positioning framework doc / Rewritten homepage headline + hero copy / 30-min review call
2. Update FAQ (3 items): Who is this for? / What do I need to provide? / What if I'm not happy?
3. CTA section: price `$1,500` + "Book My Sprint" button → Formspree form (reuse same endpoint)
4. Footer: update copyright year if needed

✓ DONE = page has real content, form works, price is visible

**▸ 9:20–10:30 PM — Visibility Audit page (70 min)**

1. Duplicate `positioning-sprint.html` → rename `visibility-audit.html`
2. Hero: H1 = "See Exactly Where You're Leaking Leads", sub = free 30-min call, no credit card
3. "What You Get" cards: Where your ideal clients look (and why they miss you) / Top 3 quick wins you can act on immediately / A clear next-step recommendation
4. CTA: swap price for "Free — 30 minutes" + "Book My Audit" → calendar link (Cal.com or Calendly)
5. Add link to both offer pages in main `index.html` footer nav

✓ DONE = audit page live, linked from footer, calendar opens on CTA click

---

## Tasks

Time-boxed execution list. Work top-to-bottom — earlier items unblock later ones.

### SHIP NOW — Blocking launch

- [ ] **Wire up the CTA form** — email input currently submits to `#`. Connect to a form handler (Netlify Forms, Formspree, or GHL form endpoint). Add a success/error state in JS.
- [ ] **Fix service card links** — "Learn More →" spans are not anchor tags. Either link to a section/page or remove until offer pages exist. Dead affordances hurt trust.
- [ ] **Add `og:image` meta tag** — no social preview image. Minimum: 1200×630px, cream bg, dark Rubik headline. Required before sharing the URL anywhere.
- [ ] **Deploy to static host** — no build needed; drop `index.html` on Netlify/Vercel and add custom domain.

### NEXT — Conversion lift before first traffic push

- [ ] **Scroll-aware nav shrink** — on scroll past hero, reduce nav padding (`20px → 12px`) and increase background opacity. Pure CSS transition, no library needed.
- [ ] **Scroll-reveal animations** — fade + translate-up on section entry via `IntersectionObserver`. Add `.reveal` class to section headers and cards; trigger with a class toggle. No GSAP — keep it native.
- [ ] **A/B hero headline** — test Pattern 3 ("Your Competitors Aren't Better — They're Just More Visible") vs current Pattern 1. Can be done with a URL param + 3 lines of JS.
- [ ] **Add scarcity signal to CTA** — "Only 3 audit spots open this month" above the form (only if true). Increases urgency without discounting.

### SOON — Offer pages

- [ ] **Positioning Sprint page** — use the offer page template from `Swipe File.md §4`. Deliverable: sharper message in 5 days, $1,500–$3,000 price point.
- [ ] **Visibility Audit page** — free entry offer, 30-min call. Drives pipeline. Same template.
- [ ] **Pricing page** — 3-tier structure (Audit / Sprint / Retainer). Lead with value transformation, not feature table.

## Deployment

No build required. Copy `index.html` to any static host (Netlify, Vercel, S3, etc.).
