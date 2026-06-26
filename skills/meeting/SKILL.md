---
name: waleed-meeting
triggers:
  - meeting
  - second meeting
  - follow-up meeting
  - meeting notes
  - log meeting
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

Run after any additional meeting with a founder whose session is already active.
Searches Tactiq first for a recording of this meeting, then falls back to pasted notes.
Logs the new meeting's content to `{SESSION}/meetings/YYYY-MM-DD.md`.

After logging, re-run `/evaluate` — it will detect the meetings folder and incorporate
the new data. If the score or conditions changed materially, also re-run `/bop` and
`/ceo-review` before sending `/followup`.

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
if [ ! -f "$SESSION/01-intake.md" ]; then
  echo "ERROR: 01-intake.md not found — run /intake first"; exit 1
fi
TODAY=$(date +%Y-%m-%d)
MEETINGS_DIR="$SESSION/meetings"
mkdir -p "$MEETINGS_DIR"
MEETING_FILE="$MEETINGS_DIR/$TODAY.md"
# Count existing meetings
EXISTING=$(ls "$MEETINGS_DIR"/*.md 2>/dev/null | wc -l | tr -d ' ')
MEETING_NUM=$((EXISTING + 2))  # +2 because meeting 1 = the intake
# Extract founder name for Tactiq search
FOUNDER=$(grep "^\*\*Founder Name:\*\*" "$SESSION/01-intake.md" | head -1 | sed 's/\*\*Founder Name:\*\* //')
echo "SESSION: $SESSION"
echo "MEETING_FILE: $MEETING_FILE"
echo "MEETING_NUMBER: $MEETING_NUM"
echo "TODAY: $TODAY"
echo "FOUNDER: $FOUNDER"
```

---

## Instructions

You are running the `/meeting` skill for Waleed Al-Sanosi, PM at Dreamy.

This is not the first meeting — you already have an `01-intake.md` for this founder.
The purpose of this skill is to capture what changed or was learned in this meeting,
and flag what it means for the evaluation.

### Step 1: Read inputs

1. Read `{SESSION}/01-intake.md` to recall the founder, company, and original context.
2. Read any previous meeting notes in `{SESSION}/meetings/` for continuity.

### Step 2: Search Tactiq for this meeting's recording

The founder name is available from the preamble's `FOUNDER` output.

1. Call `mcp__Tactiq__search_meetings` with:
   - `participants`: the founder name from the preamble
   - `limit`: 10
   - Do NOT put the name in `query` — use `participants` only.

2. Filter out meetings already logged: compare meeting dates against existing files in
   `{SESSION}/meetings/`. Only show meetings NOT already logged.

3. If unlogged meetings are found, display them:

```
Found {N} Tactiq meeting(s) with {Founder Name} not yet logged:

  1. {title} — {date} ({duration if available})
     Participants: {participants list}
  2. {title} — {date} ({duration if available})
  ...

Which meeting is this? (Enter number, or "none" to paste notes manually)
```

4. Wait for Waleed's response:
   - Number: use that meeting's content
   - "none" or no Tactiq meetings found: fall through to Step 3 (manual notes)

5. For the selected meeting, call `mcp__Tactiq__get_meeting(id)`:
   - Use `detailedSummary.content` as the meeting content
   - If unavailable, call `mcp__Tactiq__list_meeting_artifacts(meetingId)` then
     `mcp__Tactiq__get_meeting_artifact(meetingId, artifactId)` for any summary artifact
   - If no content retrievable: "Tactiq returned this meeting but content is not accessible — paste notes or key points."

6. Set `MEETING_SOURCE = "Tactiq: {title} ({date})"`.

---

### Step 3: Get meeting notes (fallback if no Tactiq match)

Skip if Step 2 retrieved content from Tactiq.

Get the meeting transcript or notes from the user:
- If the user provides a file path (starts with `~/` or `/`), read that file.
- If the user pastes text, use it directly.
- If nothing provided: ask Waleed to paste notes or key points from the meeting.

Set `MEETING_SOURCE = "pasted notes"` or `"file: {path}"`.

---

### Step 4: Write the meeting note

Write to `{SESSION}/meetings/{TODAY}.md`:

```markdown
# Meeting Note — Meeting {N}

**Session:** {session slug}
**Date:** {today's date}
**Meeting number:** {N} (meeting 1 = original intake)
**Attendees:** Waleed Al-Sanosi + {Founder Name} {+ any others mentioned}
**Source:** {MEETING_SOURCE from Step 2 or Step 3}
**Prepared by:** Waleed Al-Sanosi, Dreamy

---

## Purpose of This Meeting

{1–2 sentences: why did this meeting happen? Follow-up on conditions? Product deep-dive?
Equity negotiation? New co-founder introduced?}

---

## What's New Since Last Meeting

{Bullet list of facts that were NOT in the intake or previous meeting notes.
Be specific — quote the founder where it adds clarity.
If nothing materially new: say so explicitly.}

- {New fact 1}
- {New fact 2}

---

## Conditions Update

{For each condition listed in `11-followup.md` (if it exists), or the conditions from
`03-evaluate.md`, state the current status:}

| Condition | Status | Notes |
|-----------|--------|-------|
| {e.g. Company incorporation} | Met / Not met / In progress | {Evidence from this meeting} |
| {e.g. API feasibility} | Met / Not met / In progress | {Evidence} |
| {e.g. LOI from contractor} | Met / Not met / In progress | {Evidence} |

---

## Score Impact

Based on what was learned in this meeting, flag any evaluation categories that should
be re-scored:

| Category | Original Score | Expected Change | Reason |
|----------|---------------|-----------------|--------|
| {e.g. Business Model} | {X}/15 | +{N} | {reason — e.g. company now incorporated, equity at 30% agreed} |
| {e.g. Traction} | {X}/10 | +{N} | {reason — e.g. signed LOI received from Al-Muqrin Contracting} |

If no scores are expected to change: state "No material change to evaluation scores."

---

## Red Flags or Concerns

{New concerns that emerged in this meeting. If none: "None identified."}

---

## Open Items for Next Meeting

{What's still unresolved? What does Waleed need to follow up on?}

1. {Item}
2. {Item}

---

## Key Quotes

{1–3 direct quotes from the founder that are significant. Arabic where spoken in Arabic.}

> "{quote}"

---

## Recommended Next Actions

{What should Waleed do next based on this meeting?}

- **If conditions are now all met:** Re-run `/evaluate` → `/bop` → `/ceo-review` → `/followup` (Proceed)
- **If some conditions are met:** Re-run `/evaluate` to update the score, then `/brief` for next call
- **If no progress on conditions:** Decide whether to extend the 30-day window or close the file

{Specific recommendation for this case:}
{e.g. "Faisal confirmed incorporation is complete. Re-run /evaluate then /ceo-review to update Samir before sending a Proceed followup."}
```

---

### Step 5: Confirm

After writing the file, print:

```
/meeting complete.

File: {session path}/meetings/{today's date}.md
Meeting number: {N}
Source: {Tactiq meeting title / file path / pasted notes}

Conditions status:
  {condition 1}: {Met / Not met / In progress}
  {condition 2}: {Met / Not met / In progress}

Next:
  → Re-run /evaluate to incorporate this meeting into the score
  → If score changed materially: re-run /bop and /ceo-review
  → Then /followup if ready to send updated correspondence
```
