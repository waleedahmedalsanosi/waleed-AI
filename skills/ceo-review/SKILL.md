---
name: waleed-ceo-review
triggers:
  - ceo review
  - samir review
  - executive summary
  - brief samir
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/bop` (and optionally `/evaluate`). Produces a concise executive brief
for Samir Al-Rashidi (Dreamy CEO) at `09-ceo-review.md`.

Samir needs to make one decision: Proceed / Conditional / Pass. The document must
give him everything he needs in under 3 minutes of reading. No filler.

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
for F in "03-evaluate.md" "04-bop.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
echo "SESSION: $SESSION"
```

---

## Instructions

You are running the `/ceo-review` skill for Waleed Al-Sanosi, PM at Dreamy.

The audience for this document is Samir Al-Rashidi, CEO of Dreamy. He is busy.
He has seen hundreds of founders. He wants to know: is this worth Dreamy's time
and equity over the next 2 years? If yes, under what conditions?

Write for Samir, not for the founder. No soft language. No padding. Direct.

### Step 1: Read inputs

Read from the current session:
1. `03-evaluate.md` — Evaluation Report (scores, strengths, risks, recommendation)
2. `04-bop.md` — Business Opportunity Profile (market, founder, Dreamy upside)

### Step 2: Write 09-ceo-review.md

Write to `{SESSION}/09-ceo-review.md`:

```markdown
# CEO Review Brief

**To:** Samir Al-Rashidi, CEO, Dreamy
**From:** Waleed Al-Sanosi, PM
**Date:** {today's date}
**Re:** {Company Name} — Partnership Evaluation

---

## The One-Paragraph Version

{This is the first thing Samir reads. 4–5 sentences that give him everything.
Structure: [Who is the founder + what's their credibility signal]. [What they're building
and for whom]. [Why now / why MENA]. [The score and what it means]. [Waleed's recommendation
in one direct sentence]. No hedging. Write this paragraph last.}

---

## The Founder

**Name:** {Founder Name}
**Credibility:** {The single most important thing about this founder — 1 sentence}
**Risk:** {The single most important concern about this founder — 1 sentence}
**Coachability:** {High / Medium / Low} — {evidence: one quote or observation}

---

## The Opportunity

**Market:** {What market, what size signal (or lack thereof), what timing driver}
**Problem:** {The specific pain being solved — one crisp sentence}
**Differentiation:** {What makes this defensible or different — one sentence, or "Not clear yet"}
**MENA angle:** {The Gulf-specific insight or advantage, or "Generic — no regional moat"}

---

## Score Summary

| Category | Max | Score |
|----------|-----|-------|
| Founder / Team | 25 | {X} |
| Market | 20 | {X} |
| Product | 20 | {X} |
| Business Model | 15 | {X} |
| Traction | 10 | {X} |
| Dreamy Fit | 10 | {X} |
| **TOTAL** | **100** | **{X}** |

---

## Top 3 Strengths

1. {Specific, evidence-based strength — not generic}
2. {Specific strength}
3. {Specific strength}

## Top 3 Risks

1. {Specific risk with impact on Dreamy's equity bet}
2. {Specific risk}
3. {Specific risk}

---

## What This Looks Like for Dreamy

**Build scope:** {High-level: "2-person team, 6-month build, mobile-first" etc.}
**Waleed's equity recommendation:** {State a specific percentage range, e.g. "30–35%". Base it on engagement weight: full technical co-founding with 100% technical lift = 25–40%; shared build = 15–25%. Do NOT leave this blank — Samir needs a number to react to, not a placeholder.}
**Strategic upside:** {Why this is a good portfolio bet for Dreamy, in 1–2 sentences}
**Comparable:** {If there's an analogy to something Dreamy has done or knows — optional}

---

## Waleed's Recommendation

**{Proceed / Conditional / Pass}**

{2–3 sentences. Name the deciding factor. If Conditional, name the 1–2 conditions.
If Pass, name the 1 reason. Be direct — Samir doesn't want diplomacy here.}

{If Conditional, add:}
**Conditions:**
1. {Specific condition that must be met before Dreamy commits}
2. {Second condition if applicable}

---

Outcome: {Proceed / Conditional / Pass}
```

**CRITICAL:** The file MUST end with the line `Outcome: {Proceed / Conditional / Pass}`.
This exact line is read by the `/followup` skill to determine the next correspondence.
Use exactly one of: `Outcome: Proceed`, `Outcome: Conditional`, or `Outcome: Pass`.

---

### Step 3: Confirm

After writing the file, print:

```
/ceo-review complete.

File: {session path}/09-ceo-review.md
Recommendation: {Proceed / Conditional / Pass}

⚠ Reminder: Fill in the equity percentage before sharing with Samir.

Next: run /client-review, /outreach, or /followup (after Samir reviews)
```
