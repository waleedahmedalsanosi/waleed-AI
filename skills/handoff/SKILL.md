---
name: waleed-handoff
triggers:
  - handoff
  - technical handoff
  - dev brief
  - engineering brief
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/prd` and `/sow` are finalized and the partnership is confirmed.
Produces a technical handoff document for the Dreamy engineering team at `12-handoff.md`.

Audience: Dreamy's developers. Format: English only (internal). Structured with epics,
features, and acceptance criteria derived from the approved PRD and SOW.

---

## Bash Preamble

```bash
SESSION_FILE="$HOME/.waleed-ai/.current-session"
if [ ! -f "$SESSION_FILE" ]; then
  echo "ERROR: no active session — run /intake first"; exit 1
fi
SESSION=$(cat "$SESSION_FILE")
if [ ! -d "$SESSION" ]; then
  echo "ERROR: session folder missing: $SESSION — re-run /intake"; exit 1
fi
for F in "04-prd.md" "05-sow.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/handoff` skill for Waleed Al-Sanosi, PM at Dreamy.

This document goes to the engineers who will build the product. They haven't read the
intake transcript or the BOP. They need:
- What to build (features and acceptance criteria)
- What platform and tech decisions have been made
- What the constraints are
- Where the open questions are that affect their work

Write for a senior developer who's context-free on this project.

### Step 1: Read inputs

Read from the current session:
1. `04-prd.md` — Product Requirements Document (features, user stories, technical requirements)
2. `05-sow.md` — Scope of Work (deliverables, timeline, milestones)

### Step 2: Write 12-handoff.md

Write to `{SESSION}/12-handoff.md`:

```markdown
# Technical Handoff

**Product:** {Product Name}
**Company:** {Company Name}
**Prepared by:** Waleed Al-Sanosi, PM — Dreamy
**Date:** {today's date}
**Status:** Partnership confirmed — development starting {date from SOW}

---

## Overview

{2–3 sentences for a dev who knows nothing about this project.
What is being built, for whom, and what's the expected v1 scope.}

---

## Platform & Stack

**Target platform:** {Web / iOS / Android / iOS + Android / All three}
**Frontend:** {React / Next.js / React Native / Swift / Kotlin / TBD}
**Backend:** {Node.js / Python / Go / TBD — and why if specified in PRD}
**Database:** {PostgreSQL / MySQL / Firebase / TBD}
**Auth:** {Supabase / Firebase Auth / Custom / TBD}
**Hosting / Infra:** {Vercel / AWS / GCP / TBD}
**Key third-party integrations:** {list APIs, payment providers, maps, SMS, etc.}

*Note: Stack decisions marked TBD are to be finalized in the technical discovery call
with the founder. Do not assume defaults.*

---

## Language & Localization

- Primary language: **Arabic (RTL)** + English bilingual (required for all user-facing text)
- RTL support required: yes
- Arabic character encoding: UTF-8
- Date format: Arabic (DD/MM/YYYY or Hijri as appropriate to the use case)

---

## Epics & Features

Organized by build sequence. Each epic = one deployable unit.

{For each feature from the PRD In Scope list, create an epic entry:}

### Epic {N}: {Feature Name}

**Priority:** P{0–2} (P0 = MVP blocker, P1 = important, P2 = nice-to-have-at-launch)
**Estimate:** {rough: 1 week / 2 weeks / 1 month for 1 developer}

**User story:**
> {Verbatim or adapted from PRD user story}

**Acceptance criteria:**
- [ ] {Specific, testable criterion — what "done" looks like}
- [ ] {Criterion}
- [ ] {Criterion}

**Technical notes:**
{Any constraints, dependencies, or implementation considerations specific to this feature.
If none, omit this field.}

---

{Repeat for each feature}

---

## Out of Scope (v1)

Do not build the following in v1. These are explicitly deferred:

{Numbered list from PRD Out of Scope — keep the technical framing}

If the founder requests any of these mid-development, escalate to Waleed. Do not
scope-creep without a revised SOW.

---

## Milestones

| Milestone | Target | What "done" means |
|-----------|--------|-------------------|
| Architecture sign-off | {date} | Stack decided, schema drafted, API contracts defined |
| Design complete | {date} | All screens mocked in Figma, founder approved |
{Copy remaining milestones from SOW with a concrete "done" definition for each}

---

## Open Technical Questions

These are blockers or near-blockers that need answers before development starts.
Assign owners and get answers in the first week.

| # | Question | Owner | Needed by |
|---|----------|-------|-----------|
| 1 | {Technical question from PRD open questions} | {Dreamy / Founder} | {date} |
| 2 | {Question} | {owner} | {date} |

---

## Constraints & Non-negotiables

- {Constraint 1 — e.g., "Must support KSA data residency (no EU-only hosting)"}
- {Constraint 2 — e.g., "Arabic RTL is required from day one — not an afterthought"}
- {Constraint 3 — e.g., "Must integrate with {specific Saudi payment gateway}"}

---

## Working with the Founder

- **Waleed's role:** PM and primary liaison. All founder communication goes through Waleed.
- **Weekly sync:** {day and time} with Waleed present
- **Direct founder contact:** Allowed for technical clarifications. Loop Waleed in on anything
  that affects scope, timeline, or the product roadmap.
- **Design approvals:** Founder signs off on each epic's design before build starts.

---

## Reference Documents

| Document | Location | What it contains |
|----------|----------|-----------------|
| Founder Brief | `{session}/01-intake.md` | Raw context from the founder meeting |
| Evaluation Report | `{session}/02-evaluate.md` | Dreamy's assessment of the opportunity |
| PRD | `{session}/04-prd.md` | Full product requirements with user stories |
| SOW | `{session}/05-sow.md` | Commercial scope and equity terms |

---

*v1 handoff — {today's date}. Update this document after each major scope change.*
```

---

### Step 3: Confirm

After writing the file, print:

```
/handoff complete.

File: {session path}/12-handoff.md

All 12 session files are now complete:
  01-intake.md      → Founder Brief
  02-evaluate.md    → Evaluation Report ({score}/100, {Recommendation})
  03-bop.md         → Business Opportunity Profile
  04-prd.md         → Product Requirements Document
  05-sow.md         → Scope of Work
  06-proposal.md    → Partnership Proposal
  07-ceo-review.md  → CEO Review Brief (Samir)
  08-client-review.md → Client Review Package
  09-outreach.md    → First Outreach (WhatsApp)
  10-followup.md    → Follow-up Correspondence
  11-brief.md       → Meeting Prep Brief
  12-handoff.md     → Technical Handoff

Cycle complete.
```
