# Jekyll Personal Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold a Minima-themed Jekyll blog with an About page in this repo and deploy it via GitHub Pages at `https://hongchaodeng.github.io/`.

**Architecture:** Pages-native build (Approach A from spec). Push source to `main`; GitHub Pages runs Jekyll automatically using the `github-pages` gem. No CI workflow, no local Jekyll install — iteration is push-and-view.

**Tech Stack:** Jekyll, Minima theme, `jekyll-feed`, `jekyll-seo-tag`, GitHub Pages.

**Spec:** `docs/superpowers/specs/2026-05-09-jekyll-personal-site-design.md`

**Note on testing:** This is static-site config, not application code. There are no unit tests; verification is "GitHub Pages built successfully and the site loads." That happens in the final task.

---

## File Structure

Files to create at the repo root:

| File | Responsibility |
|---|---|
| `_config.yml` | Site metadata (title, URL, author), theme selection, plugin list |
| `Gemfile` | Pins to `github-pages` gem so Pages knows which Jekyll version |
| `.gitignore` | Excludes Jekyll build artifacts |
| `index.md` | Homepage; uses Minima's `home` layout to list posts |
| `about.md` | About page at `/about/`; auto-added to nav by Minima |
| `_posts/2026-05-09-welcome.md` | Starter post so the homepage isn't empty |

`README.md` already exists and is left untouched.

---

### Task 1: Create `_config.yml`

**Files:**
- Create: `_config.yml`

- [ ] **Step 1: Write the file**

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

- [ ] **Step 2: Verify YAML is valid**

Run: `ruby -ryaml -e "YAML.load_file('_config.yml')" && echo OK`
Expected: prints `OK`. If it errors, fix the YAML and retry.

---

### Task 2: Create `Gemfile`

**Files:**
- Create: `Gemfile`

- [ ] **Step 1: Write the file**

```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

No verification step — Pages will build with the latest `github-pages` gem regardless; the file just needs to exist and declare it.

---

### Task 3: Create `.gitignore`

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: Write the file**

```
_site/
.jekyll-cache/
.jekyll-metadata
vendor/
.bundle/
```

---

### Task 4: Create `index.md`

**Files:**
- Create: `index.md`

- [ ] **Step 1: Write the file**

```markdown
---
layout: home
---
```

That's the entire file (front matter only). Minima's `home` layout renders the posts list automatically — no body needed.

---

### Task 5: Create `about.md`

**Files:**
- Create: `about.md`

- [ ] **Step 1: Write the file**

```markdown
---
layout: page
title: About
permalink: /about/
---

Short bio paragraph here. Edit freely.
```

The `title: About` front matter causes Minima to add this page to the header nav automatically.

---

### Task 6: Create the starter post

**Files:**
- Create: `_posts/2026-05-09-welcome.md`

- [ ] **Step 1: Create the `_posts/` directory if needed and write the file**

```bash
mkdir -p _posts
```

Then write `_posts/2026-05-09-welcome.md`:

```markdown
---
layout: post
title: "Welcome"
date: 2026-05-09
---

First post.
```

**Filename rule:** Jekyll requires `YYYY-MM-DD-slug.md`. If the date prefix is missing or malformed, the post is silently skipped.

---

### Task 7: Commit all site files

- [ ] **Step 1: Verify the files exist**

Run: `ls -la _config.yml Gemfile .gitignore index.md about.md _posts/2026-05-09-welcome.md`
Expected: all six files listed; no "No such file" errors.

- [ ] **Step 2: Stage and commit**

```bash
git add _config.yml Gemfile .gitignore index.md about.md _posts/2026-05-09-welcome.md
git commit -m "Scaffold Jekyll site with Minima theme and starter post"
```

- [ ] **Step 3: Verify commit landed cleanly**

Run: `git log -1 --stat`
Expected: shows the commit with all six files added; working tree clean.

---

### Task 8: Push to GitHub

- [ ] **Step 1: Push to `origin/main`**

Run: `git push origin main`
Expected: push succeeds. Includes both the spec commit (already in `main`) and the scaffold commit.

If push is rejected (e.g., remote has diverged), inspect with `git fetch && git log origin/main..main` and `git log main..origin/main` and resolve before continuing.

---

### Task 9: Enable GitHub Pages

This is a manual step in the GitHub UI — `gh` CLI is not installed locally, so the executing agent must instruct the user.

- [ ] **Step 1: Tell the user to enable Pages**

Show this exact instruction to the user:

> Open `https://github.com/hongchaodeng/hongchaodeng.github.io/settings/pages` in your browser. Under **Build and deployment**:
> - **Source:** `Deploy from a branch`
> - **Branch:** `main` / `/ (root)`
>
> Click **Save**. The first build will queue immediately. Tell me when it's saved.

- [ ] **Step 2: Wait for user confirmation**

Do not proceed to verification until the user confirms Pages is configured.

---

### Task 10: Verify the deployed site

- [ ] **Step 1: Wait for the first build**

First builds typically take 30–60 seconds. Build status is visible at:
`https://github.com/hongchaodeng/hongchaodeng.github.io/actions`

- [ ] **Step 2: Fetch the homepage**

Run: `curl -sI https://hongchaodeng.github.io/ | head -1`
Expected: `HTTP/2 200`. If `404`, the build hasn't finished or Pages source isn't configured — wait 30s and retry, or recheck Settings → Pages.

- [ ] **Step 3: Confirm the About page resolves**

Run: `curl -sI https://hongchaodeng.github.io/about/ | head -1`
Expected: `HTTP/2 200`.

- [ ] **Step 4: Confirm the post is rendered**

Run: `curl -s https://hongchaodeng.github.io/ | grep -o 'Welcome' | head -1`
Expected: prints `Welcome` (the post title appears in the homepage list).

- [ ] **Step 5: Confirm the RSS feed exists**

Run: `curl -sI https://hongchaodeng.github.io/feed.xml | head -1`
Expected: `HTTP/2 200`. (Sanity check that `jekyll-feed` is active.)

- [ ] **Step 6: Report success to the user**

Summarize: live URL, About URL, feed URL, and the workflow for adding new posts (create `_posts/YYYY-MM-DD-slug.md`, commit, push, wait ~30s).

---

## Self-Review Notes

- **Spec coverage:** All six files from the spec's File Layout section have a creation task (Tasks 1–6). The deploy-flow steps from the spec (push to main, enable Pages source, verify build) map to Tasks 7–10. The "writing a new post" workflow is reported to the user in Task 10 Step 6.
- **No placeholders:** Each task contains the literal file contents or exact command. No "TBD" or "implement appropriately."
- **Type consistency:** N/A (no types — all config/markdown).
- **Edge case — push rejection:** Task 8 has a fallback path if `git push` fails.
- **Edge case — Pages not configured yet:** Task 10 Step 2 tells the agent to retry/recheck if a 404 appears.
