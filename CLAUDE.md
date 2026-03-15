# Lines in Art – Press Kit Site

## Project Overview

Static HTML press kit website for **Lines in Art**, a children's picture book that teaches kids to observe real art through play. Published by **Little Oaknut LLC**.

- **Live URL:** https://linesinart.com/press
- **Contact:** hello@linesinart.com
- **Kickstarter launched:** March 12, 2026
- **GitHub:** git@github.com:ghigo/linesinart-press.git

## Tech Stack

- Vanilla HTML5 + CSS3 (no framework, no build step)
- Minimal JavaScript (only dynamic footer year)
- No external dependencies
- Static hosting (no backend)

## File Structure

```
linesinart-press/
├── sitemap.xml         # SEO sitemap
├── BingSiteAuth.xml    # Bing webmaster verification
├── download/
│   └── individual/     # Local copies of PDF press materials
└── press/
    ├── index.html      # Press kit page
    └── styles.css      # All styles
```

Root is intentionally empty — add new pages as sibling directories alongside `press/`.

File downloads are hosted externally at `https://downloads.linesinart.com/`.

## Site Sections

1. **Header** – Title, tagline, Kickstarter CTA, email contact
2. **Downloads** – Bundled press kits (full, images-only, press releases)
3. **Quick Access PDFs** – Individual documents (press release short/extended/Italian, fact sheet, Kickstarter overview, creators bio, press contact)
4. **Social Media** – Instagram (@linesinartbook), Facebook, TikTok (@linesinart)
5. **Updates Timeline** – Milestone changelog

## Design System

CSS custom properties defined in `:root`:
- Primary: `#4356A6` (Ocean Twilight blue)
- Accent: `#FF8F00` (orange), `#FFBD05` (yellow), `#568203` (green)
- Background: `#FFFCFF` (off-white)
- Responsive breakpoints: 780px, 520px
- Typography: fluid sizing with `clamp()`

## SEO & Analytics

- Google Analytics 4: `G-N059GWK2SE` (page path: `/press`)
- JSON-LD structured data: Organization, WebSite, WebPage, Book
- Open Graph + Twitter Card meta tags
- Canonical URL, hreflang (Italian variant)

## Key Constraints

- No build process – edit HTML/CSS directly
- Keep the page lightweight (currently ~20 KB total uncompressed)
- All PDF assets served from `downloads.linesinart.com`, not this repo
- `.claude/settings.local.json` restricts `git config` commands via bash hooks
