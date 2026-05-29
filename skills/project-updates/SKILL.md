---
name: project-updates
description: Use when writing or reviewing a project status update to make it scannable, honest, and verifiable.
---

# Project Updates

A good update reads in 30 seconds, tells the truth about where things stand, and lets anyone drill into detail without asking. Apply these when writing or reviewing one.

## Structure

- **Scoped title** — one line saying what actually changed this period.

Avoid: `Weekly Update #7`

Prefer: `Payments integration — staging tests passed, 1-week slip`

- **Set health honestly** — On track / At risk / Off track. Flag "at risk" the week a dependency blocks you, not after the slip lands.

- **Three fixed sections** — `Progress` → `Blockers` → `Next`. Reader scans in seconds.

Avoid: one wall of prose where the blocker is buried in sentence five.

Prefer:

```
Progress: checkout flow live.
Blockers: waiting on vendor API key.
Next: load test Monday.
```

## Content

- **Numbers, not adjectives.**

Avoid: "great progress", "almost there"

Prefer: "68 accounts migrated", "12 of 15 tickets done", "error rate 4% → 0.3%"

- **Show the delta**, not just the absolute.

Avoid: "milestones advanced this week"

Prefer: "Release milestone: 80% → 100%"

- **Link everything** — every claim points to proof: ticket IDs, PRs, dashboards, docs, demo recordings, decision threads.

Avoid: "fixed the sync bug" with no ticket and no way to verify.

- **Name people and give credit.**

Avoid: "thanks to the team"

Prefer: "Thanks @sam and @priya for unblocking the deploy."

- **Make cross-team dependencies explicit** — who you're waiting on, who's affected, what they must do.

Avoid: "waiting on other teams"

Prefer: "Mobile team must bump the API client to v3 before launch."

## Honesty

- **Bad news first** — with cause and a new ETA.

Avoid: slip mentioned in the last line, no cause, no new date.

Prefer: "Two bugs found in testing → 1-week slip, ETA next Friday, overall deadline still holds."

- **Turn findings into action items** with an owner and a ticket.

Avoid: "onboarding felt confusing."

Prefer: "Users hit a dead end after sign-up → TICKET-123, owner @sam."

- **Name the shortcuts and debt you took on.**

Avoid: a silent gap that support discovers two months later.

Prefer: "No automated cancellation flow yet — manual for now, follow-up project linked."

## Closing Update

When the project ends:

- **Final stats.**

Avoid: "Done! 🎉" and nothing else.

Prefer: "Jan–Oct, ~200 working days, 173 tickets, 7 teams involved."

- **Wins on the sideline** — reusable learnings for the next project.

Prefer: "Moving account creation upstream simplified the whole flow — reuse this pattern elsewhere."

- **Known shortcuts + follow-up work, linked** — each deferred item links to its own follow-up.

## Cadence

- Regular rhythm — short when quiet, fuller at milestones. A tight "wrapping up, two items left" note in calm weeks; a full writeup at launch. Not silence for three weeks, then a novel.

## Core

Honest health + hard numbers + every claim links to proof + every problem has an owner.
