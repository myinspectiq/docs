# Docs → Marketing site sync workflow

The docs repo (`github.com/myinspectiq/docs`, this repo) is the **source of truth**
for the Help Centre served at `https://myinspectiq.com/docs`. The marketing site
repo (`C:\Users\Zhigool\Documents\Projects\inspectiqhome`) renders these docs
natively at `/docs` using a sync script — it does NOT fetch docs live.

## When you update docs or add videos/images here

1. **Edit and push in this repo** (MDX pages under the topic folders, images in
   `images/`, videos in `videos/`). Mintlify deploys automatically from GitHub.

2. **Run the sync from the marketing site repo** (this is where the script lives,
   and it writes into that repo's `src/` and `public/` folders):
   ```bash
   cd C:\Users\Zhigool\Documents\Projects\inspectiqhome
   npm run sync-docs
   ```
   Or sync straight from a local checkout of this repo without pushing first:
   ```bash
   npm run sync-docs -- C:\Users\Zhigool\Documents\Projects\docs
   ```

3. **Commit the regenerated files** in `inspectiqhome` and deploy:
   - `src/content/docs/` (every MDX page, links/assets rewritten to `/docs/*`)
   - `public/docs/` (images + videos actually referenced by the pages)
   - `src/content/docs-manifest.ts` (sidebar + SEO metadata from `docs.json`)
   - `src/content/docs-urls.json` (flat URL list used by the sitemap script)

Nothing else needs updating — no manual page registration, no sitemap edits.

## Gotchas

- **Videos/images are only copied if referenced.** A new `.mp4` in `videos/`
  won't appear on the site until an MDX page embeds it.
- The script excludes repo housekeeping files (README, AGENTS.md, LICENSE,
  CHANGELOG, CONTRIBUTING, style.css, favicon.svg) — don't rely on those as pages.
- Run the sync from `inspectiqhome`, not from this repo — the script belongs to
  that project and writes into its folders. This repo has no runner.

## Where things live

| What | Where |
|---|---|
| Sync script | `inspectiqhome/scripts/sync-docs.mjs` |
| Script runner | `npm run sync-docs` (in `inspectiqhome/package.json`) |
| Full process notes | `inspectiqhome/AGENTS.md` (Notes section) |
| Docs navigation config | this repo's `docs.json` |
