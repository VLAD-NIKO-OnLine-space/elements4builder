# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Elements4Builder is a static HTML/CSS component library — a collection of pre-built, self-contained website sections for assembling landing pages. There is no build system, no package manager, and no framework.

## Development

- **Dev server:** VS Code Live Server extension on port 5501 (configured in `.vscode/settings.json`)
- **No build step, no tests, no linter** — files are plain HTML edited directly

## Architecture

Each top-level directory is a section type (Header, HERO, Features, Gallery, FAQ, Reviews, Plans, Roadmap, How, Partners, ABOUT, Footer, Something, Toc). Each contains numbered HTML variants (e.g., `How/how1.html` through `how20.html`).

Every HTML file is **self-contained**: semantic HTML + embedded `<style>` + optional `<script>`. No external imports — FontAwesome icons are referenced by class name but the `<link>` tag is NOT included in component files (it's loaded by the consuming page). Some Toc files use Swiper.js.

### Templating Convention

Components use `data-*` attributes as placeholders for a consuming builder system:
- `data-var="domain_name"` / `data-var="site_domain_plain"` — text variables
- `data-href="site_domain"` — dynamic links
- `data-image`, `data-illustration` — media placeholders (images, backgrounds)
- `data-partner-link`, `data-partner-logo` — partner section bindings
- `data-footer-label`, `data-footer-link`, `data-social` — footer bindings

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

### Class Naming

Each component uses a scoped BEM-like prefix matching its file: `ft14__card` (features14), `hw10__card` (how10), `pt5__stat` (partners5). This prevents CSS collisions when multiple components coexist.

## Conventions When Creating New Components

- **Do NOT include `:root` variables, CSS resets, or `box-sizing` rules** — the consuming page handles these
- **Do NOT include FontAwesome `<link>` tags** — only reference FA class names
- Use `data-image` (not hardcoded `src`) for any image/background placeholder
- Include `loading="lazy"` on images
- Always add `@media (prefers-reduced-motion: reduce)` to disable animations
- Internal CSS animations (hover effects, pulse, rotate, shimmer) are encouraged; scroll-triggered appear animations (AOS, intersection observer fade-ins) are not used
- When creating a new variant, read ALL existing variants in that directory first to ensure the new design is visually unique
