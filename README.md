# Claude Code — Share & Learn

Slidev presentation deck covering three shifts in how to use Claude Code effectively, presented by Duy Bui on April 21, 2026.

**Live:** [claude-share.urieljsc.com](https://claude-share.urieljsc.com)

## Tech Stack

| Component | Technology |
|-----------|------------|
| Presentation | Slidev 0.49 |
| Frontend | Vue 3.4 |
| Build | Vite 5 |
| Container | Docker (Node 20 + nginx) |
| CI/CD | GitHub Actions (tex-corver) |
| Hosting | Kubernetes (uriel namespace) |

## Development Commands

```bash
# Start dev server with live preview
npm run dev

# Build static site to dist/
npm run build

# Export to PDF
npm run export
```

## Project Structure

```
slides.md              # Main presentation source (Slidev markdown)
claude-code-talk.md    # Speaker script and detailed notes
style.css              # Custom Slidev CSS overrides
vite.config.ts         # Vite configuration
Dockerfile             # 3-stage build (deps → builder → nginx)
nginx.conf             # Static serving with SPA fallback
.github/workflows/     # CI/CD deployment pipeline
```

## Presentation Outline

1. **Core Concepts** — Context, CLAUDE.md, permission modes
2. **Live Demo** — Plan-first workflow (prompt → design doc)
3. **Extending Claude Code** — Skills, MCP servers (Notion, pencil.dev)
4. **Brainstorm First, Code Later** — Greenfield and bug fix patterns

## Deployment

Pushes to `master`/`main` trigger automatic deployment via GitHub Actions:

1. Docker multi-stage build (`npm run build` → static HTML)
2. Pushed to Kubernetes cluster via tex-corver reusable workflow
3. Served at `claude-share.urieljsc.com` on port 80 with nginx

Manual deploy: trigger via GitHub Actions workflow dispatch.

## Resources

| Resource | Description |
|----------|-------------|
| [Speaker Script](claude-code-talk.md) | Detailed talk notes and talking points |
| [Deployment Plan](docs/superpowers/plans/2026-04-21-deploy-slidev.md) | Slidev deployment implementation plan |
