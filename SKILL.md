---
name: waleed-ai
description: "Waleed AI — 12-skill PM automation suite for Dreamy partnership evaluation cycles"
triggers:
  - intake
  - evaluate
  - bop
  - prd
  - sow
  - proposal
  - ceo review
  - client review
  - outreach
  - followup
  - brief
  - handoff
  - waleed
allowed-tools:
  - Read
  - Write
  - Bash
---

## Waleed AI — Skill Router

PM automation suite for Dreamy (dreamybuilders.com) partnership evaluation cycles.
12 sequential skills: `/intake` → `/evaluate` → `/bop` → `/prd` → `/sow` →
`/proposal` → `/ceo-review` → `/client-review` → `/outreach` → `/followup` →
`/brief` → `/handoff`

**Install path:** `~/.claude/skills/waleed-ai/`

## Routing

When the user invokes one of the 12 skills below, read the corresponding SKILL.md
file and execute it as instructions.

| Skill | File |
|-------|------|
| `/intake` | `~/.claude/skills/waleed-ai/skills/intake/SKILL.md` |
| `/evaluate` | `~/.claude/skills/waleed-ai/skills/evaluate/SKILL.md` |
| `/bop` | `~/.claude/skills/waleed-ai/skills/bop/SKILL.md` |
| `/prd` | `~/.claude/skills/waleed-ai/skills/prd/SKILL.md` |
| `/sow` | `~/.claude/skills/waleed-ai/skills/sow/SKILL.md` |
| `/proposal` | `~/.claude/skills/waleed-ai/skills/proposal/SKILL.md` |
| `/ceo-review` | `~/.claude/skills/waleed-ai/skills/ceo-review/SKILL.md` |
| `/client-review` | `~/.claude/skills/waleed-ai/skills/client-review/SKILL.md` |
| `/outreach` | `~/.claude/skills/waleed-ai/skills/outreach/SKILL.md` |
| `/followup` | `~/.claude/skills/waleed-ai/skills/followup/SKILL.md` |
| `/brief` | `~/.claude/skills/waleed-ai/skills/brief/SKILL.md` |
| `/handoff` | `~/.claude/skills/waleed-ai/skills/handoff/SKILL.md` |

## Session Architecture

```
~/.waleed-ai/
  .current-session          ← active session path (written by /intake)
  sessions/
    YYYY-MM-DD-founder/
      01-intake.md
      02-evaluate.md
      03-bop.md
      04-prd.md
      05-sow.md
      06-proposal.md
      07-ceo-review.md
      08-client-review.md
      09-outreach.md
      10-followup.md
      11-brief.md
      12-handoff.md
```

## Evaluation Framework

`~/.claude/skills/waleed-ai/lib/evaluation-framework.md`
100 points across 6 categories: Founder/Team (25), Market (20), Product (20),
Business Model (15), Traction (10), Dreamy Fit (10).
