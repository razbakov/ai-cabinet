## Configuration

- Issue tracker: GitHub Issues (repo: samchen/hq)
- Org path: `~/Orgs/hq`
- Media path: `~/Local/hq` (NOT git — large files, recordings, attachments)

### Project Path Registry

| Project | Git Repo Path |
|---------|--------------|
| my-saas | `~/Projects/my-saas` |
| blog | `~/Projects/blog` |

### Agent Team

Sam is the Commander (human in the loop). Makes strategic decisions, is the public face, validates hypotheses with real people, and reviews/approves agent outputs. Everything else is delegated.

Each agent's full config (persona, skills, OKRs, boundaries) lives in `.claude/agents/<name>.md` — source of truth.

| Agent | Role | File |
|-------|------|------|
| **Aria** | Chief of Staff — dispatches tasks, runs daily reviews, tracks progress | `.claude/agents/aria.md` |
| **Rex** | CTO — writes code, ships features, manages technical debt | `.claude/agents/rex.md` |

**Triage flow:** Sam sends message -> Aria classifies -> routes to Rex (code). If it's operations (calendar, contacts, reviews), Aria handles it directly. Aria never does engineering work.

**Decision Authority:**

| Decision | Decides | Advises |
|----------|---------|---------|
| Product direction | Sam | Rex |
| Architecture | Rex | Sam |
| Sprint priorities | Sam | Rex |
| Daily task order | Aria | Sam |
| PR merge | Rex | Sam (review) |

## Rules — Shortcuts

<!-- Add your custom shortcuts here -->

## Rules — Agent Operations

- Every dispatched agent must have a task card.
- Dispatched agents must create a PR as their final step.
- Agent reports must show deliverables (PR links, file sizes), not just status.
- Do not sign messages with agent names — each agent has its own bot.

## Rules — Daily Review

- Daily review saved to `ops/sessions/YYYY-MM-DD-daily-review.md`.
- Weekly reviews in `ops/reviews/YYYY-MM-DD-weekly-review.md`.
- Daily review must check last weekly review for missed priorities.
- Save strategic summaries to files immediately — file first, message second.

## Rules — Contacts

- When adding a new contact, spawn a subagent to research online and enrich the profile.
- Chat attachments (media) live in `~/Local/hq/contacts/<name>/`, not in the git repo.

## Rules — General

- Always search GitHub before claiming something doesn't exist.
- Orgs (multi-repo, governance) go under `~/Orgs/<org-name>`. Single-repo projects go under `~/Projects/<project-name>`.

## Processing Instructions

### Contacts

Each person: `contacts/<firstname-lastname>/contact.md` with frontmatter (name, type, projects, location, contact, status). Chat text logs in repo, media attachments in `~/Local/hq/contacts/<name>/`.

### Sessions

Daily logs in `ops/sessions/`. Three types:
- **Browser history** (`YYYY-MM-DD-browser.md`): Chrome History SQLite -> time block summary
- **AI transcripts** (`YYYY-MM-DD-ai-sessions.md`): Claude JSONL -> project groups
- **Other** (`YYYY-MM-DD-topic.md`): Check-ins, working sessions
