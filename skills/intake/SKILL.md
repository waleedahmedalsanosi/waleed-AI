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
  - mcp__Tactiq__search_meetings
  - mcp__Tactiq__get_meeting
  - mcp__Tactiq__get_meeting_artifact
  - mcp__Tactiq__list_recent_meetings
---

## When to invoke this skill

Run at the start of every new founder evaluation cycle. Searches Tactiq first for
existing recordings of this founder, then falls back to a pasted transcript or file path.
Produces a structured Founder Brief at `~/.waleed-ai/sessions/{YYYY-MM-DD-founder}/01-intake.md`.

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

### Step 0: Search Tactiq for existing meeting recordings

The user has provided a founder name (e.g., "intake Faisal Al-Harbi"). Before asking for
a transcript, check whether Tactiq already has a recording from this meeting.

1. Extract the founder's name from the user's message (everything after the trigger word "intake").
2. Call `mcp__Tactiq__search_meetings` with:
   - `participants`: the founder's name (first name, last name, or full name — try the most specific form first)
   - `limit`: 10
   - Do NOT put the founder's name in `query` — use `participants` only.

3. If meetings are found, display them clearly:

```
Found {N} Tactiq meeting(s) for {Founder Name}:

  1. {title} — {date} ({duration if available})
     Participants: {participants list}
  2. {title} — {date} ({duration if available})
     Participants: {participants list}
  ...

Which meeting is the intake? (Enter number, or "all" to combine, or "none" to paste transcript)
```

4. Wait for Waleed's response:
   - Number (e.g., "1"): use that meeting
   - "all": pull all listed meetings and combine their content
   - "none" or no Tactiq meetings found: fall through to Step 1 (file/paste)

5. For each selected meeting, call `mcp__Tactiq__get_meeting(id)`:
   - Use `detailedSummary.content` as the transcript content if present
   - If `detailedSummary` is unavailable (requires Team plan), call `mcp__Tactiq__list_meeting_artifacts(meetingId)` and then `mcp__Tactiq__get_meeting_artifact(meetingId, artifactId)` for any summary or notes artifact
   - If no content is retrievable at all, tell Waleed: "Tactiq returned this meeting but its content is not accessible — paste the transcript or provide a file path."

6. If multiple meetings were selected ("all"), concatenate their content in chronological order,
   separating with `--- Meeting {N}: {title} ({date}) ---`.

7. Set `TRANSCRIPT_SOURCE = "Tactiq: {title} ({date})"` for use in Step 4.

---

### Step 1: Determine transcript source (fallback if Tactiq has no match)

Skip this step if Step 0 found and retrieved content from Tactiq.

Look at the user's message. After the trigger word ("intake") and the founder's name:
- If the next token starts with `~/` or `/` — treat it as a file path. Use the Read tool to read that file. That is the transcript.
- Otherwise — the transcript is everything the user pasted in their message after the founder name. Read from the chat context.
- If no file path and no pasted content: ask Waleed to paste the transcript or provide a file path.

Set `TRANSCRIPT_SOURCE = "file: {path}"` or `"pasted text"`.

---

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
**Transcript source:** {TRANSCRIPT_SOURCE from Step 0 or Step 1}

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
Transcript source: {Tactiq meeting title / file path / pasted text}

Next: run /evaluate
```

Do not summarize the brief back to the user — they can read the file.
