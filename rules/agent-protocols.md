## Agent Protocols

How managers package work for the Commander's review and how proposals reach decisions.
This is the conversational layer on top of the existing rules in `agent-team.md`,
`agent-operations.md`, and `telegram.md`. The goal is for Telegram conversations with
agents to feel like working with humans on a sociocracy circle: short, clear,
review-ready, and decisive.

### Sociocracy basis

The team operates as a sociocracy circle. The Commander is the linked-out role; the six
managers form the top circle. Each manager runs their own circle (org coordinators,
project coordinators, sub-agents). Two principles apply at the conversational layer:

- **Domain authority** — each manager has a clear domain. Inside it, they act without
  asking. Outside it, they propose.
- **Consent, not consensus** — managers ship if there are no objections within the time-box.
  Default-consent beats default-blocked. Tensions surface up; routine doesn't.

Each manager's specific domain-authority lines (what they do without asking, what they
propose, what always escalates) live in their private agent file at
`.claude/agents/<name>.md` under "Domain Authority".

### Review-ready message format

When a manager sends the Commander a Telegram message asking for approval or surfacing a
delivered outcome, the message must follow this shape:

```
<Agent>: <one-line title>
Why: <which KR/driver this serves — short>
What: <1-2 lines, what changed or what's done>
Media: <PR link · screenshot · video · mockup · doc — what lets the Commander review without leaving Telegram>
Asking: <decision in one sentence>. Default: <action> by <time> unless you object.
```

Notes on the shape:
- **Why** anchors the work to a known KR or driver — no orphan asks. If a manager can't
  cite a KR, the work probably shouldn't be in front of the Commander.
- **What** is one or two lines. Anything longer belongs in the linked artifact.
- **Media** is non-optional when relevant. Code → PR. UI → screenshot or video. Content →
  the draft. Strategy memo → the doc. The Commander reviews from Telegram, not by digging.
- **Asking** ends with default-consent: silence by the time-box = ship. The Commander
  can `ok` to ship now, push back to revise, or `hold` to extend the box.

Half-baked updates ("started on X", "still working", "FYI") do not get sent. If it's not
a decision asked or a delivered outcome, it stays inside the manager's circle.

### Default-consent time-boxes

| Stakes | Default-consent window | Examples |
|---|---|---|
| Routine | 4h | merge a clean PR, publish a draft, dispatch a sub-agent, schedule a known recurring meeting |
| Material | 24h | ship a feature to prod, send a partner email, change a price, kick off a campaign |
| Strategic | no auto-consent — escalate | sign a contract, deprecate a project, change OKRs, anything irreversible |

Strategic items never auto-ship. The manager may propose and recommend, but the Commander
must explicitly approve.

### Silence is not consent for consequential or listed items

Default-consent (silence → ship) is a convenience for **single, reversible, low-stakes**
asks. It is **void** in two cases, because in both the Commander's silence cannot be read as
agreement:

1. **Consequential items** — anything involving money, an outbound message, a published or
   irreversible change, a standing rule, or account/permission changes. These require an
   **explicit approval**. Silence means *not done* — the item parks and waits, it never executes.
2. **Listed items** — whenever two or more decisions share one message. A single reply
   channel cannot disambiguate which item it answers, so silence (or even one "ok") must not
   be spread across the list. Each item needs its **own unambiguous answer**.

The reliable mechanism for both is a **per-item explicit control** — e.g. Telegram inline
buttons (`✓ / ✗ / ⏸`) where each tap carries that item's id, the message edits in place to
confirm, and an untapped item stays **Pending** and re-surfaces. **The system acts only on an
explicit tap; absence of a tap does nothing.** This inverts default-consent precisely where
default-consent is dangerous.

FYIs (no decision requested) don't need this — they're informational. Only decisions do.

### Working with the protocol

- The Commander replies in natural language. "ok" / "ship it" / "yes" → approve now.
  "wait" / "hold" → pause the time-box. "no" / "change X" / "why this not Y?" → revise
  and resend. Silence past the time-box → consent **only for a single, reversible,
  low-stakes item** — never for a consequential item or one presented in a list (see
  "Silence is not consent for consequential or listed items" above).
- Managers should resend the same review-ready message (with revisions noted) rather
  than starting a new thread when a request bounces.
- If a decision needs cross-manager input (e.g., a Marco proposal that touches Viktor's
  domain), the proposing manager handles the cross-talk inside the team and the
  Commander sees one consolidated review-ready message, not two.
