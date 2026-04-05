# Architecture

## System Overview

```
┌─────────────────────────────────────────────────────┐
│                   Commander (Human)                  │
│  Strategic decisions, public face, approvals         │
└──────────────────────┬──────────────────────────────┘
                       │ voice notes, messages, commands
                       ▼
┌─────────────────────────────────────────────────────┐
│              Chief of Staff (Dispatch)               │
│  Classifies → routes → tracks. Never handles tasks.  │
└──┬────┬────┬────┬────┬──────────────────────────────┘
   │    │    │    │    │
   ▼    ▼    ▼    ▼    ▼
 CTO  Content Strategy Coach Community
  │      │       │      │       │
  ▼      ▼       ▼      ▼       ▼
Code   Blog    OKRs  Wellbeing  CRM
Tests  SEO     BizDev Journal  Events
Deploy Social  Pricing Routine Contacts
```

## Three Layers

### 1. Org Layer (governance)

Lives in `~/Orgs/<name>/`. Contains:
- `CLAUDE.md` — operational rules, agent table, decision matrix
- `.claude/agents/` — agent definitions (source of truth)
- `ops/` — sessions, inbox, reviews
- `contacts/` — CRM
- `strategy/` — decisions, experiments
- `profile.md`, `now.md` — personal context for coaching

No application code lives here. This is the logbook.

### 2. Project Layer (code)

Lives in `~/Projects/<name>/`. Each project has its own:
- `CLAUDE.md` — project-specific rules
- `.claude/agents/coordinator.md` — project coordinator agent
- Source code, tests, CI/CD

### 3. Bridge Layer (project agents)

Project agents in the org layer route tasks to project coordinators:

```
Org: ~/Orgs/ikigai/.claude/agents/wedance.md
  → cd ~/Orgs/WeDance
  → claude --agent coordinator
  → Coordinator handles the task in project context
```

## Agent Types

### Executive Agents (6 roles)

| Role | Domain | Tools | Key Constraint |
|------|--------|-------|---------------|
| Chief of Staff | Ops, dispatch | Read, Glob, Grep, Bash, TodoWrite, WebSearch, WebFetch | Never handles tasks directly |
| CTO | Engineering | All tools | Creates PRs for major changes |
| Content Lead | Blog, SEO, social | Read, Write, Glob, Grep, Bash, TodoWrite, WebSearch, WebFetch | Content must be reviewed before publishing |
| Strategy Lead | OKRs, biz dev | Read, Glob, Grep, Bash, TodoWrite, WebSearch, WebFetch | Recommends only, human decides |
| Personal Coach | Wellbeing | Read, Write, Glob, Grep, TodoWrite | Never prescribes, only suggests |
| Community Lead | CRM, events | Read, Write, Glob, Grep, Bash, WebSearch, WebFetch, TodoWrite | Never represents human externally |

### Project Agents (thin routers)

3 lines of instructions: `cd` to project, run coordinator, pass through the request.

## Communication Channels

### Claude Code (primary)

Every agent is available via `claude --agent <name>`. This is the default interaction mode.

### Telegram (optional)

Each agent gets a dedicated Telegram bot. Architecture:

```
User sends message to @maya_bot
  → telegram-bots.py receives it
  → Pastes into agent's tmux window (claude --agent maya)
  → Agent processes with full tool access
  → Agent replies via telegram-send.py (fire-and-forget)
```

Self-healing features:
- **TTL**: Sessions auto-restart every 4 hours to prevent context bloat
- **Watchdog**: If agent doesn't respond within 2 minutes, restart and re-send
- **Security**: Only responds to the configured owner Telegram ID

### Voice (future)

Butler / voice assistant integration for voice-first interaction.

## Data Flow

### Daily Review

```
6:00 AM — Data gathering (browser history, AI transcripts, app usage)
9:00 AM — Chief of Staff sends daily review DM
  → Inbox processing (Telegram, Notion, bookmarks)
  → Calendar review
  → Plan proposal
  → Calendar sync
  → Session file saved to ops/sessions/
```

### Task Dispatch

```
Human drops message
  → Chief of Staff classifies (code? content? strategy? personal? community?)
  → Routes to appropriate agent
  → Agent creates PR / content / analysis
  → Chief of Staff tracks completion in scrum report
```

### Contact Processing

```
Human meets someone
  → Sends screenshot / voice note to Community agent
  → Agent creates contacts/<name>/contact.md
  → Spawns subagent to research online
  → Enriches profile
  → Adds follow-up tasks
```

## S3 Governance

The framework uses Sociocracy 3.0 patterns:

- **Consent-based decisions**: Major decisions require human's explicit consent
- **Domain delegation**: Each agent has full autonomy within their domain constraints
- **Logbook**: Chief of Staff maintains the team logbook via daily/weekly reviews
- **Retrospectives**: Weekly review includes team retro
- **Decision Authority Matrix**: Defined in CLAUDE.md — who decides vs who advises
