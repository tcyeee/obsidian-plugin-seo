# obsidian-plugin-seo

A [Claude Code](https://claude.com/claude-code) / Agent **skill** that optimizes an Obsidian plugin's discoverability through its metadata — nothing else.

Obsidian plugins are found through two separate, non-overlapping indexes:

- **Obsidian's in-app community plugin browser + `community.obsidian.md/plugins/<id>`** — both search only `manifest.json`'s `name`/`description`. The README is invisible to this surface.
- **GitHub / Google** — driven by the repo's description, topics, homepage URL, and README opening text.

This skill audits both, builds a benefit-and-category keyword set grounded in what the plugin actually does, rewrites `manifest.json`/`package.json` descriptions, and updates the GitHub repo's description/topics/homepage via `gh repo edit` — verifying any external URL by fetching it rather than guessing.

## Install

```bash
git clone https://github.com/tcyeee/obsidian-plugin-seo ~/.claude/skills/obsidian-plugin-seo
```

Or copy the folder into any agent's skills directory — the skill is the single `SKILL.md`.

## Use

```
/obsidian-plugin-seo
```

…or just describe the task ("优化插件描述", "让更多人搜到插件", "SEO优化").

## Out of scope

Forum posts, Reddit/YouTube outreach, and marketing sites are real discovery channels but a different kind of work (content/outreach, not metadata) — not covered here.

## License

[MIT](./LICENSE)
