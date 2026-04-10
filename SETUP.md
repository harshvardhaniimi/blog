# Blog setup & runbook

This file documents the modernized blog setup. Everything here was added/updated on 2026-04-10.

## Local development

Requires Hugo 0.160.1+ (extended). Already installed at `/usr/local/bin/hugo`.

```bash
cd ~/Dropbox/Personal/blog
hugo server                 # dev server at http://127.0.0.1:1313
hugo --gc --minify          # production build into ./public
```

Netlify's pinned Hugo version lives in `netlify.toml` (`HUGO_VERSION`).

## Publishing a new post

1. Create/edit a post under `content/posts/<slug>/index.md` (or continue the blogdown flow from RStudio).
2. Commit & push to GitHub. Netlify rebuilds automatically.
3. MailerLite's RSS campaign polls `https://blog.harsh17.in/index.xml` on its configured interval and sends the new post to subscribers (see below).

## 1. Comments (Giscus)

Giscus stores comments in a GitHub Discussions tab on a repo you own. Commenters sign in with GitHub, which kills anonymous spam.

**One-time setup:**

1. Create a public repo for comments (or reuse an existing blog repo). Example name: `harshvardhaniimi/blog-comments`.
2. Install the Giscus GitHub App on that repo: https://github.com/apps/giscus
3. Enable **Discussions** in the repo settings (Settings → General → Features → Discussions).
4. Go to https://giscus.app, fill in the form (repo name, mapping = "pathname", category = "General"). It will spit out a `<script>` with four values you care about:
   - `data-repo`
   - `data-repo-id`
   - `data-category`
   - `data-category-id`
5. Paste those into `config.yaml` under `params.giscus`. Leaving `repo` blank disables comments.

The comments partial lives at `layouts/partials/comments.html` and is included from `layouts/_default/single.html`. To hide comments on a specific post, add `comments: false` to its frontmatter.

## 2. Newsletter (MailerLite, managed, free)

MailerLite's free tier covers up to 1,000 subscribers and 12k emails/month, includes double opt-in (anti-spam subscriber verification), unsubscribe handling, and **RSS campaigns** (automatically email new feed items).

**One-time setup:**

1. Sign up at https://www.mailerlite.com/ (free plan).
2. Create an embedded signup form: MailerLite dashboard → Forms → Embedded form. Note the **Account ID** and **Form ID** shown in the snippet.
3. Paste them into `config.yaml` under `params.mailerlite`. The signup block appears at the bottom of each post automatically.
4. Create an **RSS campaign**: Campaigns → New campaign → RSS campaign.
   - Feed URL: `https://blog.harsh17.in/index.xml`
   - Check frequency: **every 6 hours** (closest to your "6h after publish" requirement — a post published just after a poll waits ~6h; on average ~3h)
   - Template: pick a simple one, use `{{rss:title}}`, `{{rss:content}}`, `{{rss:url}}` tokens
   - Send to: your subscribers list

That's the whole chain. No code in this repo touches MailerLite — it polls the RSS feed.

**Importing the old BCC list:** the BCC list from `~/Dropbox/Personal/blog-newsletter/send-letters.py` can be imported into MailerLite via Subscribers → Import → CSV. Note that anyone imported this way should get a re-confirmation (double opt-in) — MailerLite handles that.

## 3. Dark mode

`params.mode` in `config.yaml` is now `"toggle"`. A toggle icon appears in the site header. Values are `light`, `dark`, `auto`, or `toggle`.

## 4. Theme overrides

Instead of editing `themes/archie/` directly, overrides live in `layouts/`:

- `layouts/partials/footer.html` — strips the dead Google Analytics reference from the theme.
- `layouts/_default/single.html` — fixes `.Site.DisqusShortname` (removed in modern Hugo), adds comments + subscribe.
- `layouts/partials/comments.html` — Giscus embed.
- `layouts/partials/subscribe.html` — MailerLite embed.

## Social icons

The archie theme renders socials via feather-icons, which has no Threads glyph. The `layouts/partials/footer.html` override special-cases `icon: threads` and inlines an SVG from `cdn.simpleicons.org`. To add other icons not in feather, add another `if eq $key.icon "name"` branch the same way.

## Files backed up during migration

Anything I touched that had a prior version was copied to `~/Desktop/deleted by clawd/` before the edit:

- `food-choices-in-america.index.md.orig` — original with removed `{{% tweet %}}` shortcode.
- `2025-02-18-ai.index.html.orig` — original with slug `ai` (collided with the 2021 `ai-improvements` post; renamed to `ai-manifesto` with a `/ai/` alias preserved).

Nothing was deleted.
