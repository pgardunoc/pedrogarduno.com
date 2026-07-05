# pedrogarduno.com

Personal site of Pedro Garduno — Senior Software Engineer & Founder. Hosted on GitHub Pages at [pedrogarduno.com](https://pedrogarduno.com) (pedrogarduno.net redirects here).

## Stack

- A single static `index.html` — no frameworks, no build step, all CSS inline
- Self-hosted [Geist / Geist Mono](https://vercel.com/font) fonts in `assets/fonts/`
- Resume PDF at `assets/Resume-PedroGarduno.pdf`

## Structure

```
index.html                        the whole site
CNAME                             custom domain for GitHub Pages
assets/
  Resume-PedroGarduno.pdf         resume (linked from hero and footer)
  fonts/                          Geist & Geist Mono woff2 subsets
  img/                            favicon + apple-touch-icon
```

## Deploying

Push to `master` — GitHub Pages builds and serves automatically. To update the resume, replace `assets/Resume-PedroGarduno.pdf` and push.
