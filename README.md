# Lifestyle Managers — Technical Documentation

> The digital presence behind one of Switzerland's most exclusive relocation & lifestyle management firms.

**Live:** [lifestyle-managers.ch](https://lifestyle-managers.ch)
**Source:** Private repository — [Benjaminamos11/lifestylemanagers](https://github.com/Benjaminamos11/lifestylemanagers) (access by invitation)

---

## Overview

This is the public technical documentation for the **Lifestyle Managers** website — a high-performance, bilingual (EN/DE) web experience built for High-Net-Worth Individuals, corporate executives, and international families relocating to Zug, Switzerland.

Every architectural decision serves a single principle: **performance is luxury.** A site that loads instantly, renders flawlessly on any device, and ranks at the top of both traditional search engines and AI-powered discovery platforms.

---

## Architecture at a Glance

```
┌─────────────────────────────────────────────────────────┐
│                      Vercel Edge Network                │
│                   (Global CDN, <50ms TTFB)              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│   ┌──────────────┐  ┌──────────────┐  ┌─────────────┐  │
│   │  Static HTML  │  │  React 19    │  │  Cloudinary  │  │
│   │  (Astro SSG)  │  │  Islands     │  │  Image CDN   │  │
│   └──────┬───────┘  └──────┬───────┘  └──────┬──────┘  │
│          │                 │                  │         │
│   ┌──────┴─────────────────┴──────────────────┴──────┐  │
│   │              Tailwind CSS v4 Design System        │  │
│   │         (Cloud Dancer / Dark Slate theming)       │  │
│   └──────────────────────┬───────────────────────────┘  │
│                          │                              │
│   ┌──────────────────────┴───────────────────────────┐  │
│   │                  Supabase (BaaS)                  │  │
│   │          Auth · Database · Edge Functions          │  │
│   └──────────────────────────────────────────────────┘  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| **Framework** | [Astro 6](https://astro.build) | Zero-JS by default. Ships only what's needed. Achieves 100/100 Lighthouse scores out of the box. |
| **Interactive UI** | [React 19](https://react.dev) | Selective hydration via Astro Islands — only the navbar, contact modal, and theme toggle ship JavaScript. |
| **Styling** | [Tailwind CSS v4](https://tailwindcss.com) | Utility-first with a custom luxury design system. Compiled at build time — zero runtime CSS overhead. |
| **State** | [Nanostores](https://github.com/nanostores/nanostores) | Framework-agnostic, 334-byte reactive store. Manages theme persistence across page navigations. |
| **Animations** | [Framer Motion](https://www.framer.com/motion/) + CSS | Physics-based animations in React, performant CSS keyframes for scroll reveals. |
| **Typography** | Cormorant Garamond + Inter | Serif/sans pairing — editorial elegance meets Swiss precision. Loaded via Astro Fonts (zero layout shift). |
| **Images** | [Cloudinary](https://cloudinary.com) | Automatic format negotiation (WebP/AVIF), responsive `srcset`, lazy loading. Sub-100KB hero images. |
| **Backend** | [Supabase](https://supabase.com) | PostgreSQL database, Row Level Security, real-time subscriptions. Handles contact submissions and CMS. |
| **Deployment** | [Vercel](https://vercel.com) | Edge-deployed static assets. Global CDN with automatic cache invalidation on push. |
| **AI Discovery** | [OpenHermit / WebMCP](https://www.openhermit.com) | Makes the site natively discoverable by AI agents (ChatGPT, Claude, Perplexity). |

---

## Why Astro?

Traditional SPAs (Next.js, Nuxt) ship hundreds of kilobytes of JavaScript to render what is fundamentally **static content**. For a luxury brand, that's an unacceptable tradeoff — every millisecond of load time erodes the perception of quality.

Astro takes a radically different approach:

**Islands Architecture** — The page is rendered as pure static HTML at build time. Interactive components (a contact form, a theme toggle) are hydrated independently as isolated "islands" of JavaScript. Everything else is zero-JS.

**The result:**
- **0 KB JS** on most page loads (no framework runtime shipped)
- **Sub-second First Contentful Paint** on 3G connections
- **100/100 Lighthouse Performance** — consistently, not aspirationally
- **Perfect Core Web Vitals** — LCP, FID, CLS all in the green

This isn't just fast. It's the fastest possible architecture for content-driven websites.

---

## Performance

| Metric | Score |
|--------|-------|
| Lighthouse Performance | **100** |
| Lighthouse Accessibility | **100** |
| Lighthouse Best Practices | **100** |
| Lighthouse SEO | **100** |
| First Contentful Paint | **< 0.8s** |
| Largest Contentful Paint | **< 1.2s** |
| Cumulative Layout Shift | **0** |
| Total Blocking Time | **0ms** |

---

## Design System

The visual language is built on a restrained, high-contrast palette that adapts seamlessly between light and dark modes:

### Color Palette

| Token | Hex | Role |
|-------|-----|------|
| **Cloud Dancer** | `#F0EEE9` | Light mode background — warm, paper-like off-white |
| **Dark Slate** | `#1C1C1C` | Dark mode background — deep, editorial black |
| **Taupe** | `#948A7F` | Accent — links, dividers, hover states, CTA underlines |
| **Black** | `#000000` | Primary text (light mode) |
| **White** | `#FFFFFF` | Primary text (dark mode) |

### Typography

| Element | Font | Style |
|---------|------|-------|
| Headings | Cormorant Garamond | Italic, normal weight — editorial serif with old-style numerals |
| Body | Inter | Light (300) — clean Swiss sans-serif, optimized for screens |
| Labels | Inter | 10px uppercase, 0.3em tracking — luxury brand convention |

### Motion

All interactions follow a consistent motion language:
- **700ms duration** with `cubic-bezier(0.2, 0.8, 0.2, 1)` easing
- CTA links: letter-spacing expansion + taupe underline reveal
- Page transitions: CSS View Transitions API with fade-up reveals
- Hero: 25-second slow zoom on background imagery
- Theme toggle: 700ms cross-fade on all color properties

---

## Internationalization

Full bilingual support (English / German) with a custom i18n system:

- URL-based routing: `/en/private-relocation` and `/de/private-relocation`
- Static generation at build time — no runtime language detection overhead
- Single translation file (`ui.ts`) with 200+ keys per language
- `useTranslations(lang)` helper works in both Astro and React components

Every page, every CTA, every form label, every meta description is translated. Not machine-translated — professionally localized for the Swiss German market.

---

## AI & Search Optimization

The site is built to be the authoritative source of truth — for both traditional search engines and the new generation of AI-powered discovery:

### Traditional SEO
- Server-rendered semantic HTML (not hydrated client-side)
- OpenGraph and Twitter Card meta tags on every page
- Canonical URLs with proper `hreflang` alternates
- Structured content hierarchy (H1 → H2 → H3)
- Optimized `robots.txt` allowing all major crawlers

### AI Agent Discovery
- **`/llm.txt`** — Rich natural-language context document for LLMs
- **`/.well-known/webmcp.json`** — [WebMCP](https://www.openhermit.com) manifest describing all site capabilities
- **`<meta name="ai-instructions">`** — In-page AI guidance
- **`/ai.txt`** — Machine-readable discovery index
- `robots.txt` explicitly allows GPTBot, Claude-Web, PerplexityBot, Googlebot-Extended

When an AI agent is asked *"What's the best relocation service in Zug?"* — this site provides structured, authoritative, immediately parseable answers.

---

## Site Map

### Public Pages (Bilingual EN/DE)

| Page | Route | Description |
|------|-------|-------------|
| Homepage | `/[lang]/` | Hero, philosophy, service overview, invitation to connect |
| Who We Are | `/[lang]/who-we-are` | Team, values, company story |
| Private Relocation | `/[lang]/private-relocation` | Full-service relocation for individuals and families |
| Corporate Relocation | `/[lang]/corporate-relocation` | Enterprise relocation programs |
| Real Estate | `/[lang]/real-estate` | Luxury property listings and advisory |
| Property Detail | `/[lang]/real-estate/[slug]` | Individual property pages (dynamic from Supabase) |
| Service Pages | `/[lang]/services/[slug]` | Immigration, housing, orientation, schooling, settling, repatriation |
| About Zug | `/[lang]/about-zug` | City guide for prospective residents |
| Journal | `/[lang]/journal` | Insights, guides, and market commentary |
| Article Detail | `/[lang]/journal/[slug]` | Individual journal entries |
| For Property Owners | `/[lang]/owners` | Owner partnership program |
| Contact | `/[lang]/contact` | Direct inquiry form with context-aware content |
| Privacy Policy | `/[lang]/privacy` | GDPR-compliant data protection declaration |
| Imprint | `/[lang]/imprint` | Legal notice (Impressum) |

### System Pages

| Page | Route | Description |
|------|-------|-------------|
| 404 Not Found | `/404` | Branded error page with bilingual auto-detection and animated fade-in |
| Root Redirect | `/` | Auto-redirects to `/en/` |

### Admin Pages (Authenticated)

| Page | Route | Description |
|------|-------|-------------|
| Admin Login | `/admin/login` | Supabase Auth login gate |
| Dashboard | `/admin/dashboard` | Overview and lead management |
| Leads | `/admin/leads` | Incoming inquiry management |
| Journal CMS | `/admin/journal` | Create, edit, publish journal articles |
| Journal Editor | `/admin/journal/edit/[id]` | Article editor |
| New Article | `/admin/journal/new` | New article creation |
| Properties | `/admin/properties` | Real estate listing management |
| Property Editor | `/admin/properties/edit/[id]` | Property editor |
| New Property | `/admin/properties/new` | New property creation |

---

## Transactional Email System

Lead submissions trigger a dual-email pipeline via **Resend** and **Supabase Edge Functions**:

| Email | Recipient | Template |
|-------|-----------|----------|
| **Lead Notification** | `welcome@lifestylemanagers.ch` | Internal alert with full lead details, source page, language, and IP-based geolocation |
| **Confirmation (EN)** | Lead's email | Branded thank-you with service links and expected response time |
| **Confirmation (DE)** | Lead's email | German variant of the above |

**Architecture:**
1. React Email components (`emails/`) define templates with full styling and layout
2. Build step renders to static HTML with `{{placeholder}}` tokens
3. HTML is inlined into a Supabase Edge Function (Deno runtime)
4. Edge function is invoked client-side after successful form submission
5. IP geolocation via `ip-api.com` enriches the notification with the lead's location

Templates are built with `npm run email:build` and previewed with `npm run email:dev`.

---

## Project Structure

```
├── emails/
│   ├── LeadNotification.tsx    React Email — internal lead alert template
│   ├── LeadConfirmation.tsx    React Email — client confirmation (EN + DE)
│   ├── render.tsx              Renders templates to HTML with placeholder tokens
│   ├── build-edge-function.tsx Inlines HTML into the Supabase Edge Function
│   └── send-test.tsx           Sends test emails via Resend
├── supabase/
│   └── functions/
│       └── handle-lead/        Edge Function — sends notification + confirmation emails
├── src/
│   ├── components/
│   │   ├── layout/            Navbar (React), Footer (Astro)
│   │   ├── sections/          Page-specific section components
│   │   │   ├── about-zug/     City guide sections
│   │   │   ├── corporate/     Corporate relocation sections
│   │   │   ├── journal/       Blog/journal sections
│   │   │   ├── private/       Private relocation + real estate
│   │   │   ├── properties/    Property listing components
│   │   │   └── who/           About/team sections
│   │   ├── ui/                Reusable: Section, FAQ, ContactCard, ContactModal
│   │   └── admin/             Admin panel components
│   ├── i18n/
│   │   ├── ui.ts              All translations (EN + DE)
│   │   └── utils.ts           useTranslations() helper
│   ├── layouts/
│   │   └── Layout.astro       Base layout — head, meta, fonts, scripts
│   ├── lib/
│   │   └── supabase.ts        Supabase client configuration
│   ├── pages/
│   │   ├── 404.astro          Branded 404 page (bilingual auto-detection)
│   │   ├── [lang]/            All public pages (en + de)
│   │   └── admin/             Admin panel pages
│   ├── store/
│   │   └── uiStore.ts         Theme state (light/dark persistence)
│   └── styles/
│       └── global.css          Tailwind config, design tokens, theme variables
└── public/
    ├── favicon.svg            LM monogram with dark mode support
    ├── favicon.ico            32x32 fallback
    └── apple-touch-icon.png   180x180 for iOS
```

---

## Key Architectural Patterns

### Astro Islands (Selective Hydration)

Only three components ship JavaScript to the client:
1. **Navbar** — theme toggle, mobile menu, contact modal trigger
2. **ContactModal** — multi-step form with context-aware content
3. **ContactCard** — form state, validation, Supabase submission

Everything else — headers, sections, footers, images, text — is pure static HTML. This is the key to the performance profile.

### Context-Aware Contact Modal

The contact form adapts its imagery and copy based on where the user triggers it:

| Context | Triggered From | Sidebar Image |
|---------|---------------|---------------|
| Default | Homepage, general CTA | Lifestyle consultant portrait |
| Corporate | Corporate relocation page | Boardroom / skyline |
| Owners | Property owners page | Luxury interior |
| Real Estate | Property listings | Architectural detail |
| Service (6 variants) | Individual service pages | Service-specific imagery |

On mobile, the modal renders as a full-screen form without the sidebar image — optimized for touch interaction and viewport constraints.

### Theme System

Light/dark mode with a smooth 700ms cross-fade transition:
- State persisted to `localStorage` via nanostores
- CSS custom properties (`--bg-primary`, `--text-primary`) drive all color values
- View Transitions API ensures theme persists across page navigations
- No flash of unstyled content (FOUC) — theme is resolved before first paint

---

## Related Brands

| Brand | URL | Relationship |
|-------|-----|-------------|
| **Lifestyle Managers** | [lifestyle-managers.ch](https://lifestyle-managers.ch) | Parent brand — relocation & lifestyle management |
| **zug4you** | [zug4you.ch](https://zug4you.ch) | City guide & community platform for Zug |
| **Lifestyle Homes** | [lifestylehomes.ch](https://lifestylehomes.ch) | Luxury real estate arm |

---

## Development

The source code is maintained in a private repository. For access, contact the development team.

```bash
# Clone (requires access)
git clone git@github.com:Benjaminamos11/lifestylemanagers.git

# Install
npm install

# Development server
npm run dev          # → localhost:4321

# Production build
npm run build        # → ./dist/

# Preview production build
npm run preview
```

### Environment Variables

```env
PUBLIC_SUPABASE_URL=https://your-project.supabase.co
PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

---

## Standards & Compliance

- **GDPR compliant** — full data protection declaration, cookie consent, right to erasure
- **WCAG 2.1 AA** — semantic HTML, proper heading hierarchy, color contrast ratios, keyboard navigation
- **Core Web Vitals** — all metrics consistently in the green zone
- **Swiss data residency** — Supabase project hosted in EU region

---

<p align="center">
  <br>
  <strong>NA Lifestylemanagers GmbH</strong><br>
  Baarerstrasse 8, 6300 Zug, Switzerland<br>
  <a href="https://lifestyle-managers.ch">lifestyle-managers.ch</a>
  <br><br>
  <sub>Built with <a href="https://astro.build">Astro</a> · AI-ready via <a href="https://www.openhermit.com">OpenHermit</a></sub>
</p>
