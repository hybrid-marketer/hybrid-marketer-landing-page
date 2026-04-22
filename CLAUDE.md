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
claude --resume "glauber-personal-brand-framework" 