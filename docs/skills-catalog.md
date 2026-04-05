# Skills Catalog

Recommended skills per agent role. Install with `claude install-skill <url>`.

Skills are independent of the framework — install only what you need.

## Core (all presets)

| Skill | Purpose | Install |
|-------|---------|---------|
| `process-inbox` | GTD inbox processing | `claude install-skill https://github.com/razbakov/skills/tree/main/skills/process-inbox` |
| `scrum` | Check status of all dispatched agents | `claude install-skill https://github.com/razbakov/skills/tree/main/skills/scrum` |
| `org-coach` | S3-based organizational coaching | `claude install-skill https://github.com/razbakov/skills/tree/main/skills/org-coach` |
| `workflow` | Multi-step task orchestration | `claude install-skill https://github.com/razbakov/skills/tree/main/skills/workflow` |
| `inbox` | Fire-and-forget agent dispatch | `claude install-skill https://github.com/razbakov/skills/tree/main/skills/inbox` |

## Chief of Staff (ops)

| Skill | Purpose |
|-------|---------|
| `daily-review` | Orchestrate full daily review (meta-skill with 10 child skills) |
| `weekly-review` | Saturday review + next week planning |
| `suggest-tasks` | Scan projects and propose prioritized agent tasks |
| `dispatch-approved` | Dispatch approved tasks from task board |
| `daily-review-telegram-inbox` | Export and classify Telegram saved messages |
| `daily-review-browser-history` | Chrome history summary |
| `daily-review-browser-bookmarks` | Chrome bookmarks as GTD inbox |
| `daily-review-ai-transcripts` | Claude/Conductor session summaries |
| `daily-review-app-usage` | macOS app focus time tracking |
| `daily-review-sync-projects` | Pull all repos, sync project status |
| `daily-review-suggest-plan` | Propose today's schedule |
| `daily-review-calendar-sync` | Push plan to Google Calendar |
| `daily-review-save-review` | Save daily review to sessions/ |

## CTO (engineering)

| Skill | Purpose |
|-------|---------|
| `writing-plans` | Design implementation plans from specs |
| `test-driven-development` | TDD before implementation |
| `github-issue` | Implement a GitHub issue |
| `developing-tickets` | Develop tickets by ID |
| `github-next-issue` | Pick and start next issue |
| `run-sprint` | Dispatch all sprint issues in parallel |
| `sprint-planning` | Select stories, estimate, create issues |
| `estimation` | Story point estimation |
| `pr-review-responder` | Address PR review comments |
| `review-all-prs` | Fix reviews on all open PRs |
| `simplify` | Review changed code for quality |
| `dependency-vuln-report` | Security audit of dependencies |
| `nuxt-seo-audit` | SEO setup for Nuxt sites |
| `bdd-patterns` | BDD Given-When-Then structure |
| `bdd-scenarios` | Write effective BDD scenarios |
| `bdd-from-ux` | BDD scenarios from existing UI |

## Content Lead

| Skill | Purpose |
|-------|---------|
| `content-seo-agent` | SEO audits, blog posts, content calendar |
| `viral-threads` | Transform content into social media threads |
| `website-voice-audit` | Brand voice consistency check |
| `youtube-metadata-updater` | YouTube title/description/chapters/tags |
| `video-podcast-producer` | Split recordings into clips, thumbnails |
| `image-from-gemini` | AI-generated images |
| `image-from-html` | Code-first poster/graphic generation |
| `brand-poster` | Poster/flyer/social media graphics |
| `social-post` | Cross-platform content distribution |
| `x-post` | Post tweets and threads |
| `transcribe-via-faster-whisper` | Transcribe video/audio locally |

## Strategy Lead

| Skill | Purpose |
|-------|---------|
| `product-coach` | Mission/vision -> hypothesis -> JTBD -> story map |
| `user-story` | Transform requirements into INVEST stories |
| `estimation` | Story point sizing |
| `sprint-planning` | Sprint scope and milestone planning |
| `project-start` | README -> user journey -> story map -> architecture |
| `sales-bizdev-agent` | Lead pipeline, outreach, deal tracking |
| `freelance-job-hunt` | Search freelance platforms for opportunities |
| `portfolio-hiring-analysis` | Skill gaps and hiring recommendations |
| `storyteller-tactics` | Craft narratives for pitches |

## Personal Coach

| Skill | Purpose |
|-------|---------|
| `personal-coach` | Assessment, check-in, unblock, journal |
| `year-review` | Level 10 Life scoring and action plan |
| `org-coach` | S3-based coaching sessions |

## Community Lead

| Skill | Purpose |
|-------|---------|
| `agent-browser` | Web research for contact enrichment |
| `dogfood` | Test community platforms |
