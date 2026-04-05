## Configuration

- Issue tracker: GitHub Issues (repo: priya-dev/agency)
- Org path: `~/Orgs/agency`
- Media path: `~/Local/agency` (NOT git — large files, recordings, attachments)

### Project Path Registry

| Project | Git Repo Path |
|---------|--------------|
| client-portal | `~/Projects/client-portal` |
| marketing-site | `~/Projects/marketing-site` |
| internal-tools | `~/Projects/internal-tools` |

### Agent Team

Priya is the Commander (human in the loop). Makes strategic decisions, is the public face, validates with clients, and reviews/approves agent outputs. Everything else is delegated.

Each agent's full config lives in `.claude/agents/<name>.md` — source of truth.

| Agent | Role | File |
|-------|------|------|
| **Nova** | Chief of Staff — ops, daily reviews, inbox, calendar, dispatch | `.claude/agents/nova.md` |
| **Atlas** | CTO — engineering, architecture, code, testing, deployments | `.claude/agents/atlas.md` |
| **Iris** | Head of Content & Growth — blog, SEO, social, visuals | `.claude/agents/iris.md` |

**Triage flow:** Priya sends message -> Nova classifies -> routes to: Atlas (code), Iris (content). Nova never handles tasks directly — she dispatches and tracks.

**Decision Authority:**

| Decision | Decides | Advises |
|----------|---------|---------|
| Client scope | Priya | Atlas |
| Architecture | Atlas | Priya |
| Content tone | Priya | Iris |
| Sprint priorities | Priya | Atlas |
| Daily task order | Nova | Priya |
| Publishing content | Priya (approval) | Iris |
| PR merge | Atlas | Priya (review) |

## Rules — Notion & Tasks

- **All tasks live in Notion.** Never add task items to markdown files.
- Every page must have S3 analysis (Tension, Driver, Requirement, Response Options).

## Rules — Agent Operations

- Dispatched agents must create a PR as their final step.
- Agent reports must show deliverables (PR links, file sizes), not just status.

## Rules — Daily Review

- Daily review saved to `ops/sessions/YYYY-MM-DD-daily-review.md`.
- Weekly reviews in `ops/reviews/YYYY-MM-DD-weekly-review.md`.

## Rules — General

- Orgs (multi-repo, governance) go under `~/Orgs/<org-name>`. Single-repo projects go under `~/Projects/<project-name>`.
