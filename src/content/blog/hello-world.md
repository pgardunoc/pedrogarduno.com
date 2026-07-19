---
title: "Hello, world"
description: "The format for build-log posts on this site — headings, code, lists, and links."
pubDate: 2026-07-19
draft: true
---

This is a draft post showing the format. It renders in `npm run dev` (with a DRAFT tag on the blog index) but is excluded from production builds until you set `draft: false`.

## A heading

Body text is plain markdown. **Bold** for emphasis, [links look like this](https://pedrogarduno.com), and `inline code` gets the mono treatment.

### Code blocks

```kotlin
fun main() {
    println("Shipped.")
}
```

### Lists

- One `.md` file per post, in `src/content/blog/`
- Frontmatter needs `title`, `description`, `pubDate` — `draft` defaults to `false`
- Commit, push, done

> Blockquotes work too — useful for pull-quotes when cross-posting excerpts to LinkedIn.
