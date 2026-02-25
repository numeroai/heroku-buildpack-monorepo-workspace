# Heroku Monorepo Workspace Buildpack

Drop-in replacement for [lstoll/heroku-buildpack-monorepo](https://github.com/lstoll/heroku-buildpack-monorepo) that preserves Yarn workspace infrastructure.

## What it does

1. Copies `APP_BASE` subdirectory to the build root (same as the original monorepo buildpack)
2. Preserves `packages/`, `yarn.lock`, `.yarnrc.yml`, and `.yarn/` from the repo root
3. Patches the root `package.json` with `workspaces` and `packageManager` fields so Heroku's Node.js buildpack uses Yarn 4 and resolves `workspace:*` dependencies

## Setup

```bash
# Replace the existing monorepo buildpack (position 1)
heroku buildpacks:set https://github.com/numeroai/heroku-buildpack-monorepo-workspace -i 1 -a <app-name>

# Ensure APP_BASE is set
heroku config:set APP_BASE=numero_server -a <app-name>
```

## Required config vars

- `APP_BASE` — the subdirectory containing the app (e.g., `numero_server`)
- `NPM_AUTH_TOKEN` — GitHub Packages auth token (for `@numeroai` scoped packages, if fetching from registry)
