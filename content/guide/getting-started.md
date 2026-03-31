---
title: Getting Started
description: What you need, how the pieces fit together, and the fastest path to your first working agent.
---

# Getting Started

Your AI agent team runs on [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — Anthropic's AI coding assistant that runs in your terminal. Each agent is a Claude Code session with specific instructions, a persona, and a set of skills.

There is no server to deploy. No accounts to create. Everything lives as files in your project directory.

## How It Works

```
your-project/
  CLAUDE.md                  # Agent instructions, rules, persona
  initiatives/
    agent-team.md            # Team structure and domains
  .claude/
    skills/
      daily-review/
        SKILL.md             # Reusable workflow definition
      sprint-planning/
        SKILL.md
```

When you run `claude` in this directory, it reads `CLAUDE.md` and knows who the agents are, what they can do, and how they should behave. Skills are invoked with `/skill-name` commands. That's it.

## Prerequisites

1. **Claude Code** — [install it](/guide/install-claude-code) via npm or Homebrew
2. **A project directory** — any folder where you track your work
3. **An Anthropic API key** — Claude Code will prompt you on first run

Optional but recommended:
- **MCP servers** for tools you use (Notion, GitHub, Google Calendar)
- **Telegram bots** for mobile agent access

## Setup Path

### 1. Run the Onboarding Wizard

Go to the [onboarding wizard](/onboard). It asks about your mission, projects, and pain points, then recommends which agents and skills you need. At the end, it generates a ZIP with your configured files.

### 2. Extract Into Your Project

```bash
unzip ai-cabinet-config.zip -d ~/my-project
```

This creates your `CLAUDE.md`, `agent-team.md`, and `.claude/skills/` directory with skill definitions tailored to your selection.

### 3. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Run it once to authenticate:

```bash
cd ~/my-project
claude
```

### 4. Test Your First Agent

Start with something concrete:

```
/daily-review
```

Or just talk to it naturally:

```
"What's on my calendar today? Process my inbox and propose a plan."
```

Claude reads the instructions from `CLAUDE.md`, assumes the agent persona, and executes the skill.

### 5. Add Integrations (Optional)

Agents become more powerful with tool access:

- **Notion MCP** — for task boards, databases, and project management
- **GitHub MCP** — for issues, PRs, and code review
- **Google Calendar** — via [gog CLI](https://github.com/patsnapops/gog) for scheduling
- **Telegram bots** — for messaging agents from your phone ([setup guide](/guide/telegram-bots))

Configure these in your Claude Code settings (`~/.claude/settings.json`).

## What the Wizard Generates

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Your mission, vision, rules, agent team reference, and project registry |
| `initiatives/agent-team.md` | Full agent definitions with S3 domains, responsibilities, and constraints |
| `.claude/skills/*/SKILL.md` | One skill file per selected workflow (daily review, sprint planning, etc.) |

These are plain markdown files. You own them completely — edit, extend, or rewrite anything.

## Next Steps

- [Installing Claude Code](/guide/install-claude-code) — detailed setup and key concepts
- [Setting Up Your First Agent](/guide/first-agent) — start with Maya (Chief of Staff)
- [Skills System](/guide/skills) — how skills work and how to create your own
