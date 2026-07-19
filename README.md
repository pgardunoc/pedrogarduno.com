# pedrogarduno.com

Personal site of Pedro Garduno — Senior Software Engineer at The Walt Disney Studios & Founder. Deployed on Vercel at [pedrogarduno.com](https://pedrogarduno.com) (pedrogarduno.net redirects here).

## Stack

- [Astro](https://astro.build) static output — zero client-side JS except Google Analytics
- Markdown blog via Astro content collections (`@astrojs/rss` + `@astrojs/sitemap` for feed/SEO)
- Self-hosted [Geist / Geist Mono](https://vercel.com/font) fonts in `public/assets/fonts/`
- Resume PDF at `public/assets/Resume-PedroGarduno.pdf`

## Structure

```
astro.config.mjs                  site URL + sitemap integration
src/
  layouts/Base.astro              shared head, meta/OG, fonts, GA, nav, footer
  styles/global.css               all site CSS
  pages/
    index.astro                   the one-pager
    404.astro                     not-found page
    blog/index.astro              blog index (newest first, drafts excluded)
    blog/[slug].astro             post pages
    rss.xml.js                    RSS feed
  content/blog/*.md               posts (one markdown file each)
  content.config.ts               frontmatter schema
public/
  robots.txt
  assets/                         resume, fonts, favicons, og-image
```

## Publishing a post

1. Create `src/content/blog/my-post-slug.md` — the filename becomes the URL (`/blog/my-post-slug`)
2. Fill in the frontmatter:

   ```yaml
   ---
   title: "Post title"
   description: "One-two sentences — used in the index, meta tags, OG unfurls, and RSS."
   pubDate: 2026-07-19
   ---
   ```

   Add `draft: true` to keep it out of production while writing — drafts render in `npm run dev` (tagged DRAFT on the index) but are never built.

3. Commit and push. Vercel builds and deploys automatically; push a branch instead to get a preview URL first.

## Developing

```
npm install
npm run dev       # local dev server (drafts visible)
npm run build     # production build to dist/
```

Requires Node 20+ (`.nvmrc` says 22).
