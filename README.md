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

Create `src/content/blog/my-post-slug.md` with `title` / `description` / `pubDate` frontmatter, commit, push to `master` — live in about a minute. Add `draft: true` while writing (renders in dev only).

**Full guide — frontmatter reference, drafts/previews, images, the LinkedIn flow, rollbacks: [PUBLISHING.md](PUBLISHING.md)**

## Developing

```
npm install
npm run dev       # local dev server (drafts visible)
npm run build     # production build to dist/
```

Requires Node 20+ (`.nvmrc` says 22).
