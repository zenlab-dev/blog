# Agent guide — blog

This repository is the Hugo source for `https://blog.zenlab.top`.
[`CLAUDE.md`](./CLAUDE.md) has additional Claude-specific notes; this file is
the agent-neutral quick guide.

## Repo model

- `content/` contains pages and posts. Put new posts under `content/blog/`,
  normally grouped by year.
- `themes/diary/` is the bundled Hugo Diary theme (not a submodule — edit
  templates directly). Theme components:
  - `layouts/` — Go templates: `_default/baseof.html` (HTML skeleton),
    `_default/single.html` (post view), `index.html` (homepage), partials
    for sidebar, TOC, head, comments, etc.
  - `assets/scss/` — 3-layer SCSS: `liquid-glass.scss` (glassmorphism design
    system), `journal.scss` (core theme), `modern.scss` (UI enhancements).
  - `static/js/` — `journal.js` (core), `code-enhance.js` (copy buttons,
    line numbers), `liquid-glass.js` (dynamic effects), `toc.js` (scroll spy).
  - `i18n/` — 10 languages (zh-CN default, en, es, fr, de, it, ko, nl,
    pt-br, zh-hant).
- `hugo.toml` is the main site config (baseURL, theme, menus, taxonomies,
  KaTeX/Mermaid, comment systems, Open Graph/Twitter Cards, reading time).
- `wrangler.jsonc` and `.github/workflows/deploy.yml` publish the built
  static site to Cloudflare Pages.
- Generated output goes to `public/` — must not be committed.

## Commands

Requires Hugo Extended.

```bash
make help           # List all targets
make serve          # Dev server localhost:1313
make serve-lan      # LAN-accessible dev server (0.0.0.0)
make build          # Production build (hugo --minify)
make preview        # Build + Python HTTP preview
make clean          # Remove public/ and resources/
make new title="Post title"  # New blog post
```

## Content features

- **GitHub-style alerts**: `> [!NOTE]`, `> [!TIP]`, `> [!IMPORTANT]`,
  `> [!WARNING]`, `> [!CAUTION]` render as styled boxes (via
  `render-blockquote.html` render hook)
- **KaTeX/LaTeX**: Enabled in `hugo.toml` for math rendering
- **Mermaid diagrams**: Enabled for flowcharts/diagrams in code fences
- **Bilibili embeds**: Shortcode `{{< bilibili BVID >}}`
- **Image processing**: Shortcode `{{< insertFigure src="" caption="" >}}`
  with Hugo Fit/Fill/Resize pipeline
- **Comments**: 7 supported systems (Disqus, Gitalk, Livere, Twikoo, Waline,
  Utterances, Giscus) — all currently disabled in config
- **Code blocks**: Auto language badges, copy buttons, line numbers
- **SEO**: Open Graph, Twitter Cards, JSON-LD, Google Analytics, Microsoft Clarity

## SCSS architecture

1. `liquid-glass.scss` — Apple-inspired glassmorphism design system with CSS
   custom properties, mouse-driven dynamic lighting, scroll blur
2. `journal.scss` — Core layout, typography (CJK support), post cards,
   sidebar, TOC, pagination, responsive breakpoints
3. `modern.scss` — Enhanced code blocks, GitHub alerts, reading progress bar,
   back-to-top, image lightbox, archive/friends grid, mobile/print/iOS safe
   area optimizations

## Rules of the road

- Keep content and comments in the language appropriate for the article.
- Do not add private Cloudflare tokens, Twikoo credentials, analytics secrets,
  or generated `public/` output.
- Twikoo is currently disabled in `hugo.toml`; do not enable it without a real
  `twikooEnvId`.
- Clean up generated `public/` output, temporary files, and background Hugo
  servers before finishing.
- Deployment is automatic on pushes to `main` through GitHub Actions and
  Cloudflare Pages.

## Validation

Before proposing changes done:

```bash
hugo --minify
git diff --check
```

For visual changes, run `make serve` or `hugo server --disableFastRender` and
inspect the affected page.
