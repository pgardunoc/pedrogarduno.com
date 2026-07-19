# Pedro Garduno - Personal Website

## About
This is Pedro Garduno's personal resume/portfolio website + blog, deployed on Vercel at https://pedrogarduno.com (repo: pgardunoc/pedrogarduno.com). The .net domain (pedrogarduno.net) redirects to .com via GoDaddy domain forwarding.

## Stack
- Astro, static output only — no SSR, no adapter, zero client JS except Google Analytics
- Shared layout `src/layouts/Base.astro` (head/meta/OG, fonts, GA, nav, footer); all CSS in `src/styles/global.css`
- Blog: markdown posts in `src/content/blog/*.md` (frontmatter: title, description, pubDate, draft). Drafts render in dev only. RSS at `/rss.xml`, sitemap at `/sitemap-index.xml`
- Self-hosted Geist / Geist Mono fonts in `public/assets/fonts/` (woff2, variable weights)
- Resume PDF at `public/assets/Resume-PedroGarduno.pdf` (linked from hero and footer) — URL path `/assets/Resume-PedroGarduno.pdf` must never break
- Google Analytics: G-TF8428JCHL (in Base.astro and 404.astro — keep on every page)
- Node 20+ (`.nvmrc`), deps: astro, @astrojs/rss, @astrojs/sitemap only

## Key Info
- Pedro is a Senior Software Engineer at The Walt Disney Studios (Oct 2025 - Present)
- Previously: Edward Jones (Dec 2022–Jul 2025), Zappos (2014–2016), Oracle (2021–2022) — 18+ years of experience
- Founder of four live products: StoryKept (storykept.com), Solid Remote Jobs (solidremotejobs.com), JetLag XI (jetlagxi.com), VettLab (vettlab.io)
- LinkedIn: https://www.linkedin.com/in/pgarduno
- Email: pgardunoc@gmail.com (canonical — pedrodev7@gmail.com is an alternate, do not use it publicly)
- Tech: Kotlin, Java, Spring Boot, Docker, Postgres, AWS, Kubernetes, Redis

## Design Goals
- Dark, minimal, high-conversion layout (Geist type, #4ade80 green accent)
- Hero → founder projects → experience → contact footer
- Prominent LinkedIn link, resume download, and contact info
- Mobile-responsive (single media query at 820px)
