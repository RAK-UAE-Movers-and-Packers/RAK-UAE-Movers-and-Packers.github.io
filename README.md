# RAK UAE Movers and Packers Website

Static website for **RAK UAE Movers and Packers**, a movers and packers service in Ras Al Khaimah, UAE.

The site is built with a small Node.js static generator. It outputs plain HTML, CSS, images, sitemap, and robots files into `dist/site`, which is intended to be served by Cloudflare Pages.

## What This Site Includes

- English, Arabic, Russian, and Hindi pages.
- A root English landing page at `/`.
- Language-specific home pages under `/en/`, `/ar/`, `/ru/`, and `/hi/`.
- Local SEO landing pages for Ras Al Khaimah service areas such as Al Hamra Village, Mina Al Arab, Al Marjan Island, Al Nakheel, and Al Dhait.
- SEO metadata, canonical URLs, `hreflang` alternates, sitemap, robots file, Open Graph metadata, and MovingCompany JSON-LD structured data.
- WhatsApp call-to-action links using the business phone number.
- Responsive layout for phone, tablet, and desktop screens.

## Project Structure

```text
.
|-- assets/
|   |-- icons/              # Globe and language flag icons
|   `-- styles.css          # Website styling
|-- docs/
|   `-- maintenance.md      # Content, SEO, and deployment guide
|-- src/
|   |-- build.mjs           # Static site generator
|   `-- site.config.mjs     # Business details, languages, pages, and areas
|-- package.json            # Build and validation scripts
`-- dist/site/              # Generated site output, ignored by git
```

## Requirements

- Node.js 20 or newer is recommended.
- `npm` must be available.

There are no runtime server dependencies. The generated site is static.

## Local Development

Build the site:

```bash
npm run build
```

Validate generated HTML:

```bash
npm run validate
```

The generated website will be written to:

```text
dist/site
```

To preview locally with any static server:

```bash
npx serve dist/site
```

## Cloudflare Pages Setup

Use these settings in Cloudflare Pages:

```text
Repository: RAK-UAE-Movers-and-Packers/website
Branch: main
Framework preset: None
Build command: npm run build
Build output directory: dist/site
Root directory: leave empty
```

If Cloudflare asks for a Node version, set this environment variable:

```text
NODE_VERSION=20
```

## Updating Site Content

Most business and page content lives in:

```text
src/site.config.mjs
```

Common updates:

- Change the phone or WhatsApp number in `site.phoneDisplay`, `site.phoneHref`, and `site.whatsappNumber`.
- Change the production domain in `site.baseUrl`.
- Edit translated page copy inside the `languages` object.
- Add, rename, or remove service-area pages in the `areas` array.

After any content change, run:

```bash
npm run validate
```

See [docs/maintenance.md](docs/maintenance.md) for more detailed editing and deployment notes.

## Deployment Flow

1. Edit `src/site.config.mjs`, `src/build.mjs`, or `assets/styles.css`.
2. Run `npm run validate`.
3. Commit and push changes to `main`.
4. Cloudflare Pages builds with `npm run build`.
5. Cloudflare serves the generated files from `dist/site`.

## Generated Files

Do not edit files inside `dist/site` directly. They are generated and ignored by git. Edit the source files instead, then run the build again.

