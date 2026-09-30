# bkglamm

> A beauty salon website and catalog platform built with Next.js, Payload CMS, and Tailwind CSS.

Live storefront with a WhatsApp booking flow, a CMS-driven product catalog, and a full admin panel — all managed from one codebase.

---

## Overview

**bkglamm** is the digital home of BKGlamm Beauty, a beauty salon based in Gaborone, Botswana. The site is built to be fully content-managed — every page, product, navigation link, and hero section is editable from the Payload admin panel with no code changes required.

### Key features

- **CMS-driven everything** — hero, navbar, footer, about page, and product catalog are all controlled from the admin panel
- **WhatsApp booking** — "Book Now" on every product page opens WhatsApp with a pre-filled message containing the product name, price, and link
- **Offset product grid** — two-column staggered layout with warm dark aesthetics matching the brand
- **Video / image hero** — background media is swappable from the admin with overlay intensity control
- **Revalidation** — pages revalidate every 60 seconds so content changes go live without a redeploy

---

## Tech stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router) |
| CMS | Payload CMS 3 |
| Database | SQLite (dev) / PostgreSQL (production) |
| Styling | Tailwind CSS v4 |
| Fonts | Cormorant Garamond + Outfit (Google Fonts) |
| Language | TypeScript |
| Deployment | Vercel (frontend) + Neon / Supabase (database) |

---

## Project structure

```
src/
├── app/
│   ├── (frontend)/         # Public-facing pages
│   │   ├── page.tsx        # Homepage (hero + catalog grid)
│   │   ├── shop/           # Catalog page + [slug] product detail
│   │   ├── about/          # About page
│   │   └── layout.tsx      # Frontend layout (includes footer)
│   ├── layout.tsx          # Root layout (fonts, navbar, globals)
│   ├── globals.css         # Tailwind v4 + BKGlamm theme tokens
│   └── not-found.tsx       # 404 page
├── collections/
│   └── Products.ts         # Products collection (name, images, price, stock, category)
├── globals/
│   ├── Navigation.ts       # Navbar links + CTA
│   ├── HomepageHero.ts     # Hero headline, badge, CTAs, background media
│   ├── SiteFooter.ts       # Footer columns, socials, bottom bar
│   ├── AboutPage.ts        # About page sections (hero, stats, mission, team, CTA)
│   └── SiteSettings.ts     # WhatsApp number + contact details
├── components/
│   ├── layout/
│   │   ├── Navbar.tsx      # Server component — fetches nav data
│   │   ├── NavbarClient.tsx# Client component — scroll/mobile behaviour
│   │   └── Footer.tsx      # Server component — fetches footer data
│   ├── sections/
│   │   ├── Hero.tsx        # Server component — fetches hero data
│   │   ├── HeroClient.tsx  # Client component — parallax / video
│   │   └── CatalogGrid.tsx # Server component — fetches + renders products
│   └── ui/
│       └── BookNowButton.tsx # Client component — WhatsApp deep link
├── lib/
│   └── utils.ts            # cn() helper (clsx + tailwind-merge)
└── types/
    └── index.ts            # Product, Category, Cart, User types
```

---

## Getting started

### Prerequisites

- Node.js v18+
- Git Bash (Windows) or any Unix terminal
- A PostgreSQL database (see [Neon](https://neon.tech) for a free hosted option)

### 1. Clone the repo

```bash
git clone https://github.com/your-username/bkglamm.git
cd bkglamm
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up environment variables

Copy the example file and fill in your values:

```bash
cp .env.example .env.local
```

```env
# .env.local

# Database — SQLite for local dev
DATABASE_URL=file:./database.db

# Switch to PostgreSQL for production:
# DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE

# Payload secret — generate with:
# node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
PAYLOAD_SECRET=your_secret_here

# App URL
NEXT_PUBLIC_SERVER_URL=http://localhost:3000
```

### 4. Run the dev server

```bash
npm run dev
```

- Storefront → [http://localhost:3000](http://localhost:3000)
- Admin panel → [http://localhost:3000/admin](http://localhost:3000/admin)

Create your first admin user when prompted on the admin first run.

---

## CMS usage

### Adding products

1. Go to **Admin → Products → Create New**
2. Fill in name, images, price, category, description
3. Set **Status → Published**
4. Save — the product appears on the storefront immediately

### Editing the homepage hero

Go to **Admin → Homepage Hero** to change:
- Headline text
- Pill badge
- Background type (gradient / image / video)
- Overlay intensity
- CTA button labels and links

### Updating navigation

Go to **Admin → Navigation** to add, remove, or reorder nav links and update the CTA button.

### WhatsApp booking number

Go to **Admin → Site Settings** and update the **WhatsApp Number** field.  
Format: country code + number, no `+` or spaces.  
Botswana example: `26771234567`

---

## Deployment

### Database

For production, switch to PostgreSQL. Get a free database at [neon.tech](https://neon.tech) or [supabase.com](https://supabase.com), then update `DATABASE_URL` in your environment variables.

In `src/payload.config.ts`, swap the adapter:

```ts
// Install: npm install @payloadcms/db-postgres
import { postgresAdapter } from '@payloadcms/db-postgres'

db: postgresAdapter({
  pool: { connectionString: process.env.DATABASE_URL }
})
```

### Vercel

```bash
npm install -g vercel
vercel
```

Set `DATABASE_URL`, `PAYLOAD_SECRET`, and `NEXT_PUBLIC_SERVER_URL` in the Vercel dashboard under **Settings → Environment Variables**.

---

## Design system

All colours are defined as CSS custom properties in `globals.css`:

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#0e0b08` | Page backgrounds |
| `--surface` | `#1a1510` | Cards, panels |
| `--border` | `#2e2419` | Dividers, card borders |
| `--gold` | `#c9a96e` | Headlines, prices, CTAs |
| `--gold-muted` | `#8a6e3e` | Tags, section labels |
| `--text` | `#f0e6d3` | Primary text |
| `--text-muted` | `#9a8470` | Descriptions, secondary copy |
| `--text-faint` | `#5a4e42` | Timestamps, back links |

---

## Scripts

```bash
npm run dev        # Start development server
npm run build      # Production build
npm run start      # Start production server
npm run lint       # Run ESLint
```

---

## License

MIT — feel free to fork and adapt for your own projects.

---

Built by [Shaun Lefika](https://github.com/shaundotdev) · Gaborone, Botswana 🇧🇼
