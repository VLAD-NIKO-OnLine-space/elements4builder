# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Elements4Builder is a static HTML/CSS component library — a collection of pre-built, self-contained website sections for assembling landing pages. There is no build system, no package manager, and no framework.

## Development

- **Dev server:** VS Code Live Server extension on port 5501 (configured in `.vscode/settings.json`)
- **No build step, no tests, no linter** — files are plain HTML edited directly

## Architecture

Each top-level directory represents a section type (Header, HERO, Features, Gallery, FAQ, Reviews, Plans, Roadmap, How, Partners, ABOUT, Footer, Something, Toc). Each directory contains numbered HTML file variants of that section (e.g., `HERO/hero1.html` through `hero24.html`).

Every HTML file is a **self-contained component**: semantic HTML + embedded `<style>` + optional `<script>`, with no external dependencies beyond FontAwesome icons.

### Templating Convention

Components use `data-*` attributes as placeholders for a consuming builder system:
- `data-var="domain_name"` / `data-var="site_domain_plain"` — text variables
- `data-href="site_domain"` — dynamic links
- `data-image`, `data-illustration` — media placeholders
- `data-footer-label`, `data-footer-link`, `data-social` — footer bindings

### CSS Theming

All components share CSS custom properties for consistent theming:
- `--bodyBG`, `--textColor1`, `--textColor2`, `--secondStyleColor`
- `--borderRadius`, `--maxWidthContainer`

### Responsive Breakpoints

- `950px` — tablets
- `800px` — small tablets
- `600px` — mobile

## Conventions

- Keep components self-contained (styles and scripts inline within each HTML file)
- Use CSS custom properties for any colors, radii, or sizing that should be themeable
- Use `data-*` attributes for dynamic content — never hardcode domain names or site-specific text
- Include `loading="lazy"` and `decoding="async"` on images
- Use FontAwesome class names for icons (e.g., `fa-solid fa-quote-left`)
