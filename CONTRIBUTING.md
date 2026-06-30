# Contributing to Asgard API Docs

## Branch strategy

| Branch | Purpose |
|---|---|
| `main` | Production — auto-deploys to GitHub Pages. Never commit directly. |
| `develop` | Staging — merge feature branches here first for review. |
| `fix/*` or `feat/*` | Feature/fix branches — branch from `develop`, PR back to `develop`. |

## Workflow

```
1. Branch off develop:
   git checkout develop
   git pull origin develop
   git checkout -b feat/my-change

2. Make changes, commit:
   git add .
   git commit -m "docs: describe what changed"

3. Push and open PR to develop:
   git push -u origin feat/my-change
   # Open PR: feat/my-change → develop

4. After review, merge develop → main:
   git checkout main
   git merge develop
   git push origin main
   # GitHub Pages redeploys automatically
```

## Commit message convention

| Prefix | Use for |
|---|---|
| `docs:` | Content changes (new pages, corrections) |
| `fix:` | Bug fixes (broken links, wrong params) |
| `feat:` | New endpoints or sections |
| `chore:` | Config, CI, tooling changes |
