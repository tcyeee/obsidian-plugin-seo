---
name: obsidian-plugin-seo
description: Use when the goal is to make an Obsidian plugin more discoverable through its metadata — the manifest.json/package.json description text that Obsidian's in-app community plugin browser and the community.obsidian.md plugin page search against, plus the GitHub repository's description, topics, and homepage URL. Triggers on "/obsidian-plugin-seo", "SEO优化", "插件SEO", "优化插件描述", "让更多人搜到插件", "GitHub 关键词", "repo topics", or when manifest.json's description reads as a mechanism description with no searchable benefit keywords. Scope is metadata only — does not cover forum posts, Reddit, YouTube outreach, or a standalone marketing site.
---

# Obsidian Plugin SEO

## Overview

An Obsidian plugin is discovered through two separate surfaces, and they do not share an index:

1. **Obsidian's in-app community plugin browser + `community.obsidian.md/plugins/<id>`** — both are driven by `manifest.json`'s `name` and `description` fields only. Neither indexes the README. Every word in `description` is the entire surface area for in-app search matching.
2. **GitHub** (and Google results for the repo) — driven by the repo's one-line description, its topics, the homepage URL, and the README's opening paragraph/headings. This is a completely different keyword surface and is crawled by Google.

Because these two surfaces don't overlap, optimizing only one leaves half the discovery funnel untouched. This skill audits and rewrites both in one pass.

## Step 1 — Audit the current description

```bash
cat manifest.json   # "description" field — the in-app search text
cat package.json 2>/dev/null | grep -A1 '"description"'
gh repo view --json description,homepageUrl,repositoryTopics
```

Common failure pattern: the description explains *mechanism* ("transforms the UI into X layout") but contains none of the *benefit/category* words a user would actually type into search — "minimal", "clean", "distraction-free", "zen", "focus mode", "declutter", "note-taking", "markdown editor". A mechanism-only description can be 100% accurate and still never match a real search query.

## Step 2 — Build the keyword set

Combine three kinds of terms, grounded in what the plugin actually does — never add a term the plugin doesn't truthfully deliver:

- **Benefit terms**: minimal, clean, distraction-free, zen, focus, declutter, simplify
- **Category terms**: note-taking, markdown, writing, productivity
- **Brand/mechanism terms people search directly**: e.g. "Typora-style" if the plugin mimics a known tool's look — these carry real search volume from users migrating between tools

Write one description that reads naturally (not keyword-stuffed) but hits as many of these as fit. Target length ~150–220 characters — long enough to cover several keywords, short enough to render cleanly as 2–3 lines in the plugin browser list.

**Guardrail**: the description must not start with the plugin's own name (separate Obsidian submission-bot rule — see `obsidian-plugin-lint`'s manifest.json check). An SEO rewrite can accidentally reintroduce this.

## Step 3 — Apply to manifest.json and package.json

Keep both in sync — they're the same discovery surface duplicated across two files:

```bash
# edit manifest.json "description" and package.json "description" to the same text
# if package.json has an empty "keywords": [], fill it with the same term set, e.g.:
#   ["obsidian", "obsidian-plugin", "minimal", "minimalist", "distraction-free", "zen-mode", "note-taking"]
```

## Step 4 — Apply to GitHub repo metadata

```bash
gh repo edit <owner>/<repo> \
  --description "<same or closely matching description>" \
  --homepage "<verified URL>" \
  --add-topic obsidian-plugin \
  --add-topic <other relevant topics...>
```

Rules for this step:

- **Topics**: prioritize the canonical `obsidian-plugin` topic over separate `obsidian` + `plugin` tags — `obsidian-plugin` has its own GitHub topic page (`github.com/topics/obsidian-plugin`) that people actually browse, and is the tag the rest of the Obsidian plugin ecosystem uses. Add it even if `obsidian` and `plugin` are already present; don't remove the existing ones. Then add benefit/category topics from Step 2 that are true of the plugin (e.g. `distraction-free`, `zen-mode`, `minimalist`, `note-taking`, `markdown`).
- **Homepage URL**: if the plugin is published, this should point at its canonical listing page. Obsidian's plugin pages have moved from `obsidian.md/plugins?id=<id>` to `community.obsidian.md/plugins/<id>` — **verify the live URL by fetching it before writing it into the homepage field**; never guess a URL from pattern memory alone, since Obsidian has changed this path before.
- **Repo description**: can match the manifest description exactly, or be phrased slightly more human for a developer audience browsing GitHub — either is fine as long as it carries the same keywords.

## Step 5 — Verify

```bash
cat manifest.json | grep description
cat package.json | grep -A1 '"description"\|"keywords"'
gh repo view --json description,homepageUrl,repositoryTopics
```

Confirm: no field starts with the plugin name, all three metadata surfaces (manifest.json, package.json, GitHub) carry a consistent keyword set, and the homepage URL was fetched and confirmed live rather than guessed.

## Out of scope

Forum posts (Obsidian Share & Showcase), Reddit/r/ObsidianMD demos, YouTube outreach, and a standalone marketing website are legitimate discovery channels but are a different kind of work (content/outreach, not metadata) — not covered by this skill.
