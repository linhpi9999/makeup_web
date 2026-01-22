# Thỏ MakeUp — Copilot Instructions

## Project Overview

**Thỏ MakeUp** is a Vietnamese beauty services website featuring makeup courses, booking services, and bridal packages. It's a **static HTML/CSS site** with a tiered color design system and mobile-first responsive layout. No build tools, frameworks, or backend required.

**Language:** Vietnamese (vi) with accessible HTML semantics and minimal vanilla JS for theme switching and mobile navigation.

---

## Architecture & Key Patterns

### CSS Organization (Design Tokens → Components → Pages)

- **tokens.css**: Design system (colors, spacing, typography, shadows via CSS custom properties)
  - Brand colors: `--primary` (rose #d84a6a), `--premium` (champagne #d6b27c), `--accent` (lavender)
  - Layout: `--container: 1120px`, `--gutter: 18px`, `--radius: 18px`

- **base.css**: Global reset, HTML element defaults, `.container` layout helper

- **components.css**: Reusable UI patterns (buttons, links, tags, cards)
  - Button variants: `.btn-primary` (filled), `.btn-outline` (bordered), `.btn-ghost` (transparent), `.btn-soft` (soft fill)
  - Cards: `.card` with `.card-media`, `.card-body`, `.card-row` for layout

- **sections.css**: Header, footer, navigation, CTA sections (sticky header with blur backdrop)

- **pages.css**: Page-specific overrides (currently minimal; `.page-hero-title` for detail pages)

**Example CSS dependency flow:**  
`.btn-primary` relies on tokens (`--primary`, `--primary-hover`, `--ring`) defined in tokens.css. Always import in order: tokens → base → components → sections → pages.

### HTML Page Structure

All HTML files follow this pattern:
1. Language: `<html lang="vi">`
2. Theme support: `data-theme="light"` on `<html>` (supports `data-theme="dark"`)
3. CSS import order (see above)
4. Header with sticky navigation + mobile drawer
5. Main content with semantic `<section class="section">` containers
6. Footer with year auto-update
7. Inline script for theme toggle + mobile drawer UI (no external JS file)

**Pages:**
- `index.html` (homepage with hero, services carousel, CTA sections)
- `khoa-hoc.html` (courses catalog)
- `khoa-hoc-chi-tiet.html` (course detail)
- `dich-vu.html` (services catalog)
- `dich-vu-chi-tiet.html` (service detail — shows pattern for detail pages)
- `bang-gia.html` (pricing)
- `dat-lich.html` (booking)
- `lien-he.html` (contact)
- `ve-tho.html` (about)
- `bao-mat.html`, `dieu-khoan.html`, `hoan-tien.html` (legal pages)
- `404.html` (error page)

### Key Interactive Features (Vanilla JS)

**Theme Toggle:**
```javascript
// Stored in localStorage as "theme" (values: "light" | "dark")
// Toggled via #themeToggle button, sets/reads data-theme attribute on <html>
```

**Mobile Drawer:**
```javascript
// #burger button toggles #drawer visibility
// Uses aria-expanded, aria-hidden, and .is-open class for state
// Click outside drawer closes it
```

---

## Development Conventions

### Responsive Design

- **Viewport-relative sizing:** Use `clamp()` for typography that scales fluidly
  - Example: `.page-hero-title { font-size: clamp(26px, 3vw, 44px); }`
- **Breakpoints:** Mobile-first approach; media queries at `@media (max-width: 640px)` for small screens, `@media (min-width: 1024px)` for larger
- **Container queries:** Grid layouts use CSS Grid with `grid-template-columns` responding to viewport

### Image Handling

- **Background images:** Prefer `background-image` + fallback gradients over inline `<img>` to preserve layout
  - Applied to `.hero-image`, `.card-media`, `.media-wide` classes
  - Fallback gradients (e.g., `radial-gradient(...)`) ensure branded colors show if images missing
- **Image paths:** Relative paths from HTML root (e.g., `assets/images/hero.jpg`)
- **Lazy loading:** Use `loading="lazy"` on product/course images, `loading="eager"` on logo

### Color Palette & Typography

- **Primary brand:** Rose (`#d84a6a`) for CTAs and highlights
- **Secondary:** Champagne (`#d6b27c`) for premium services
- **Accents:** Lavender (`#b9a7ff`), Sage (`#a8c6b0`) for variety
- **Text:** Charcoal (`#1f1a1c`) on porcelain (`#faf7f5`)
- **Font stack:** `ui-sans-serif, system-ui, Segoe UI, Roboto` (no external fonts)

### Naming & Classes

- **BEM-inspired but relaxed:** Use hyphens for clarity (e.g., `.card-media`, `.nav-link`, `.btn-primary`)
- **State classes:** `.is-active`, `.is-open`, prefixed for clarity
- **Utility-like but scoped:** Classes describe purpose, not style (e.g., `.container`, `.card`, not `.flex-row`)

---

## Common Tasks & Workflows

### Adding a New Service or Course

1. Add detail page file (e.g., `khoa-hoc-chi-tiet.html`) mirroring [dich-vu-chi-tiet.html](./dich-vu-chi-tiet.html#L1):
   - Update `<title>` and meta description
   - Change `.nav-link.is-active` to match current section
   - Use `.card` components for service info
2. Link from catalog page (`khoa-hoc.html` or `dich-vu.html`)
3. Add background image via `.media-rose`, `.media-gold`, or `.media-lav` class

### Updating Navigation

Edit the `<nav class="nav">` and `.mobile-drawer` sections in **all** HTML files consistently. Current nav items:
- Trang chủ (index.html)
- Khóa học (khoa-hoc.html)
- Dịch vụ (dich-vu.html)
- Bảng giá (bang-gia.html)
- Đặt lịch (dat-lich.html)
- Liên hệ (lien-he.html)

### Dark Mode Support

All CSS custom properties already support `[data-theme="dark"]` selector. To add dark-specific styles:
```css
[data-theme="dark"] .my-class {
  background: rgba(246, 241, 243, 0.06);
}
```

### Adding New Components

1. Define in `components.css` using existing CSS variables
2. Include selector combos for button sizes (`.btn-sm`, `.btn-lg`)
3. Test in light and dark themes

---

## Important Notes

- **No build step:** Changes are live in browser—no compilation required
- **SEO:** All pages include `<meta name="description">` and Open Graph tags for sharing
- **Accessibility:** ARIA labels on buttons, semantic HTML (`<main>`, `<section>`, `<article>`), proper heading hierarchy
- **Git:** Repository includes `.git/` folder; commit frequently when updating content or styles
