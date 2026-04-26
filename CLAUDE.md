# CLAUDE.md

This file gives Claude (and any other AI assistant) the working context required to safely edit the Umplify website. The site is a Jekyll project deployed via GitHub Pages at [https://umplify.com](https://umplify.com).

## What this site is

Umplify is a Toronto-based AI transformation and Azure cloud platform engineering partner. The website is a marketing and lead-generation site for software-led businesses, ISVs, scale-ups, and enterprise teams in Canada and the United States.

Primary visitor goals (in priority order):

1. Book a free discovery call (`/contact/`).
2. Understand the AI transformation and Azure platform service offering.
3. Read insight content on the blog at `/blog/`.

## Tech stack

- **Static site generator:** Jekyll, deployed via GitHub Pages.
- **Theme:** [`mmistakes/minimal-mistakes`](https://github.com/mmistakes/minimal-mistakes) v4.24.0 via `remote_theme` (do not vendor the theme).
- **Markdown:** kramdown with GFM input.
- **Plugins (must stay GitHub Pages compatible):**
  - `jekyll-paginate`
  - `jekyll-sitemap`
  - `jekyll-gist`
  - `jekyll-feed`
  - `jekyll-include-cache`
  - `jekyll-redirect-from`
- **Hosting:** GitHub Pages with custom domain via `CNAME`.
- **Analytics:** Google gtag (`G-WSEKFYVPNE`) configured in `_config.yml`.

If a change requires a plugin not on the GitHub Pages allow-list, do not add it. Find another approach.

## Repository layout

```
_config.yml              Site config (title, plugins, defaults, analytics)
_data/
  navigation.yml         Top nav order and labels
  ui-text.yml            Theme UI strings (do not edit lightly)
_includes/               Theme overrides and partials
  head/custom.html       Custom head: favicon, OG tags, JSON-LD schemas
  masthead.html          Top nav (overrides theme default)
  footer.html, etc.
_layouts/                Theme layouts (single, splash, home, archive, etc.)
_pages/                  All non-blog pages (services, home, about, contact, faq)
_posts/                  Blog articles. Filename pattern: YYYY-MM-DD-slug.md
_sass/                   Theme SCSS overrides
assets/
  images/
    revamp/              Current hero/diagram SVGs
    splash/              Feature-row icons
    logos/               Brand logos
robots.txt               Allows all, points to sitemap.xml
CNAME                    umplify.com
```

## Content authoring rules

### Brand voice

- **Plain, senior, production-first.** The reader is a CTO, VP Engineering, founder, or senior architect. Speak in their language.
- **No fluff.** Every sentence either establishes credibility or drives a decision.
- **No em dashes (—).** Use commas, periods, semicolons, parentheses, or restructure the sentence. (See `memory/feedback_no_em_dashes.md` in the user's memory.)
- **Canadian English** spellings where they differ (e.g., "modernize" is fine, "centre" not "center" only in proper nouns; the site uses US-style "modernize/optimize").
- **Avoid hype words:** "revolutionary", "cutting-edge", "10x", "game-changer", "transformative" without proof.
- **Outcomes before features.** Lead with what the reader gets, then explain how.
- **CTA ladder.** Every important page should end with at least one primary CTA (`Book a free discovery call`) and one secondary (e.g., `How we engage`, `Read the FAQ`).

### Page front matter conventions

Service pages should look like this:

```yaml
---
title: "Service Name"
permalink: /service-slug/
layout: single
author_profile: false
service_schema: true        # renders Schema.org Service JSON-LD via _includes/head/custom.html
excerpt: "One sentence used for meta description and OG/Twitter card."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.22"
  overlay_image: /assets/images/revamp/hero-ai.svg   # AI pages
  # or /assets/images/revamp/hero-cloud.svg          # platform/Azure pages
redirect_from:              # optional, requires jekyll-redirect-from
  - /old-url/
---
```

Landing pages (home, ai-transformation, cloud-solutions) use `layout: splash` and feature rows. See `_pages/home.md` for the canonical pattern.

### Permalinks

Service pages use top-level slugs (`/agentic-workflow-design/`). Legacy URLs under `/cloud-solutions/...` are kept as `redirect_to:` stubs for SEO continuity. Do not break existing permalinks. If a page is retired, replace its content with a stub:

```yaml
---
title: "..."
permalink: /old/url/
sitemap: false
redirect_to: /new/url/
---
```

### Internal linking

Every service page should:

1. Link to at least three related service pages.
2. Link to `/contact/`.
3. Link to `/how-we-engage/` if appropriate.

Use markdown link syntax with leading slash: `[Label](/path/)`.

### Images

- Hero/header images live in `assets/images/revamp/`.
- Feature row icons live in `assets/images/splash/`.
- Always include alt text on `![alt](path)`.
- SVG is preferred. Avoid raster heroes when an SVG exists.
- The site `og_image` is `/assets/images/revamp/hero-ai.svg`. Some social platforms reject SVG OG images; prefer setting a page-specific `header.overlay_image` so the head template emits a usable URL.

### SEO and structured data

`_includes/head/custom.html` already emits:

- `Organization` JSON-LD on every page.
- `Service` JSON-LD when a page has `service_schema: true` in its front matter.
- `FAQPage` JSON-LD on `/faq/` (kept in sync with `_pages/faq.md`).
- `BlogPosting` JSON-LD on blog posts.

If you add a new FAQ question on `_pages/faq.md`, also update the `FAQPage` JSON-LD block in `_includes/head/custom.html`. They must stay synchronized.

If you add a new service page, set `service_schema: true` to get Service JSON-LD for free.

### Navigation

Top nav is driven by `_data/navigation.yml`. Keep the list short (target eight items or fewer). New service detail pages should be cross-linked from the relevant landing page (`/ai-transformation/` or `/cloud-solutions/`), not added to the top nav.

## Blog posts

Blog posts live in `_posts/` with filenames `YYYY-MM-DD-slug.md`. They are excluded from the audit process Claude runs on marketing pages. **Do not modify blog articles unless explicitly asked.** When writing new posts:

- No em dashes (this is a project-wide rule, not blog-only).
- Use the same kramdown/GFM conventions as existing posts.
- Posts inherit `layout: single` and other defaults from `_config.yml`.

## Local development

```bash
bundle install
bundle exec jekyll serve
# open http://127.0.0.1:4000/
```

Built output goes to `_site/`. **Never commit `_site/`.**

## Things to avoid

1. **Em dashes anywhere in site content.**
2. Adding plugins outside the GitHub Pages allow-list.
3. Editing files inside the `mmistakes/minimal-mistakes` theme directly. Override via `_includes/`, `_layouts/`, or `_sass/` instead.
4. Changing existing permalinks without a `redirect_from:` (or `redirect_to:` stub) covering the old URL.
5. Committing build artifacts (`_site/`, `.jekyll-cache/`, `.sass-cache/`).
6. Hard-coding the site URL. Use `{{ site.url }}` or `| absolute_url` / `| relative_url` filters.

## Useful tasks for Claude

- **Add a new service page.** Copy the front matter pattern above, set `service_schema: true`, write the body, cross-link from `_pages/ai-transformation.md` or `_pages/cloud-solutions.md`, and link related pages from inside the new page.
- **Retire a page.** Replace its content with the `redirect_to:` stub pattern shown above and verify nothing else in the repo links to it (`grep -r "/old-url/" _pages _data _includes`).
- **Add a new FAQ question.** Edit both `_pages/faq.md` and the `FAQPage` JSON-LD block inside `_includes/head/custom.html`.
- **Update the brand description.** Update `_config.yml` (`title`, `description`, `subtitle`) and re-check `_includes/head/custom.html` `Organization` JSON-LD if structural details change.
- **Audit content for em dashes before commit.** `grep -rn "—" _pages _data _includes _config.yml _posts`. The result should be empty.

## Contact for questions

Repository owner: Arash Sabet (`arash.sabet@umplify.com`). The site reflects positioning that has been deliberately chosen; large strategic shifts (positioning, pricing language, ICP) should be confirmed before being shipped.
