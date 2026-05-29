---
name: project-updates
description: Use when writing or reviewing a project status update to make it scannable, honest, and verifiable.
---

# Project Updates

## Structure

- **Scoped title** — one line saying what actually changed this period.
  - ❌ `Weekly Update #7`
  - ✅ `Payments integration — staging tests passed, 1-week slip`
- **Set health honestly** — On track / At risk / Off track. Flag "at risk" the week a dependency blocks you, not after the slip lands.
- **Three fixed sections** — `Progress` → `Blockers` → `Next`. Reader scans in seconds.
  - ❌ One wall of prose where the blocker is buried in sentence five.
  - ✅ `Progress:` checkout flow live. `Blockers:` waiting on vendor API key. `Next:` load test Monday.

## Content

- **Numbers, not adjectives.**
  - ❌ "great progress", "almost there"
  - ✅ "68 accounts migrated", "12 of 15 tickets done", "error rate 4% → 0.3%"
- **Show the delta**, not just the absolute.
  - ❌ "milestones advanced this week"
  - ✅ "Release milestone: 80% → 100%"
- **Link everything** — every claim points to proof: ticket IDs, PRs, dashboards, docs, demo recordings, decision threads.
  - ❌ "fixed the sync bug" with no ticket and no way to verify.
- **Name people and give credit.**
  - ❌ "thanks to the team"
  - ✅ "Thanks @sam and @priya for unblocking the deploy."
- **Make cross-team dependencies explicit** — who you're waiting on, who's affected, what they must do.
  - ❌ "waiting on other teams"
  - ✅ "Mobile team must bump the API client to v3 before launch."

## Honesty

- **Bad news first** — with cause and a new ETA.
  - ❌ Slip mentioned in the last line, no cause, no new date.
  - ✅ "Two bugs found in testing → 1-week slip, ETA next Friday, overall deadline still holds."
- **Turn findings into action items** with an owner and a ticket.
  - ❌ "onboarding felt confusing."
  - ✅ "Users hit a dead end after sign-up → TICKET-123, owner @sam."
- **Name the shortcuts and debt you took on.**
  - ❌ A silent gap that support discovers two months later.
  - ✅ "No automated cancellation flow yet — manual for now, follow-up project linked."

## Closing Update

When the project ends:

- **Final stats.**
  - ❌ "Done! 🎉" and nothing else.
  - ✅ "Jan–Oct, ~200 working days, 173 tickets, 7 teams involved."
- **Wins on the sideline** — reusable learnings for the next project.
  - ✅ "Moving account creation upstream simplified the whole flow — reuse this pattern elsewhere."
- **Known shortcuts + follow-up work, linked** — each deferred item links to its own follow-up.

## Core

Honest health + hard numbers + every claim links to proof + every problem has an owner.
