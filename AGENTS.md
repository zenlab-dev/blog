# Agent guide — blog

This repository is the Hugo source for `https://blog.zenlab.top`.
[`CLAUDE.md`](./CLAUDE.md) has additional Claude-specific notes; this file is
the agent-neutral quick guide.

## Repo model

- `content/` contains pages and posts. Put new posts under `content/blog/`,
  normally grouped by year.
- `themes/diary/` is the bundled Hugo Diary theme. This repo does not use a
  root-level `layouts/` directory; edit the theme templates directly when
  template changes are needed.
- `hugo.toml` is the main site config.
- `wrangler.jsonc` and `.github/workflows/deploy.yml` publish the built static
  site to Cloudflare Pages.
- Generated output goes to `public/` and must not be committed.

## Commands

Requires Hugo Extended.

```bash
make help
make serve
make build
make preview
make new title="Post title"
```

Equivalent direct commands:

```bash
hugo server --disableFastRender
hugo --minify
HUGO_ENVIRONMENT=production hugo --minify
```

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
