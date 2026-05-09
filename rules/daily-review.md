## Daily Review

The daily review is the heartbeat of the operating system. It's how nothing falls through cracks.

### File locations

- **Daily reviews** → `ops/sessions/YYYY-MM-DD-daily-review.md`
- **Weekly reviews** → `ops/reviews/YYYY-MM-DD-weekly-review.md`
- **Telegram inbox dumps** → `ops/inbox/YYYY-MM-DD-HH-MM.md`

The daily review only **links to** inbox files — it doesn't inline them.

### What the daily review must include

- Check the last weekly review for missed priorities.
- Process Chrome bookmarks as a GTD inbox.
- Check the dispatch surface (Butler / equivalent) for agent messages during inbox processing.
- A meal plan for lunch and dinner. Use the `/meal-suggestion` skill.
- Reasoning for the day's priorities at the end (one paragraph: why these, why now, what gets dropped).

### Daily Check-in calendar event

- Must include deep links: dispatch surface URL, Telegram chat link, session file path.
- The `/daily-standup` skill must schedule the standup agenda via the dispatch surface before sending the morning DM.

### Schedule

- Data gathering at **6am** (background — agent collects context).
- Morning DM from Maya at exactly **9:00** — this is the **daily-plan consent gate** (see below), not a delivered plan.
- End-of-day closure ping at **21:00** (or per-Commander config) — see "End-of-day closure" below.

### Daily-plan consent gate (9:00 morning DM)

The 9:00 morning DM is a **review-ready proposal**, not an auto-delivered plan. Maya proposes the day; the Commander consents.

- Maya assembles candidate calendar blocks and candidate tasks from the system queue (GitHub Issues, parked items from previous days, deadlines from contacts/projects).
- Maya filters to the daily cap (per-Commander config in private CLAUDE.md; default suggestion: 5 actionable items, calendar blocks + tasks combined). If the candidate count exceeds the cap, Maya picks by current OKR priority (Layer 1 / Layer 3) and surfaces the rest as "parked, can promote tomorrow."
- The DM uses review-ready format (per `agent-protocols.md`) with a **default-consent window** (per-Commander config; default suggestion: 30 min). Example shape:

  > Today I propose: 3 calendar blocks (X 10–11, Y 14–15, Z 17–18) + 2 tasks (A, B). Anything you don't consent to, I park back in the system queue. Default-consent: 30 min. Reply with edits or `ok`.

- **Until consent (or silence past the window), nothing lands on the Commander's Calendar or Tasks.** Auto-syncing without consent is the failure mode this rule exists to fix.
- Items the Commander rejects go back to GitHub Issues (system queue) or to the per-agent parked-tasks file (e.g. `ops/agents/maya/parked-tasks.md`). They don't disappear; they just don't enter the personal surface today.

### End-of-day closure (21:00 closure ping)

Calendar events and tasks are claims about the future. Without an explicit closure step, they rot — the system can't tell what happened.

- At 21:00, Maya pings the Commander with the day's plan and asks for a yes/no per item:

  > Today's plan: ✓ X happened, ✓ Y happened, ✗ Z skipped. Tasks: ✓ A done, ✗ B undone. Confirm or correct?

- The Commander confirms or corrects. Maya logs the result to that day's `ops/sessions/YYYY-MM-DD-daily-review.md` so closure history is auditable.
- **Skipped items do NOT auto-roll-forward.** They go back to the system queue (GitHub Issues or parked-tasks file) and re-compete for tomorrow's cap slots via the 9:00 consent gate. No silent backlog growth.
- This step is what closes the loop on any reliability KR ("zero missed follow-ups", "zero dropped commitments") — without closure tracking, those KRs are vibes, not measurements.

### File first, message second

When producing strategic summaries, save the file before sending the message. The message should link to the file, not contain it. This protects long-form thinking from getting buried in chat scrollback.

### Other recurring processes

- Inbox processing from the dispatch surface runs **hourly**.
- Sessions live in `ops/sessions/`. Three types:
  - **Browser history** (`YYYY-MM-DD-browser.md`) — Chrome History SQLite → time-block summary
  - **AI transcripts** (`YYYY-MM-DD-ai-sessions.md`) — Claude session JSONL + Conductor DB → project groups
  - **Other** (`YYYY-MM-DD-topic.md`) — check-ins, working sessions, focused topics
