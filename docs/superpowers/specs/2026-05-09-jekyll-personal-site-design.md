# Jekyll Personal Site — Design

**Date:** 2026-05-09
**Repo:** `hongchaodeng/hongchaodeng.github.io`
**Live URL:** `https://hongchaodeng.github.io/`

## Goal

A minimal personal blog with an About page, deployed via GitHub Pages.

## Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Site type | Blog | User stated primary purpose is writing posts |
| Theme | Minima | Default Jekyll theme; built into GitHub Pages with zero setup |
| Pages | Blog index + About | "Most common" shape per user choice |
| Domain | Default `*.github.io` | No custom domain wanted; trivial to add later |
| Build | Pages-native (Approach A) | Push to `main`, Pages builds automatically; no CI config |
| Local preview | Skipped | System Ruby (2.6) is too old; user prefers push-and-view loop |

## File Layout

```
hongchaodeng.github.io/
├── _config.yml
├── _posts/
│   └── 2026-05-09-welcome.md
├── about.md
├── index.md
├── Gemfile
├── .gitignore
└── README.md          # already exists, untouched
```

## File Contents

### `_config.yml`
```yaml
title: Hongchao Deng
email: hongchao.deng@anyscale.com
description: Personal blog
url: "https://hongchaodeng.github.io"
author:
  name: Hongchao Deng

theme: minima
plugins:
  - jekyll-feed
  - jekyll-seo-tag
```
Both plugins are on the GitHub Pages allowlist.

### `Gemfile`
```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```
Pins to the `github-pages` gem so local versions match what Pages runs.

### `index.md`
```yaml
---
layout: home
---
```
Minima's `home` layout renders the posts list.

### `about.md`
```yaml
---
layout: page
title: About
permalink: /about/
---

Short bio paragraph here. Edit freely.
```
Minima auto-adds any page with a `title` to the header nav.

### `_posts/2026-05-09-welcome.md`
```yaml
---
layout: post
title: "Welcome"
date: 2026-05-09
---

First post.
```
Filename must follow `YYYY-MM-DD-slug.md` or Jekyll skips it.

### `.gitignore`
```
_site/
.jekyll-cache/
.jekyll-metadata
vendor/
.bundle/
```

## Deploy Flow

### One-time GitHub setup
1. Push files to `main` branch.
2. **Settings → Pages**.
3. **Source:** Deploy from a branch. **Branch:** `main`, folder `/ (root)`.
4. Save. First build queues immediately.

### Per-push behavior
- Push to `main` → Pages detects commit → runs Jekyll → publishes.
- First build ~30–60s; subsequent ~15–30s.
- Build status visible in **Actions** tab and **Settings → Pages**.

### Writing a new post
1. Create `_posts/YYYY-MM-DD-slug.md` with `layout: post`, `title`, `date` front matter.
2. Write Markdown body.
3. `git add && git commit && git push`.
4. Refresh `https://hongchaodeng.github.io/` in ~30s.

### Failure handling
- Build failures email the commit author with the error.
- Common causes: malformed YAML front matter, post filename missing `YYYY-MM-DD-` prefix, plugin not on allowlist.
- Rollback: revert the bad commit and push.

## Out of Scope

- Custom domain
- Comments (Disqus, utterances, etc.)
- Analytics
- Tags/categories pages
- Dark mode
- Search

These are easy to add later without restructuring.
