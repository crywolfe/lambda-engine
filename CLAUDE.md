# Lambda Engine — Claude Code Context

## Repository Map

| Repo | Domain | Branch |
|------|--------|--------|
| `crywolfe/lambda-engine` (this repo) | `lambda.gerrywolfe.com` | `main` |
| `crywolfe/crywolfe.github.io` | `gerrywolfe.com` | `master` |

## Deployment

Changes to `gerrywolfe.html` in this repo do **not** auto-deploy to `gerrywolfe.com`. To deploy:

1. Copy `gerrywolfe.html` → `index.html` in `crywolfe/crywolfe.github.io`
2. Commit and push to the `master` branch of that repo
3. GitHub Pages takes 1–2 minutes to go live
4. Hard-refresh (`Cmd+Shift+R`) to bypass browser cache

```bash
gh repo clone crywolfe/crywolfe.github.io /tmp/crywolfe.github.io
cp gerrywolfe.html /tmp/crywolfe.github.io/index.html
cd /tmp/crywolfe.github.io && git add index.html && git commit -m "Sync gerrywolfe.html" && git push origin master
```
