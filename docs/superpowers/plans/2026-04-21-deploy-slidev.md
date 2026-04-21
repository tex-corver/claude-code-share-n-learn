# Deploy Slidev Deck via tex-corver CI/CD Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deploy this Slidev deck to `https://claude-share.urieljsc.com` via the `tex-corver/.github` reusable workflow, so every push to `master` ships prod.

**Architecture:** A 3-stage Dockerfile (`node:20-alpine` deps → `node:20-alpine` builder running `npm run build` → `nginx:alpine` runner serving `dist/` on port 80). A single caller workflow `deploy-prod.yaml` invokes `docker-build-push.yaml@master` on pushes to `master`. Mirrors the established pattern used by `tex-corver/uriel-branding` (live at `https://profile.urieljsc.com`) so this deck slots cleanly into the existing `uriel` org namespace.

**Tech Stack:** Slidev 0.49 · Vue 3 · Node 20 · nginx alpine · Docker Buildx · GitHub Actions reusable workflows · self-hosted `arc-runner-set` · Artifactory JCR · `tex-corver/DeployConfigRepo` (K8s manifests).

**Reference implementation** (verified via `gh api` on 2026-04-21): `tex-corver/uriel-branding` — same org's Next.js static-export site. This plan adopts its file layout, port, nginx-config style, and workflow inputs wholesale. Differences: Slidev's output dir is `dist/` (vs Next.js `build/`) and there are no `NEXT_PUBLIC_*` build args to thread through.

**Context assumptions** (verified):
- `package.json` has `"build": "slidev build"` which emits static `dist/`.
- `.gitignore` already excludes `dist`, `node_modules`, `components.d.ts`.
- `slides.md` has `download: false` — no PDF export needed at build time.
- `vite.config.ts` has no `base` set → assets are served from `/`.
- No tests exist → `test-command: echo 1` (matches uriel-branding).
- Target URL: `https://claude-share.urieljsc.com`, org `uriel`, project `claude-code-share-n-learn`.

---

## File Structure

| Path | Purpose |
|---|---|
| `Dockerfile` | 3-stage: `deps` installs node_modules, `builder` runs `npm run build`, `runner` is `nginx:alpine` serving `dist/` from `/usr/share/nginx/html` on `:80`. |
| `.dockerignore` | Exclude `node_modules`, `dist`, `.git`, `.github`, docs, markdown. Forces a clean install in `deps` stage and keeps context small. |
| `nginx.conf` | Single `server` block (NOT a full nginx.conf — it's copied to `/etc/nginx/conf.d/default.conf`). Listens on `:80`, SPA-fallback to `/index.html`, long-cache for hashed assets, gzip. |
| `.github/workflows/deploy-prod.yaml` | Caller workflow. Trigger: push to `master`/`main` + manual dispatch. Invokes `tex-corver/.github/.github/workflows/docker-build-push.yaml@master`. |

Files live at repo root so `docker-build-context: .` works without changes. Workflow name `deploy-prod.yaml` mirrors `uriel-branding` for discoverability.

---

## Task 1: Add `.dockerignore`

**Files:**
- Create: `.dockerignore`

- [ ] **Step 1: Write `.dockerignore`**

```
# Dependencies
node_modules

# Build outputs
dist
.remote-assets
components.d.ts

# Git
.git
.gitignore

# IDE
.idea
.vscode
*.swp
*.swo

# Environment files
.env
.env.*
!.env.example

# Documentation
*.md
LICENSE

# Misc
.DS_Store
*.log
npm-debug.log*

# GitHub & Claude
.github
.claude
.agents

# Project-specific throwaways
claude-code-talk.md
skills-lock.json
```

- [ ] **Step 2: Commit**

```bash
git add .dockerignore
git commit -m "[chore] add .dockerignore for docker build context"
```

---

## Task 2: Add `nginx.conf`

**Files:**
- Create: `nginx.conf`

- [ ] **Step 1: Write `nginx.conf`**

```nginx
server {
    listen 80;
    server_name _;
    root /usr/share/nginx/html;
    index index.html;

    # SPA fallback — Slidev deep-links (e.g. /3) resolve to index.html
    location / {
        try_files $uri $uri.html $uri/ /index.html;
    }

    # Long-cache hashed Slidev/Vite assets
    location /assets/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Gzip
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml text/javascript image/svg+xml;
    gzip_min_length 256;
}
```

- [ ] **Step 2: Commit**

```bash
git add nginx.conf
git commit -m "[chore] add nginx server block serving Slidev build on :80"
```

Why this shape (not a full `nginx.conf`): the Dockerfile copies this file to `/etc/nginx/conf.d/default.conf`, so it's included into the base image's existing top-level `http {}` block. This matches `tex-corver/uriel-branding`.

Why port 80 (not 8000): uriel-branding uses port 80 and passes `port: 80` to the reusable workflow. Matching this keeps ops predictable across the org's apps.

---

## Task 3: Add the `Dockerfile`

**Files:**
- Create: `Dockerfile`

- [ ] **Step 1: Write `Dockerfile`**

```dockerfile
# Stage 1: Dependencies
FROM node:20-alpine AS deps

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci --ignore-scripts


# Stage 2: Builder
FROM node:20-alpine AS builder

WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
COPY . .

RUN npm run build


# Stage 3: Serve static files with nginx
FROM nginx:alpine AS runner

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

- [ ] **Step 2: Build the image locally**

Run: `docker build -t claude-share:local .`
Expected: build succeeds; `docker images claude-share` lists the `local` tag.

- [ ] **Step 3: Run the container**

Run: `docker run --rm -d -p 8080:80 --name claude-share-test claude-share:local`
Expected: `docker ps` shows `claude-share-test` on `0.0.0.0:8080->80/tcp`.

- [ ] **Step 4: Smoke-test the endpoint**

Run: `curl -sfI http://localhost:8080/ | head -1 && curl -sf http://localhost:8080/ | grep -o '<title>[^<]*</title>'`
Expected:
- first line: `HTTP/1.1 200 OK`
- second line: `<title>Claude Code — Share & Learn</title>` (or similar, sourced from `slides.md`)

- [ ] **Step 5: Stop the container**

Run: `docker stop claude-share-test`
Expected: container stops cleanly.

- [ ] **Step 6: Commit**

```bash
git add Dockerfile
git commit -m "[feat] add multi-stage Dockerfile for nginx-served Slidev build"
```

Why the `deps` stage: pinning a `node_modules`-only layer gives better cache reuse when only source changes — identical to the `uriel-branding` Dockerfile.

Why `dist` (not `build`): Slidev's `slidev build` command writes to `dist/` by default. Next.js's `output: "export"` in `uriel-branding` writes to `build/` (because they set `distDir: "build"` in `next.config.ts`). Only this `COPY --from=builder` line differs from their reference.

---

## Task 4: Add production deploy workflow

**Files:**
- Create: `.github/workflows/deploy-prod.yaml`

- [ ] **Step 1: Write `.github/workflows/deploy-prod.yaml`**

```yaml
name: Deploy prod

on:
  push:
    branches: [master, main]
  workflow_dispatch:

jobs:
  call-common-workflow:
    uses: tex-corver/.github/.github/workflows/docker-build-push.yaml@master
    with:
      org-name: uriel
      project-name: claude-code-share-n-learn
      tag: v1
      docker-build-file: Dockerfile
      docker-build-context: .
      port: 80
      ingress: true
      ingress-domain: claude-share.urieljsc.com
      test-command: echo 1
```

- [ ] **Step 2: Validate YAML syntax locally**

Run: `python3 -c "import yaml; yaml.safe_load(open('.github/workflows/deploy-prod.yaml'))" && echo OK`
Expected: prints `OK` with no traceback.

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/deploy-prod.yaml
git commit -m "[feat] add master deploy via tex-corver docker-build-push workflow"
```

Why `org-name: uriel` (not defaulting to `tex-corver`, the repo owner): `uriel-branding` passes `org-name: uriel`, and that's the K8s namespace / DNS prefix already in use for other uriel apps. Defaulting would isolate this deck into a separate `tex-corver` namespace — undesirable.

Why `ingress-domain: claude-share.urieljsc.com` (full FQDN, not bare `claude-share`): confirmed via `uriel-branding/.github/workflows/deploy-prod.yaml` which passes `ingress-domain: profile.urieljsc.com` to land at `https://profile.urieljsc.com`. The template appends nothing when a full FQDN is provided.

Why `tag: v1`: matches the uriel-branding convention. Promoting a new version is a manual bump to `v2`/etc — explicit beats implicit `latest`.

Why no `liveness-path` / `readiness-path`: uriel-branding omits them. The template's `docker-build-push.yaml` only runs a verify step when either is set, so omitting them means the deploy job returns success as soon as the manifest is pushed to `DeployConfigRepo`, and ingress is verified manually (Task 5).

Why no `env-file`: Slidev builds are fully static with no runtime configuration. (uriel-branding uses `env-file: .env.prod` to thread `NEXT_PUBLIC_*` vars into Next.js — not applicable here.)

Why no preview workflow: this is a slide deck, not a product. Preview-per-branch isn't worth the infra cost. If ever needed, the sibling `deploy-on-pr.yaml` template can be wired up the same way against non-master branches.

---

## Task 5: End-to-end verification

**Files:** none (verification only)

- [ ] **Step 1: Push to master to trigger deploy**

Run:
```bash
git push origin master
```
Expected: a GitHub Actions run named `Deploy prod` starts on `arc-runner-set` and executes three jobs in order: `run-test`, `build-image`, `deploy-on-dev`.

- [ ] **Step 2: Watch the run until it finishes**

Run: `gh run watch --exit-status $(gh run list --workflow=deploy-prod.yaml --limit 1 --json databaseId --jq '.[0].databaseId')`
Expected: exits 0; final status `success`. Total wall time is typically 3–5 min.

- [ ] **Step 3: Verify the public URL**

Run: `curl -sfI https://claude-share.urieljsc.com/ | head -1 && curl -sf https://claude-share.urieljsc.com/ | grep -o '<title>[^<]*</title>'`
Expected:
- `HTTP/2 200` (or `HTTP/1.1 200 OK`)
- `<title>Claude Code — Share & Learn</title>`

- [ ] **Step 4 (if Step 3 fails): Inspect what landed in DeployConfigRepo**

Run: `gh api repos/tex-corver/DeployConfigRepo/contents/uriel/claude-code-share-n-learn --jq '.[].name'`
Expected: shows `deployment.yaml`, `service.yaml`, `ingress.yaml` etc. If missing, the `deploy-on-dev` job didn't push manifests — re-check the workflow logs.

If manifests are there but the URL still 404s, ask in `#platform` to inspect `kubectl get ingress -n uriel claude-code-share-n-learn` for a host mismatch.

---

## Rollback plan

If the prod deploy is broken:
1. Revert the offending commit on `master`: `git revert <sha> && git push`. The `Deploy prod` workflow re-runs with the prior source and re-deploys.
2. Fast path if the image is broken but K8s is fine: `kubectl rollout undo deployment/claude-code-share-n-learn -n uriel` (ask `#platform` if you don't have cluster access).

There's no database or persistent state — static-site rollback is always safe.

---

## Self-review

- **Spec coverage:** The user's request was "deploy this with the tex-corver template" and "align with `uriel-branding`". Tasks 1–4 produce the four files that `uriel-branding` has (`Dockerfile`, `.dockerignore`, `nginx.conf`, `.github/workflows/deploy-prod.yaml`). Task 5 proves the URL actually serves.
- **Placeholder scan:** No `TBD`, no "handle edge cases", every code block is concrete and copy-pasteable. The only decision points (org name, ingress domain, tag) are called out inline with their rationale.
- **Type consistency:** Port `80` is identical across `nginx.conf` (`listen 80`), `Dockerfile` (`EXPOSE 80`), the workflow input (`port: 80`). The Docker stage names `deps` / `builder` / `runner` match the `COPY --from=` references. `dist` (build output) matches between `slidev build` and the `COPY --from=builder /app/dist` line. `org-name: uriel` and `project-name: claude-code-share-n-learn` appear only in the workflow, which is the single source of truth for both.
- **Parity check with `uriel-branding`:** The four files are structurally identical except for three deliberate deltas: (1) no `NEXT_PUBLIC_*` build args, (2) `/app/dist` instead of `/app/build`, (3) slightly different asset cache location (`/assets/` for Slidev vs `/_next/static/` for Next.js). Each delta is justified in the surrounding task notes.
