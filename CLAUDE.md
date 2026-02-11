# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Elements4Builder is a static HTML/CSS component library — a collection of pre-built, self-contained website sections for assembling landing pages. There is no build system, no package manager, and no framework.

## Development

- **Dev server:** VS Code Live Server extension on port 5501 (configured in `.vscode/settings.json`)
- **No build step, no tests, no linter** — files are plain HTML edited directly

## Architecture

Each top-level directory is a section type (Header, HERO, Features, Gallery, FAQ, Reviews, Plans, Roadmap, How, Partners, ABOUT, Footer, Something, Toc). Each contains numbered HTML variants (e.g., `How/how1.html` through `how20.html`).

Every HTML file is **self-contained**: semantic HTML + embedded `<style>` + optional `<script>`. No external imports — FontAwesome icons are referenced by class name but the `<link>` tag is NOT included in component files (it's loaded by the consuming page).

### Templating Convention

Components use `data-*` attributes as placeholders for a consuming builder system:
- `data-var="domain_name"` / `data-var="site_domain_plain"` — text variables
- `data-href="site_domain"` — dynamic links
- `data-image`, `data-illustration` — media placeholders (images, backgrounds)
- `data-partner-link`, `data-partner-logo` — partner section bindings
- `data-footer-label`, `data-footer-link`, `data-social` — footer bindings
- `data-target` — animation targets (used in Something for stat counters)

### CSS Theming

All components rely on these CSS custom properties (defined by the consuming page, NOT in component files):
- `--bodyBG`, `--textColor1`, `--textColor2`, `--secondStyleColor`
- `--borderRadius`, `--maxWidthContainer`

Transparent color variations use `color-mix(in srgb, var(--secondStyleColor) 30%, transparent)` rather than hardcoded `rgba()`.

### Responsive Breakpoints

Three standard breakpoints, in this order:
- `@media (max-width: 950px)` — tablets
- `@media (max-width: 800px)` — small tablets
- `@media (max-width: 600px)` — mobile

Responsive font sizes use `clamp()` (e.g., `font-size: clamp(28px, 4vw, 42px)`).

### Class Naming

Each component uses a scoped BEM-like prefix matching its file: `ft14__card` (features14), `hw10__card` (how10), `pt5__stat` (partners5). This prevents CSS collisions when multiple components coexist.

**Exception:** The Something directory uses inconsistent prefixes (e.g., `bx7-`, `stats--dynamic`) rather than `smth1__` style. Follow existing patterns when editing these files.

### HTML Structure

Every component's root element is a `<section>` with the scoped class (e.g., `<section class="ft14">`). Inside, content sits in a `.container` div. Cards use `<article>` tags.

### Animation Patterns

- **SVG animations** are preferred over canvas — use native SVG elements (`<animate>`, `<animateTransform>`, `<animateMotion>`) with `<defs>` for gradients, patterns, and filters
- **CSS animations** (`@keyframes`) supplement SVG for secondary effects (pulse, shimmer, glow)
- Decorative SVGs must include `aria-hidden="true"`
- Performance hints: use `will-change: transform, opacity` on animated elements; `contain: layout paint` on particle containers

### JavaScript in Components

Most components are pure CSS/HTML. When scripts are needed (particle generation, accordions, carousels):
- Use vanilla JS only — no libraries except Swiper.js for carousels
- Guard external dependencies: `if (typeof Swiper === "undefined") return;`
- For dynamic particles, set CSS custom properties (`--x`, `--y`, `--s`, `--d`) on generated elements rather than inline transform styles
- Listen for `resize` to regenerate responsive layouts (e.g., fewer particles on mobile)

### Swiper.js Usage

Toc files and some Reviews files use Swiper.js. The Swiper library itself is loaded by the consuming page. Toc components only provide HTML structure and styling (no initialization script). Reviews carousel components include their own `<script>` that initializes Swiper with configuration.

### Glass Morphism

Many components use glass effects: `backdrop-filter: blur(16px)` with `-webkit-backdrop-filter` for Safari support, combined with semi-transparent gradient backgrounds.

## Conventions When Creating New Components

- **Do NOT include `:root` variables, CSS resets, or `box-sizing` rules** — the consuming page handles these
- **Do NOT include FontAwesome `<link>` tags** — only reference FA class names
- Use `data-image` (not hardcoded `src`) for any image/background placeholder
- Include `loading="lazy"` on images
- Always add `@media (prefers-reduced-motion: reduce)` to disable animations
- Internal CSS animations (hover effects, pulse, rotate, shimmer) are encouraged; scroll-triggered appear animations (AOS, intersection observer fade-ins) are not used
- When creating a new variant, read ALL existing variants in that directory first to ensure the new design is visually unique
