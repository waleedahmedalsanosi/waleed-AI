---
name: waleed-brief
triggers:
  - brief
  - meeting prep
  - prep for call
  - prep note
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run before a follow-up call with the founder. Reads the original intake and the
follow-up correspondence to produce a concise internal meeting prep note at `12-brief.md`.

Audience: Waleed only. Format: bullet-point brief with context recap, open questions,
and suggested talking points. Should take 3 minutes to read before jumping on a call.

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
for F in "01-intake.md" "11-followup.md"; do
  if [ ! -f "$SESSION/$F" ]; then
    echo "ERROR: $F not found — run the preceding skill first"; exit 1
  fi
done
# Extract outcome from followup for context
OUTCOME=$(grep "^Outcome:" "$SESSION/11-followup.md" | tail -1 | sed 's/Outcome: //')
echo "SESSION: $SESSION"
echo "OUTCOME: $OUTCOME"
```

---

## Instructions

You are running the `/brief` skill for Waleed Al-Sanosi, PM at Dreamy.

This is for Waleed's eyes only. Write it like a colleague briefed you right before
a call. No formal register. Direct. Useful. What you'd actually want to know.

### Step 1: Read inputs

1. `01-intake.md` — original Founder Brief (the context from the first meeting)
2. `11-followup.md` — follow-up correspondence (what was sent, what the current status is)

### Step 2: Write 12-brief.md

Write to `{SESSION}/12-brief.md`:

```markdown
# Meeting Prep Brief

**Founder:** {Founder Name}
**Company:** {Company Name}
**Call purpose:** {what this meeting is about — follow-up on Proceed / Conditional / Pass}
**Date:** {today's date}
**Status:** {Proceed / Conditional / Pass — from followup}

---

## Context in 60 Seconds

- **Who:** {Founder name}, {background in one phrase — "ex-Careem operations, 5 years in logistics"}
- **What:** {product in one sentence — what it does, for whom}
- **Market:** {the opportunity in one phrase — "B2B SaaS for Saudi SME payroll, $2B TAM"}
- **Score:** {X}/100 — {Recommendation}
- **Where we left off:** {one sentence on what was sent to the founder and when}

---

## What They Care About

Based on the intake transcript, {Founder Name}'s top priorities are:

1. {The thing they cared most about — technical timeline, equity terms, product direction}
2. {Second priority}
3. {Third priority — or omit if only two are clear}

These are the levers. Come prepared to address them directly.

---

## Open Questions

Things that were unclear or unresolved from the intake:

1. {Question — who needs to answer it and by when}
2. {Question}
3. {Question — or note if none remain}

{If Conditional:}

### Conditions to Resolve on This Call

From Samir's review, these conditions must be met before Dreamy commits:

1. {Condition from ceo-review — has it been met? What to check on the call?}
2. {Condition 2 if applicable}

---

## Suggested Talking Points

Not a script — these are the 3–4 things worth raising:

1. **{Topic}:** {Why bring it up. What you want to learn or confirm. 1–2 sentences.}
2. **{Topic}:** {Same format.}
3. **{Topic}:** {Same format.}
4. **{Topic}:** {Optional — only if genuinely relevant.}

---

## What NOT to Do

{1–3 specific pitfalls for this particular founder/situation.
Examples:
- "Don't commit to a timeline before the technical scoping is done."
- "Don't raise the equity percentage until they ask — Samir hasn't confirmed it yet."
- "This founder pushed back on pricing in the intake — don't lead with monetization."}

---

## If They Ask About...

{Anticipate 2–3 questions they're likely to ask and provide concise answers:}

**"When can you start?"**
{Prepared answer based on Dreamy's current capacity}

**"What equity are you looking for?"**
{Prepared answer — either the agreed figure or "I'll confirm with Samir and follow up"}

**"{Specific question likely based on the intake}"**
{Prepared answer}

---

## Red Flags to Watch

If you hear any of the following in this call, note it and flag for reassessment:

- {Red flag 1 — based on risks from evaluate/bop}
- {Red flag 2}
- {Red flag 3}

---

*Brief generated {today's date}. Refresh if the call is >48 hours away.*
```

---

### Step 3: Confirm

After writing the file, print:

```
/brief complete.

File: {session path}/12-brief.md
```
