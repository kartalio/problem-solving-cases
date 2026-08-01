# CLAUDE.md — project guide for Claude Code

## What this is
A static marketing + blog site for **Problem Solving Cases**, a $19 downloadable
PDF of real Revolut PM interview problem-solving cases. Checkout is handled by
Lemon Squeezy (external link). No build step, no framework — plain HTML/CSS/JS.

## Hosting
- Hosted on **GitHub Pages**, repo `kartalio/problem-solving-cases`.
- Served at the **root** of the custom domain **https://nexxround.com/**.
- A `CNAME` file (managed via Settings → Pages) holds the custom domain — don't delete it.
- The **apex** `nexxround.com` is canonical — **not** `www`. All canonicals, `og:image`,
  JSON-LD and `sitemap.xml` URLs use `https://nexxround.com/...`. `CNAME` must therefore
  read `nexxround.com`; if it reads `www.nexxround.com`, GitHub Pages 301-redirects the
  apex to `www` and every canonical points at a redirect. Change it via Settings → Pages
  (which rewrites `CNAME`), not by editing the file.

## File structure
```
index.html                     # home page (root)
sitemap.xml                    # update when adding blog posts
robots.txt
CNAME                          # custom domain (auto-managed by GitHub Pages)
legal/
  terms-and-conditions.html
  privacy-policy.html
blog/
  index.html                   # blog listing (add a card per new post)
  post-template.html           # duplicate this to create a new post
  how-to-approach-a-pm-problem-solving-case.html   # example post
images/
  logo.png  hero.webp  inside.png  og-image.png
  testimonials/
    testimonial-1.webp  testimonial-2.webp  ...   # user-managed screenshots
```

## Conventions & hard rules (don't break these)

### CSS is inlined per page
There is **no shared stylesheet**. Each HTML file has its own `<style>` block.
A design change must be applied to **every** page's `<style>` block to stay
consistent. (This was a deliberate choice so each page renders standalone.)

### Image paths depend on folder depth
- Root pages (`index.html`) reference images as `images/...`
- Pages one level deep (`blog/`, `legal/`) reference them as `../images/...`
- **Never** put `../images/` on the homepage or bare `images/` on a subpage —
  that breaks images on GitHub Pages. This bug has bitten before.
- `og:image` / canonical tags use **absolute** URLs: `https://nexxround.com/...`

### Testimonials load automatically
- The homepage JS probes `images/testimonials/testimonial-1.webp`,
  `testimonial-2.webp`, … up to 50, **in parallel**, and shows whatever exists.
- Files must be **WebP** and numbered **consecutively from 1**. The section
  hides itself if there are zero images.
- Clicking a screenshot opens a lightbox. Don't remove the lightbox markup/JS.
- To add testimonials: the user just drops in the next-numbered `.webp`. No
  code change needed. Do **not** overwrite or delete files in
  `images/testimonials/` — those are user content.

### Adding a blog post
1. Copy `blog/post-template.html` → rename with hyphenated keywords
   (e.g. `product-sense-questions.html`).
2. Fill in title, meta description, canonical URL, JSON-LD, and body.
3. Add a matching `<article class="post-card">` to `blog/index.html`.
4. Add a `<url>` entry to `sitemap.xml`.
5. Keep exactly one `<h1>` per page; use `<h2>`/`<h3>` for structure.

### SEO notes
- Legal pages are intentionally `noindex`.
- Every page needs a unique `<title>` and meta description.
- Canonicals and `sitemap.xml` must point at `https://nexxround.com`.

## Things NOT to touch without asking
- Files in `images/` and `images/testimonials/` (user-managed assets).
- The Lemon Squeezy checkout URL.
- `CNAME`.

## Handy checks
- After edits, confirm no page references the old `YOURDOMAIN.com` placeholder.
- Confirm homepage images use `images/...` and blog/legal use `../images/...`.
