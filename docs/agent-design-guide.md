# Agent Design Guide

How to customize, add, or remove agents in your ikigai setup.

## Agent File Structure

Every agent lives in `.claude/agents/<name>.md` with this structure:

```markdown
---
name: agent-name           # lowercase, used in CLI: claude --agent agent-name
description: >             # shown in agent picker, keep under 2 lines
  One-line summary of what this agent does.
tools:                     # optional — omit to grant all tools
  - Read
  - Write
  - Bash
model: opus                # opus, sonnet, or haiku
color: cyan                # terminal color for the agent
---

You are AgentName — OwnerName's Role Title AI agent.

## Persona
<!-- Communication style, MBTI energy, signature questions -->

## Team
<!-- List ALL other agents so this agent knows who to hand off to -->

## Domain
<!-- What this agent handles -->

## Boundaries
<!-- What this agent does NOT do, and who to hand off to -->

## Current OKRs
<!-- Quarterly objectives and key results -->

## Available Skills
<!-- Skills this agent can invoke -->

## Message Handling
<!-- How to respond to different message types -->

## Memory
<!-- Where to read/write persistent memory -->
```

## Adding a New Agent

1. **Define the role**: What gap exists? What tasks are falling through?
2. **Create the file**: `.claude/agents/<name>.md`
3. **Set boundaries**: List what it does NOT do with explicit hand-off targets
4. **Update all other agents**: Add the new agent to every other agent's `## Team` section
5. **Update CLAUDE.md**: Add to the agent table and triage flow
6. **Create memory dir**: `mkdir -p .claude/agent-memory/<name>/`

## Removing an Agent

1. Delete `.claude/agents/<name>.md`
2. Remove from every other agent's `## Team` section
3. Remove from CLAUDE.md agent table
4. Redistribute responsibilities to remaining agents
5. Clean up `.claude/agent-memory/<name>/`

## Customizing an Agent

### Renaming

Change the `name` in frontmatter and the file name. Update all references in other agent files and CLAUDE.md.

### Changing persona

Edit the `## Persona` section. The persona affects tone but not capabilities. A warm persona and a direct persona can do the same tasks.

### Adjusting boundaries

The `## Boundaries` section is critical for correct routing. Rules:
- Every boundary should name who to hand off to
- Boundaries should be symmetric — if A can't do X and says "hand off to B", then B's domain should include X
- The Chief of Staff should never have "handles directly" — only dispatches

### Adding skills

Add the skill name and description to the `## Available Skills` section. The agent will invoke it when the task matches.

### Restricting tools

Use the `tools` frontmatter to limit what an agent can do:
- **CTO**: No tool restriction (needs all tools for engineering)
- **Personal Coach**: No Bash, no WebSearch (pure reflection, no system access)
- **Content Lead**: No Edit (creates content files but shouldn't modify code)

## Design Principles

### 1. Clear domains, no overlap

Each agent owns a distinct domain. If two agents could handle the same task, one of them shouldn't exist — merge or split until domains are clean.

### 2. Boundaries name hand-offs

"You do NOT write code" is incomplete. "You do NOT write code (hand off to Viktor)" tells the agent what to do instead.

### 3. The Chief of Staff is pure dispatch

The ops agent classifies, routes, and tracks. It never executes substantive work. This prevents bottlenecks and keeps routing clean.

### 4. Every agent knows the full team

Each agent's `## Team` section lists all other agents. This prevents agents from trying to do things outside their domain because they didn't know someone else could handle it.

### 5. OKRs drive prioritization

Every agent has OKRs. Before taking action, agents check: "does this support my KRs?" This prevents scope creep and keeps work aligned.

## Project Router Agents

For projects with their own org/coordinator, create thin router agents:

```markdown
---
name: my-project
description: >
  My Project agent. Switches to ~/Orgs/MyProject and runs Coordinator.
model: opus
color: purple
---

You route My Project tasks. Do not do the work yourself.

1. `cd ~/Orgs/MyProject`
2. Run the Coordinator agent: `claude --agent coordinator`
3. Pass through the user's request as-is
```

These are 3-line agents. No persona, no OKRs, no skills — just routing.
