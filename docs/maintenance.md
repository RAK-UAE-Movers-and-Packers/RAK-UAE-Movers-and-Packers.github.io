# Website Maintenance Guide

This repository uses a simple static generator instead of hand-maintaining every HTML page. The generator reads `src/site.config.mjs` and writes the full website to `dist/site`.

## Key Files

```text
src/site.config.mjs
```

Main content file. Edit this for business details, phone numbers, language copy, SEO text, and service areas.

```text
src/build.mjs
```

Static generator. Edit this only when changing page structure, metadata behavior, sitemap generation, or output paths.

```text
assets/styles.css
```

Global styling for all generated pages.

```text
assets/icons/
```

Globe and flag images used by the language switcher.

## Build Output

The build command is:

```bash
npm run build
```

It generates:

```text
dist/site/
|-- index.html
|-- sitemap.xml
|-- robots.txt
|-- assets/
|-- en/
|-- ar/
|-- ru/
`-- hi/
```

Cloudflare Pages should publish `dist/site`, not the repository root.

## Cloudflare Pages Configuration

Recommended setup:

```text
Framework preset: None
Build command: npm run build
Build output directory: dist/site
Root directory: leave empty
```

Recommended environment variable:

```text
NODE_VERSION=20
```

The production URL currently used by SEO tags and the sitemap is configured in `src/site.config.mjs`:

```js
baseUrl: "https://rak-uae-movers-and-packers.pages.dev"
```

Change this value if the final production domain changes, then run `npm run validate` and deploy again.

## Updating Contact Details

Edit these fields in the exported `site` object:

```js
phoneDisplay: "+971 55 145 7235",
phoneHref: "+971551457235",
whatsappNumber: "971551457235",
```

Use this format:

- `phoneDisplay`: human-readable number shown on the site.
- `phoneHref`: E.164-style phone number used by `tel:` links.
- `whatsappNumber`: digits only, used by `wa.me` links.

After editing, run:

```bash
npm run validate
```

## Updating the Domain

Update:

```js
baseUrl: "https://example.com"
```

This affects:

- Canonical URLs.
- `hreflang` alternate URLs.
- Open Graph URLs.
- JSON-LD structured data URLs.
- `sitemap.xml`.
- `robots.txt` sitemap location.

Do not add a trailing slash to `baseUrl`.

## Updating Languages

Language content is stored in the `languages` object. Each language includes:

- `code`
- `label`
- `dir`
- `locale`
- `hreflang`
- `brand`
- `home`
- `areaTemplate`
- `services`
- `process`
- `faq`

When editing Arabic, keep:

```js
dir: "rtl"
```

Phone numbers in the generated HTML are wrapped with left-to-right direction handling so they display correctly inside Arabic text.

## Adding a New Service Area

Add a new object to the `areas` array:

```js
{
  slug: "new-area-slug",
  name: {
    en: "New Area",
    ar: "Arabic area name",
    ru: "Russian area name",
    hi: "Hindi area name"
  },
  prep: {
    ru: "Russian prepositional phrase if needed"
  },
  keywords: ["New Area movers", "New Area packers"]
}
```

The generator will create one page per language:

```text
/en/new-area-slug/
/ar/new-area-slug/
/ru/new-area-slug/
/hi/new-area-slug/
```

It will also include those pages in the sitemap and language switcher.

## SEO Notes

The generator creates:

- Unique page titles and meta descriptions.
- Canonical links.
- `hreflang` alternates for all language versions.
- `x-default` alternate links.
- `sitemap.xml` with alternate language links.
- `robots.txt`.
- MovingCompany JSON-LD structured data.

For best SEO results:

- Keep page titles specific and under roughly 60 characters when practical.
- Keep meta descriptions specific and under roughly 160 characters when practical.
- Avoid copying the exact same paragraph across every service-area page.
- Keep area names natural in each language.
- Regenerate the site after every domain, language, or area change.

## Validation

Run:

```bash
npm run validate
```

This builds the site and validates generated HTML under `dist/site`.

## Common Problems

### Cloudflare shows a 404

Check that the build output directory is:

```text
dist/site
```

Also confirm the build command is:

```text
npm run build
```

### Styles or icons do not load

Confirm `assets/` exists inside `dist/site`. The build script copies the source `assets` folder into the output folder.

### SEO URLs use the wrong domain

Update `site.baseUrl` in `src/site.config.mjs`, then run:

```bash
npm run validate
```

### Edited HTML disappeared

Generated files should not be edited directly. Edit `src/site.config.mjs`, `src/build.mjs`, or `assets/styles.css`, then rebuild.

