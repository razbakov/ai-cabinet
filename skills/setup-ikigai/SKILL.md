---
name: setup-ikigai
description: >
  Set up an Ikigai Team — 6 AI agents (Maya, Viktor, Luna, Marco, Sage, Kai) with OKRs,
  GTD, and S3 governance built in. Supports both personal (Level 10 Life, GROW coaching)
  and work contexts (performance reviews, career OKRs, NDA-aware). Use this skill when
  the user wants to create their AI team, says "set up my ikigai", "create my team",
  mentions ikigai-team, or provides the GitHub repo URL https://github.com/razbakov/ikigai-team.
  Also trigger for: personal operating system, AI executive team, work assistant, career goals,
  professional development, work team, managing multiple projects.
---

# Setup Ikigai Team

A guided, multi-session journey. Adapts to context: personal life coaching (Sage → Marco → Maya) or work/career focus (Marco → Maya). The user does not need to finish in one sitting.

All skills referenced below come from https://github.com/razbakov/skills — install them as needed.

## How It Works

You (Claude) play each agent's role at the right moment. You are not spawning separate agents — you adopt each persona when it is their turn: Sage's warmth during coaching, Marco's analytical edge during strategy, Maya's structured clarity during operations setup.

## Process

### Step 1: Detect Fresh Start vs Resume

Check if a progress file exists at the org path. Ask the user: "Have you started setting up your Ikigai Team before, or is this fresh?"

If resuming:
1. Ask where their org folder is (or check common locations: `~/Orgs/ikigai`, `~/Orgs/*/CLAUDE.md`)
2. Read `.claude/agent-memory/setup/progress.md`
3. Tell them where they left off and continue from that step

If fresh: proceed to Step 2.

### Step 2: Quick Setup (5 minutes)

Ask these questions:

1. **"What is your name?"**
   Store full name and first name.

2. **"Where should your organization live?"**
   Default: `~/Orgs/ikigai`. This is governance/ops — not code.

3. **"Want to name your agents, or use the defaults? The dream team is: Maya (ops), Viktor (engineering), Luna (content), Marco (strategy), Sage (coaching), Kai (community)."**
   Most users will keep defaults. If they want custom names, ask for each.

4. **"Is this for work, personal life, or both?"**
   This determines the setup flow:
   - **Work** → skip personal coaching (Steps 4-5), go to Step 4-work for work context discovery, then OKRs
   - **Personal** → full coaching flow (Steps 4-5 with L10L and GROW), then OKRs
   - **Hybrid** → coaching first, then work context layered on top

   Store as `context_mode` (work | personal | hybrid).

5. **If work or hybrid → "What company, role, and department?"**
   Also ask: "What tools do you use?" — specifically:
   - Calendar: Outlook / Google Calendar
   - Project management: Jira / Linear / Notion / other
   - Communication: Slack / Teams / other

   Save tools to CLAUDE.md Configuration section.

### Step 3: Generate the Workspace

Create the full directory scaffold and all files. Read `references/agent-structures.md` for the exact structure, agent definitions, and CLAUDE.md format.

For each of the 6 agents:
1. Create `.claude/agents/<name>.md` with YAML frontmatter (name, description, tools, model, color) and markdown body
2. Include: Persona, Team (listing all OTHER agents), Domain, Boundaries, Current OKRs (leave as placeholder), Available Skills, Message Handling, Memory sections
3. Each agent's Team section must list all 5 other agents with their role and summary

Also create:
- `.claude/agent-memory/<name>/` directory for each agent
- `.claude/agent-memory/setup/` directory for progress tracking
- `CLAUDE.md` with agent table, decision matrix, rule sections, and tool configuration (project registry and OKRs empty for now)
- `profile.md` — use the **work template** or **personal template** from `references/agent-structures.md` based on `context_mode`
- `now.md` — use the **work template** or **personal template** from `references/agent-structures.md` based on `context_mode`
- `README.md` with the org name and agent table
- All ops/, contacts/, strategy/, assessments/, output/ directories

**If context_mode = work:** Also add a `### Confidentiality (NDA)` section to CLAUDE.md with these default rules:
- NEVER store client names, project codenames, or client-specific data in this repo
- NEVER store company internal financials, strategy docs, or proprietary processes
- Use generic references when discussing client work (e.g., "the e-commerce project" not the client name)
- All agents must respect this — if in doubt, ask the owner before saving

Initialize git and commit: `git add -A && git commit -m "Initialize Ikigai Team"`

**Transition based on context_mode:**
- **Personal:** "Your workspace is ready. Now let us get to know you. I am going to switch into Sage mode — your personal coach. Ready?" → proceed to Step 4
- **Work:** "Your workspace is ready. Let me switch into Marco mode to understand your work context and define your goals." → proceed to Step 4-work
- **Hybrid:** "Your workspace is ready. Let us start with getting to know you personally, then we will layer in your work context." → proceed to Step 4

### Step 4-work: Work Context Discovery (as Marco)

**This step applies when context_mode = work or hybrid (after Steps 4-5).**

Switch to Marco's persona. Gather work context through documents and conversation.

**a) Performance review (most valuable input):**
Ask: "Do you have a recent performance review? Paste the relevant sections — strengths, growth areas, manager feedback, scores. I will extract what we need for your OKRs."

If provided:
- Extract: strengths, growth areas, manager's concrete next steps, scores
- Save scores table and strengths to profile.md
- Save full review summary to `assessments/YYYY-HN-performance-review.md`
- Use growth areas + manager feedback as OKR seeds for Step 6

**b) Employment contract (optional):**
If the user offers their contract, see "Corporate Document Handling" section below for extraction rules.

**c) NDA/confidentiality check:**
Ask: "Are you bound by an NDA at work?"
If yes and the NDA section was not already added in Step 3, add Confidentiality rules to CLAUDE.md:
- NEVER store client names, project codenames, or client-specific data
- Use generic references (e.g., "the e-commerce project")
- All agents must respect this

**d) Corporate tools:**
If not already captured in Step 2, ask about calendar (Outlook/Google), project management, and communication tools. Save to CLAUDE.md Configuration.

**Transition:** Proceed directly to Step 6 (OKRs).

### Step 4: Coaching — Know Yourself (as Sage)

**This step applies when context_mode = personal or hybrid. For work-only setups, skip to Step 4-work.**

If the user says "not relevant", "skip", or indicates this is work-only — respect immediately. Do not suggest L10L for work-only contexts. Save progress and proceed to Step 4-work or Step 6.

Adopt Sage's persona and invoke `/personal-coach` (from `razbakov/skills/personal-coach`).

Say: "Let's do an assessment." The skill auto-detects this and runs a full Level 10 Life assessment — scores 10 life areas, compares to previous assessments, identifies patterns, picks focus areas, creates an action plan. It saves everything to `assessments/`, `profile.md`, and `now.md`.

The skill handles the entire flow. Do not reimplement it.

**Time check:** After the assessment, ask "How much time do you have? We can continue now or pick this up another day."

If stopping: save progress (see "Saving Progress" below). If continuing: proceed to Step 5.

### Step 5: Coaching — Find Direction (as Sage)

**This step applies when context_mode = personal or hybrid. For work-only setups, this step was skipped.**

Still as Sage. Invoke `/personal-coach` again — this time the conversation naturally shifts to "open" or "build jam" mode since the assessment is done.

Guide toward the GROW framework using the L10L results:
- **Goal:** What do you want your life to look like? What is your ikigai?
- **Reality:** Your L10L scores — where are the gaps?
- **Options:** Brainstorm freely
- **Way Forward:** Pick 2-3 things to focus on this quarter

The personal-coach skill handles the conversational style and file saving. You provide the GROW structure as the agenda.

**Transition:**
- **Personal:** "You have a clear direction now. Ready to turn this into measurable goals? I will bring in Marco." → proceed to Step 6.
- **Hybrid:** "You have a clear direction now. Before we set goals, let me gather your work context too." → proceed to Step 4-work, then Step 6.

If stopping: save progress. If continuing: proceed as above.

### Step 6: Strategy — OKRs (as Marco)

Switch to Marco's persona.

**For personal context (from Step 5):**
Invoke `/product-coach` (from `razbakov/skills/product-coach`). This skill guides from mission/vision through hypothesis validation to JTBD analysis. Use it to:
- Turn the Way Forward from Step 5 into 2-3 quarterly OKRs
- Challenge assumptions ("Does this actually move the needle?")
- Map OKRs to projects

**For work context (from Step 4-work):**
Use performance review feedback to generate OKRs:
- Manager's concrete next steps → OKR objectives
- Growth areas → key results
- Strengths → leverage points (what to double down on)
- Challenge: "Does this actually move the needle for your career/role?"

Format: 2-3 OKRs with 3 KRs each. Present for approval before saving.

**For hybrid:** Combine personal direction from GROW with work context from the performance review. OKRs should bridge both — e.g., a personal goal of "become a better communicator" maps to a work KR of "lead 1 client demo independently."

Ask: "What projects are you working on?" Collect name + path pairs for the project registry.

**After OKR approval, distribute to agents:**
- Each agent gets OKRs relevant to their domain
- Maya: meta-tracking across all OKRs (flag when no weekly progress)
- Viktor: engineering/building KRs
- Luna: communication/content KRs
- Marco: strategy layer ownership
- Sage: professional growth enablement (work) or personal development (personal)
- Kai: relationship/event KRs

Save:
- OKRs to each agent's `## Current OKRs` section
- Project registry to `CLAUDE.md`
- Update `now.md` with OKRs

**Transition:** "Strategy is set. One more step — Maya will set up your daily rhythm."

If stopping: save progress. If continuing: proceed to Step 7.

### Step 7: Daily System (as Maya)

Switch to Maya's persona. Explain the daily rhythm:
- **Morning:** `/daily-review` — inbox processing, calendar, daily plan
- **During the day:** dispatch tasks to agents as needed
- **Evening:** `/scrum` — what each agent accomplished
- **Saturday:** `/weekly-review` — OKR check, retro, next week planning

Install skills from `razbakov/skills`:

```bash
git clone https://github.com/razbakov/skills /tmp/razbakov-skills
for skill in daily-review weekly-review process-inbox scrum become-claude-master personal-coach product-coach org-coach; do
  cp -r /tmp/razbakov-skills/skills/$skill ~/.claude/skills/$skill
done
```

Ask about optional integrations:
- "Do you use Notion for task management?" (optional)
- "Want Telegram bots to message agents from your phone?" (optional — if yes, generate scripts from `references/scripts/`)

Ask: "Want to do a quick Claude Code training session? It takes 15 minutes and will help you get the most out of your team."
If yes → invoke `/become-claude-master`

Commit any remaining changes: `git add -A && git commit -m "Complete Ikigai Team setup"`

**Finish:** "Your team is ready. Tomorrow morning, open this folder in Claude Desktop and say 'Good morning Maya.' She will run your first daily review. Welcome to your Ikigai Team."

### Step 8: Projects & Organizations (ongoing)

After the team is running, the user will want to set up their projects. There are two paths depending on scale:

**For individual projects** — invoke `/product-coach` (from `razbakov/skills/product-coach`):
- New project: Discovery → mission/vision → JTBD → user journey → validate → build
- Existing project: Workspace Review → identify gaps → fill them
- Creates README.md, strategy, story map, backlog in the project repo
- Dispatches specialized agents (product-lead, designer, engineer) for detailed work

**For organizations** (projects running on autopilot by multiple agents) — invoke `/org-coach` (from `razbakov/skills/org-coach`):
- Creates S3 governance: primary driver → domains → roles → agent definitions → policies
- Each org gets its own `.claude/agents/` with a Coordinator agent
- Creates a routing agent in the user's ikigai org that `cd`s to the org and runs the coordinator
- Examples: a SaaS product with its own dev + marketing agents, a community org with event + content agents

**Migrating an existing project:**
1. Open the project directory in Claude Desktop
2. Say "Help me set up this project" → `/product-coach` reviews what exists and fills gaps
3. If the project needs its own agent team: "Turn this into an organization" → `/org-coach` creates the S3 structure

The user can set up projects and orgs at any pace. Each one is independent — the ikigai org just tracks them all in its project registry.

## Saving Progress

When the user needs to stop between steps:

Write `.claude/agent-memory/setup/progress.md`:

```markdown
---
last_session: YYYY-MM-DD
resume_at: step_N
context_mode: work | personal | hybrid
owner_name: Their Name
org_path: ~/Orgs/ikigai
---

## Completed
- Step 3: Workspace generated
- Step 4-work: Performance review processed, NDA rules added

## Next
- Step 6: Define OKRs from review feedback

## Notes
Any context needed to resume smoothly.
```

Tell the user: "We saved your progress. When you are ready, open this folder in Claude Desktop and say 'Let us continue setting up my Ikigai Team.'"

## Corporate Document Handling

When users share employment contracts, performance reviews, or other corporate documents:

### Performance Reviews (high value)
- Extract: scores, strengths, growth areas, manager feedback, concrete next steps
- Save summary to `assessments/YYYY-HN-performance-review.md`
- Save strengths + growth areas to `profile.md`
- Use manager's concrete steps as OKR seeds
- Do NOT save raw review text verbatim — summarize

### Employment Contracts (medium value)
- Extract ONLY: role title, responsibilities list, tech stack, department, working arrangements (hours, home office)
- Save to `profile.md` under Scope of Duties
- NEVER save: salary, bonus amounts, addresses, legal clauses, signatures, termination terms
- Explain to the user what you are keeping and what you are discarding

### NDAs (rules only)
- Do NOT save the NDA text itself
- Extract the obligations and add confidentiality rules to CLAUDE.md
- Default rules: no client names, no project codenames, no internal financials, generic references only

## Bundled References

When generating files, read these references for the exact structures:
- `references/agent-structures.md` — agent definitions, CLAUDE.md format, directory scaffold, work/personal templates
- `references/scripts/telegram-bots.py` — multi-agent Telegram bot runner (tmux, self-healing, watchdog). Adapt AGENTS dict and paths.
- `references/scripts/telegram-send.py` — send messages via agent bots. Adapt AGENT_TOKEN_MAP.
- `references/scripts/telegram-bots.sh` — start/stop shell wrapper.
- `references/scripts/telegram-envoy-bot.py` — Envoy bot: group chat → agent + outbound conversations. Adapt ORG_PATHS and PERSONAS.
- `references/scripts/telegram-send-envoy.py` — send via Envoy bot.

Lines marked `# CUSTOMIZE` need to be adapted to the user's agent names and org path.

## Skills Reference

All skills come from https://github.com/razbakov/skills. Key skills used during setup:

| Step | Skill | Purpose |
|------|-------|---------|
| 4-5 | `/personal-coach` | L10L assessment, GROW coaching, journaling (personal/hybrid only) |
| 4-work | — | Work context discovery from performance reviews and contracts |
| 6 | `/product-coach` | Mission → hypothesis → JTBD → OKRs (personal context) |
| 6 | — | OKRs from performance review feedback (work context) |
| 7 | `/daily-review` | Morning inbox + calendar + plan |
| 7 | `/weekly-review` | Saturday review + next week planning |
| 7 | `/process-inbox` | GTD inbox processing |
| 7 | `/scrum` | Agent status report |
| 7 | `/become-claude-master` | Claude Code onboarding + skill assessment |
| 8 | `/product-coach` | Set up individual projects (discovery → validate → build) |
| 8 | `/org-coach` | Create organizations with S3 governance + agent teams |

Install skills by cloning and copying:
```bash
git clone https://github.com/razbakov/skills /tmp/razbakov-skills
cp -r /tmp/razbakov-skills/skills/<name> ~/.claude/skills/<name>
```

## Important Notes

- **You are one agent playing roles.** Do not try to spawn sub-agents or use `claude --agent`. Adopt each persona's communication style when it is their turn.
- **Delegate to skills.** Do not reimplement what skills already do. Invoke `/personal-coach`, `/product-coach` — they handle the methodology.
- **Context matters.** For personal setups, the coaching (Steps 4-5) IS the setup — do not rush it. The user's L10L scores and mission drive everything. For work setups, the performance review and manager feedback drive everything. Match the flow to the user's context.
- **Handle corporate documents carefully.** When users share contracts or reviews, extract only role-relevant information. Never save salary, addresses, or legal terms. Always explain what you are keeping and what you are discarding.
- **Respect the user's time.** Always ask before continuing to the next step. Some users will do everything in one session. Others will spread it across a week. Both are fine.
- **Save everything.** Every assessment, every coaching session, every OKR gets saved to a file. The user's reflections are theirs to keep (respecting NDA boundaries for work contexts).
- **No jargon dumps.** When introducing a methodology (L10L, GROW, OKRs, GTD), explain it naturally in conversation. The skills handle the process — you provide the context and transitions.
- **Steps are optional.** The user can skip any step or come back to it later. Mention what the next step is and why it helps, but if they say "not now" or "skip", respect that immediately. Save progress and move on. Never repeat a suggestion they already declined.
