---
title: Projects & Organizations
description: Set up your projects and scale them into autonomous organizations.
---

Once your team is running, you will want to bring in your actual projects.

## Individual Projects

Open any project folder in Claude Desktop and say:

> "Help me set up this project"

This invokes `/product-coach` which guides you through:
- **Discovery** — why does this project exist? What problem does it solve?
- **JTBD** — what jobs are your users hiring this product to do?
- **User Journey** — what does the experience look like step by step?
- **Validate** — sketch approaches, prototype, test with real users
- **Build** — story map, backlog, architecture, then Viktor starts coding

It works for new ideas and existing projects. For existing ones, it reviews what you have and fills gaps.

## Organizations

Some projects grow big enough to need their own agent team. That is an organization.

> "Turn this project into an organization"

This invokes `/org-coach` which creates:
- A primary driver — why does this org exist?
- Domains — who owns what?
- Roles and agents — each domain gets its own AI agent
- A Coordinator agent that runs the whole org
- Governance — policies, review cadence, decision-making

The org lives in `~/Orgs/<name>/` with its own `.claude/agents/`. A routing agent in your ikigai folder connects it to Maya's dispatch.

## What is the Difference?

| | Project | Organization |
|--|---------|-------------|
| **Scale** | One repo, one focus | Multiple repos, multiple concerns |
| **Agents** | Uses your ikigai team (Viktor, Luna, etc.) | Has its own agent team + Coordinator |
| **Governance** | Your OKRs drive it | Has its own driver, domains, policies |
| **Autonomy** | You dispatch tasks to it | Runs on autopilot, reports back |
| **Example** | A blog, a side project, a client website | A SaaS product, a community, a business unit |

## Migrating an Existing Project

1. Open the project in Claude Desktop
2. Say "Help me set up this project" — `/product-coach` reviews what exists
3. It fills in missing pieces: README, strategy, user journey, backlog
4. If it needs its own team: "Turn this into an organization" — `/org-coach` takes over
