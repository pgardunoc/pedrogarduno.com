# Publishing a blog post

The whole flow: **create one `.md` file → fill the frontmatter → push to `master`.** Vercel builds and deploys in about a minute. Everything else — blog index, RSS feed, sitemap, meta/OG tags — updates automatically from that one file.

## TL;DR

```bash
cd ~/code/pgardunoc.github.io          # local folder still has the old repo name
nvm use                                 # needs Node 20+ (.nvmrc says 22)
# create src/content/blog/my-post.md (see frontmatter below)
npm run dev                             # preview at localhost:4321/blog
git add . && git commit -m "Post: my post title" && git push
# ~1 min later: live at pedrogarduno.com/blog/my-post
```

## 1. Create the file

Posts live in `src/content/blog/`. **The filename is the URL slug:**

```
src/content/blog/how-i-built-jetlagxi.md  →  pedrogarduno.com/blog/how-i-built-jetlagxi
```

Use short, lowercase, hyphenated names — they're permanent once shared (changing the filename changes the URL and breaks links you've posted on LinkedIn).

## 2. Frontmatter

Every post starts with this block:

```yaml
---
title: "How I built JetLag XI's difficulty model"
description: "A build log: Elo ratings, travel logistics, and a 10,000-run Monte Carlo sim."
pubDate: 2026-07-19
draft: true        # optional — remove or set false to publish
---
```

What each field drives:

| Field | Required | Used for |
|---|---|---|
| `title` | yes | Post page `<h1>`, browser tab, `og:title` (the bold text in a LinkedIn unfurl), RSS |
| `description` | yes | Blog index blurb, meta description, `og:description` (unfurl subtitle), RSS |
| `pubDate` | yes | Sort order on the index, displayed date, `article:published_time`, RSS |
| `draft` | no (defaults `false`) | `true` = never built to production |

Write the `description` like a hook, not a summary — it's the first thing people read in the LinkedIn/Google preview.

## 3. Write the body

Plain markdown below the frontmatter. Supported out of the box:

- Headings (`##`, `###`), **bold**, links, lists, blockquotes, `---` dividers
- Code blocks with syntax highlighting (Shiki, built into Astro):
  ```
  ```kotlin
  fun main() = println("Shipped.")
  ```
  ```
- Images: put the file in `public/assets/blog/` and reference it absolutely:
  ```markdown
  ![Calibration chart](/assets/blog/jetlagxi-calibration.png)
  ```

`src/content/blog/hello-world.md` is a living example of the format.

## Drafts and previews

Three preview tiers, in order of friction:

1. **Local**: keep `draft: true`, run `npm run dev` — the post renders at `/blog/<slug>` and shows on the index with a DRAFT tag. Production builds skip it entirely.
2. **Vercel preview URL**: push the draft to a branch (`git checkout -b post/my-post && git push -u origin post/my-post`). Vercel builds every branch and gives it a preview URL (visible on the deployment in the Vercel dashboard, or in the PR if you open one). Note: preview builds are production builds — `draft: true` posts won't appear. To proofread a post on a preview URL, set `draft: false` on the branch; it only goes public when merged to `master`.
3. **Publish**: `draft: false` (or delete the line), merge/push to `master`.

⚠️ The repo is public — anything pushed to *any* branch is readable on GitHub. Keep sensitive drafts local until you're ready.

## After publishing — the LinkedIn flow

1. Confirm the post is live: `https://pedrogarduno.com/blog/<slug>`
2. Check the unfurl with the [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/) — paste the post URL; it shows exactly the card LinkedIn will render (title/description come from your frontmatter). Inspecting also busts LinkedIn's cache if you edited the post.
3. Write the LinkedIn post as a hook + excerpt, link to the full post. The blockquote style in posts works well for pull-quotes.
4. Watch traffic in [Google Analytics](https://analytics.google.com) (property `G-TF8428JCHL`) — traffic from LinkedIn shows under Acquisition.

## What updates automatically on every deploy

- `/blog` index (newest first, drafts excluded)
- `/rss.xml` feed
- `/sitemap-index.xml` (Google re-crawls it on its own schedule)
- Per-post meta tags, Open Graph tags, canonical URL

No manual SEO steps per post. Ever.

## Fixing mistakes

- **Typo in a published post**: edit the `.md`, push. Live in a minute.
- **Unpublish**: set `draft: true`, push. The URL 404s (LinkedIn links will break — prefer editing over unpublishing).
- **Bad deploy / site broken**: Vercel dashboard → Deployments → pick the last good one → ⋯ → *Instant Rollback*. Then fix at leisure with `git revert`.

## Reference

- **Site**: https://pedrogarduno.com (Vercel; `www` 308-redirects to apex; pedrogarduno.net forwards via GoDaddy)
- **Repo**: https://github.com/pgardunoc/pedrogarduno.com — production branch `master`
- **Stack details**: see `README.md` and `CLAUDE.md`
- **Frontmatter schema** (source of truth): `src/content.config.ts`
