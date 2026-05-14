# Deployment Guide

## Sites & Repos

| Site | Source Repo | Branch | Canonical file (this repo) | Deployed as |
|------|-------------|--------|------------------------------|-------------|
| lambda.gerrywolfe.com | crywolfe/lambda-engine | main | `lambda.html` | `index.html` in lambda-engine |
| gerrywolfe.com | crywolfe/crywolfe.github.io | master | `gerrywolfe.html` | `index.html` in crywolfe.github.io |

## Critical Rules

### lambda.gerrywolfe.com

1. **`lambda.html` is canonical** — `lambda.html` in this repo is the authoritative version. Always sync `index.html` from `lambda.html` before deploying lambda.gerrywolfe.com:
   ```bash
   cp lambda.html index.html && git add index.html && git commit -m "Sync index.html with lambda.html"
   ```

### gerrywolfe.com

2. **`gerrywolfe.html` is canonical** — `gerrywolfe.html` in this repo is the authoritative version for the personal homepage. Before deploying gerrywolfe.com, sync the GitHub Pages repo from this file (and assets):
   ```bash
   cp gerrywolfe.html /path/to/crywolfe.github.io/index.html
   cp headshot.jpg /path/to/crywolfe.github.io/headshot.jpg
   ```
   Replace `/path/to/crywolfe.github.io` with your local clone (e.g. `/tmp/crywolfe.github.io`).

3. **`headshot.jpg` must exist at root** — `gerrywolfe.html` references `headshot.jpg` at site root. Copy it into `crywolfe.github.io/` whenever you sync the homepage so the live site does not 404 the image.

## Deploy Steps

### lambda.gerrywolfe.com
```bash
cp lambda.html index.html
git add -A && git commit -m "..."
git push origin main
gh api repos/crywolfe/lambda-engine/pages/builds -X POST
```

### gerrywolfe.com
```bash
cp gerrywolfe.html /tmp/crywolfe.github.io/index.html
cp headshot.jpg /tmp/crywolfe.github.io/headshot.jpg
cd /tmp/crywolfe.github.io
git add -A && git commit -m "..."
git push origin master
gh api repos/crywolfe/crywolfe.github.io/pages/builds -X POST
```

## Wait for build

**lambda-engine:**
```bash
sleep 45
gh api repos/crywolfe/lambda-engine/pages/builds/latest --jq '{status, updated_at}'
```

**crywolfe.github.io:**
```bash
gh api repos/crywolfe/crywolfe.github.io/pages/builds/latest --jq '{status, updated_at}'
```

## CDN Caching

Both sites use Fastly CDN (`x-cache: HIT` header). After deployment:
- Verify with cache-busting:
  - `curl -s "https://lambda.gerrywolfe.com/?v=$(date +%s)"`
  - `curl -s "https://gerrywolfe.com/?v=$(date +%s)"`
- If stale, browser hard refresh: `Cmd+Shift+R`
- CDN cache expires automatically within minutes

## gerrywolfe.com Deployment Workflow

The `crywolfe.github.io` repo may have an incomplete local clone (missing `index.html`). If `git` commands fail:

1. Clone fresh: `rm -rf /tmp/crywolfe.github.io && cd /tmp && gh repo clone crywolfe/crywolfe.github.io`
2. Make changes in `/tmp/crywolfe.github.io/`
3. Commit and push from there: `cd /tmp/crywolfe.github.io && git add -A && git commit -m "..." && git push origin master`
4. Trigger deployment: `gh api repos/crywolfe/crywolfe.github.io/pages/builds -X POST`

Note: The canonical source for gerrywolfe.com content is `gerrywolfe.html` in this repo. Sync from there when updating content, or push directly to the GitHub Pages repo for quick fixes.
