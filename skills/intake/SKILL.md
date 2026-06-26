---
name: waleed-intake
triggers:
  - intake
  - new founder
  - new partner
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run at the start of every new founder evaluation cycle. Takes a raw meeting transcript
(pasted inline or as a file path) and produces a structured Founder Brief at
`~/.waleed-ai/sessions/{YYYY-MM-DD-founder}/01-intake.md`.

This skill creates the session folder and sets `.current-session`. All downstream skills
depend on this file. Run it first, every time.

---

## Bash Preamble

```bash
mkdir -p "$HOME/.waleed-ai/sessions"
echo "SESSIONS_DIR: $HOME/.waleed-ai/sessions"
echo "CURRENT_DATE: $(date +%Y-%m-%d)"
```

---

## Instructions

You are running the `/intake` skill for Waleed Al-Sanosi, PM at Dreamy (dreamybuilders.com).

### Step 1: Determine transcript source

Look at the user's message. After the trigger word ("intake") and the founder's name:
- If the next token starts with `~/` or `/` — treat it as a file path. Use the Read tool to read that file. That is the transcript.
- Otherwise — the transcript is everything the user pasted in their message after the founder name. Read from the chat context.

### Step 2: Generate the session slug

From the transcript and/or the user's message, extract the founder's name.
Generate the session slug:
- Format: `YYYY-MM-DD-{ascii-name}` where today's date is from the preamble's `CURRENT_DATE`
- Name rules: lowercase, spaces → hyphens, Arabic names → most common Western Latin spelling
  (e.g., محمد → mohammed, أحمد → ahmed, سارة → sara)
- Examples: `2026-06-26-ahmed-alali`, `2026-06-26-mohammed-hassan`
- Collision: if `~/.waleed-ai/sessions/{slug}/` already exists, append `-2`, `-3`, etc.

Output this line (required for the bash that follows):
```
SLUG: {the-slug-you-generated}
```

### Step 3: Create session folder

Run Bash:
```bash
SLUG="{the-slug}"
SESSION="$HOME/.waleed-ai/sessions/$SLUG"
mkdir -p "$SESSION"
echo "$SESSION" > "$HOME/.waleed-ai/.current-session"
echo "SESSION: $SESSION"
```

### Step 4: Extract structured brief from transcript

Read the transcript carefully. Extract every data point present. Do not infer or fill in
gaps — if information is absent, write "Not disclosed" for that field.

Produce the following document and write it to `$SESSION/01-intake.md`:

---

```markdown
# Founder Brief

**Session:** {slug}
**Date:** {YYYY-MM-DD}
**Interviewer:** Waleed Al-Sanosi, Dreamy

---

## Company Overview

**Company Name:** {name or "Not disclosed"}
**Industry / Vertical:** {sector}
**Stage:** {Idea / Pre-seed / Seed / Series A / Other}
**Incorporated:** {Yes / No / Not disclosed} — {country if known}
**HQ Location:** {city, country}

---

## Founder / Team

**Founder Name:** {full name}
**Background:** {2–4 sentences: prior roles, industries, education}
**Co-founders:** {names and roles, or "None mentioned"}
**Team Size:** {number or "Not disclosed"}
**Notable Hires:** {any key team members mentioned}

---

## Product

**One-line description:** {what it does, for whom, what outcome}
**Problem being solved:** {specific pain point, with evidence if stated}
**Current solution / status:** {MVP / prototype / live / concept}
**Key features mentioned:** {bullet list}
**Technical approach:** {tech stack, AI/ML, platform — if discussed}

---

## Market

**Target customers:** {who, specifically — not "everyone"}
**Geography:** {MENA region focus, specific countries}
**Market size claim:** {exact quote or figure if stated, or "Not discussed"}
**Market sizing methodology:** {bottom-up / top-down / not provided}
**Key competitors named:** {list or "None mentioned"}
**Competitive advantage claimed:** {what the founder said}

---

## Business Model

**Revenue model:** {SaaS / marketplace / transaction fee / subscription / other}
**Pricing:** {if mentioned}
**Target unit economics:** {CAC, LTV, payback — if mentioned}
**Monetization status:** {Pre-revenue / Early revenue / Growing revenue}

---

## Traction

**Current users / customers:** {number or "None / Not disclosed"}
**Revenue:** {ARR/MRR figure or "Pre-revenue"}
**Key milestones achieved:** {bullet list — only what was stated}
**Pilots / LOIs:** {any mentioned}
**Notable partnerships:** {any mentioned}

---

## Ask / Dreamy Engagement

**What they're asking Dreamy for:** {tech co-founding / advisory / product build / unclear}
**Equity offered / discussed:** {percentage or "Not discussed"}
**Timeline expectations:** {launch date, fundraising plans}
**Decision-making authority:** {Is this founder the primary decision maker?}

---

## Key Quotes

{3–5 verbatim or near-verbatim quotes from the transcript that best reveal founder quality,
market insight, or risk. Format as blockquotes.}

---

## Red Flags / Open Questions

{Bullet list of anything that raised concern or requires follow-up. Be direct.
Examples: no market validation, unrealistic timeline, equity structure unclear.
If none, write "None identified."}
```

---

### Step 5: Confirm

After writing the file, print:

```
/intake complete.

Session: {slug}
File: {session path}/01-intake.md

Next: run /evaluate
```

Do not summarize the brief back to the user — they can read the file.
