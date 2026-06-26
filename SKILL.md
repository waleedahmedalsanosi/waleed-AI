---
name: waleed-ai
description: "Waleed AI — 13-skill PM automation suite for Dreamy partnership evaluation cycles"
triggers:
  - intake
  - evaluate
  - bop
  - outreach
  - prd
  - sow
  - proposal
  - ceo review
  - client review
  - followup
  - brief
  - handoff
  - meeting
  - waleed
allowed-tools:
  - Read
  - Write
  - Bash
---

## Waleed AI — Skill Router

PM automation suite for Dreamy (dreamybuilders.com) partnership evaluation cycles.
13 sequential skills: `/intake` → `/evaluate` → `/bop` → `/outreach` → `/prd` → `/sow` →
`/proposal` → `/ceo-review` → `/client-review` → `/followup` → `/brief` → `/handoff`

Use `/meeting` at any point after `/intake` to log additional founder meetings.

**Install path:** `~/.claude/skills/waleed-ai/`

## Routing

When the user invokes one of the skills below, read the corresponding SKILL.md
file and execute it as instructions.

| Skill | File |
|-------|------|
| `/intake` | `~/.claude/skills/waleed-ai/skills/intake/SKILL.md` |
| `/evaluate` | `~/.claude/skills/waleed-ai/skills/evaluate/SKILL.md` |
| `/bop` | `~/.claude/skills/waleed-ai/skills/bop/SKILL.md` |
| `/outreach` | `~/.claude/skills/waleed-ai/skills/outreach/SKILL.md` |
| `/prd` | `~/.claude/skills/waleed-ai/skills/prd/SKILL.md` |
| `/sow` | `~/.claude/skills/waleed-ai/skills/sow/SKILL.md` |
| `/proposal` | `~/.claude/skills/waleed-ai/skills/proposal/SKILL.md` |
| `/ceo-review` | `~/.claude/skills/waleed-ai/skills/ceo-review/SKILL.md` |
| `/client-review` | `~/.claude/skills/waleed-ai/skills/client-review/SKILL.md` |
| `/followup` | `~/.claude/skills/waleed-ai/skills/followup/SKILL.md` |
| `/brief` | `~/.claude/skills/waleed-ai/skills/brief/SKILL.md` |
| `/handoff` | `~/.claude/skills/waleed-ai/skills/handoff/SKILL.md` |
| `/meeting` | `~/.claude/skills/waleed-ai/skills/meeting/SKILL.md` |

## Session Architecture

```
~/.waleed-ai/
  .current-session          ← active session path (written by /intake)
  sessions/
    YYYY-MM-DD-founder/
      01-intake.md          ← /intake
      02-evaluate.md        ← /evaluate  (re-run after /meeting to refresh score)
      03-bop.md             ← /bop
      04-outreach.md        ← /outreach  (first WhatsApp — sent before full doc suite)
      05-prd.md             ← /prd
      06-sow.md             ← /sow
      07-proposal.md        ← /proposal
      08-ceo-review.md      ← /ceo-review
      09-client-review.md   ← /client-review
      10-followup.md        ← /followup
      11-brief.md           ← /brief
      12-handoff.md         ← /handoff  (only after Outcome: Proceed)
      meetings/
        YYYY-MM-DD.md       ← /meeting  (one file per additional meeting)
```

## Multi-Meeting Workflow

When you meet a founder more than once:

1. After the first meeting: run the normal sequence through `/outreach`.
2. Before the second meeting: run `/brief` to prep.
3. After the second meeting: run `/meeting` to log new information.
4. Re-run `/evaluate` — it will detect the `meetings/` folder and incorporate new data,
   producing an updated `02-evaluate.md` (clearly marked as a revision).
5. If the evaluation score or conditions changed, re-run `/bop` and `/ceo-review` to
   update Samir's brief before sending `/followup`.

## Evaluation Framework

`~/.claude/skills/waleed-ai/lib/evaluation-framework.md`
100 points across 6 categories: Founder/Team (25), Market (20), Product (20),
Business Model (15), Traction (10), Dreamy Fit (10).
