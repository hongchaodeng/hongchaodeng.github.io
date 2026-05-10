# hongchaodeng.github.io

Personal blog at https://hongchaodeng.github.io/. Built with Jekyll + Minima, deployed via GitHub Pages.

## Writing a post

Create `_posts/YYYY-MM-DD-slug.md`:

```markdown
---
layout: post
title: "Post title"
date: 2026-05-09
---

Body in Markdown.
```

The `YYYY-MM-DD-` filename prefix is required — Jekyll skips files without it.

## Deploy

GitHub Pages builds and deploys automatically on every push to `main`:

```bash
git add .
git commit -m "..."
git push
```

First build takes ~30–60s, subsequent builds ~15–30s. Build status is visible at
https://github.com/hongchaodeng/hongchaodeng.github.io/actions.

## Local preview

Use Ruby 3.3 (matches the GitHub Pages production runtime). Newer Ruby versions
break Jekyll 3.9, which is what the `github-pages` gem pins.

```bash
brew install ruby@3.3
# Add to ~/.zshrc, then `exec zsh`:
export PATH="/opt/homebrew/opt/ruby@3.3/bin:$PATH"

bundle config set --local path 'vendor/bundle'
bundle install
bundle exec jekyll serve --livereload
```

Open http://localhost:4000/. Markdown edits auto-rebuild; `_config.yml` changes need a
server restart.
