---
name: waleed-evaluate
triggers:
  - evaluate
  - score
  - assessment
allowed-tools:
  - Read
  - Write
  - Bash
---

## When to invoke this skill

Run after `/intake`. Reads the structured Founder Brief (`01-intake.md`) and the
evaluation framework rubric, then produces a scored 100-point assessment at
`02-evaluate.md` with a Proceed / Conditional / Pass recommendation.

Never run this on a raw transcript. Run it on the structured brief from `/intake`.

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
INTAKE="$SESSION/01-intake.md"
if [ ! -f "$INTAKE" ]; then
  echo "ERROR: 01-intake.md not found — run /intake first"; exit 1
fi
FIRST_LINE=$(head -1 "$INTAKE" 2>/dev/null)
if [ "$FIRST_LINE" != "# Founder Brief" ]; then
  echo "ERROR: 01-intake.md is empty or malformed — re-run /intake"; exit 1
fi
echo "SESSION: $SESSION"
echo "INTAKE: OK"
# Check for additional meeting notes
MEETINGS_DIR="$SESSION/meetings"
if [ -d "$MEETINGS_DIR" ] && [ "$(ls -A "$MEETINGS_DIR" 2>/dev/null)" ]; then
  MEETING_COUNT=$(ls "$MEETINGS_DIR"/*.md 2>/dev/null | wc -l | tr -d ' ')
  echo "MEETINGS: $MEETING_COUNT additional meeting note(s) found — will be incorporated into evaluation"
else
  echo "MEETINGS: none"
fi
```

---

## Instructions

You are running the `/evaluate` skill for Waleed Al-Sanosi, PM at Dreamy.

### Step 1: Read inputs

1. Read `01-intake.md` from the current session (path from SESSION in preamble).
2. Read `~/.claude/skills/waleed-ai/lib/evaluation-framework.md` — this is the scoring rubric.
3. If `MEETINGS` output from the preamble shows additional meeting notes, read each file in `{SESSION}/meetings/` and incorporate that information into scoring. Meeting notes update your view of the founder — they do not replace the intake. If a meeting resolves a red flag from intake (e.g., company is now incorporated), that should raise the relevant sub-criterion score and be noted explicitly.

### Step 2: Score each category

Work through each of the 6 categories in the framework. For each:
- Read the sub-criteria carefully.
- Score each sub-criterion based ONLY on evidence present in `01-intake.md`.
- Do not infer, extrapolate, or give benefit of the doubt. If data is absent, score 0.
- Sum sub-criteria scores to get the category total.
- Write a one-line Rationale citing the specific evidence (or lack thereof).

### Step 3: Check for sparse data

After computing the TOTAL:
- If TOTAL < 30, you MUST include this line in the output:
  `WARNING: Score below 30 — falls in Pass range. Either the intake data is too sparse to evaluate fairly, or this is a genuine Pass. Do not proceed without a follow-up call to fill data gaps.`
- If TOTAL is 30–54 (Watch range), note in the recommendation that Watch means "do not proceed now — revisit if conditions improve" — it is not an active engagement.

### Step 4: Determine Recommendation

Use the scoring interpretation table from the framework:
- 80–100 → Proceed
- 55–79 → Conditional
- 30–54 → Watch
- 0–29 → Pass

For Conditional or Watch: list 2–4 specific conditions or gaps that must be addressed.

### Step 5: Write 02-evaluate.md

Write to `{SESSION}/02-evaluate.md`:

```markdown
# Evaluation Report

**Session:** {slug from 01-intake.md}
**Date:** {today's date}
**Evaluator:** Waleed Al-Sanosi, Dreamy

---

## Scoring Summary

| Category | Max | Score | Rationale |
|----------|-----|-------|-----------|
| Founder / Team | 25 | {X} | {one-line evidence cite} |
| Market | 20 | {X} | {one-line evidence cite} |
| Product | 20 | {X} | {one-line evidence cite} |
| Business Model | 15 | {X} | {one-line evidence cite} |
| Traction | 10 | {X} | {one-line evidence cite} |
| Dreamy Fit | 10 | {X} | {one-line evidence cite} |
| **TOTAL** | **100** | **{X}** | |

{WARNING line here if TOTAL < 30}

---

## Category Detail

### Founder / Team ({score}/25)

| Sub-criterion | Max | Score | Notes |
|---------------|-----|-------|-------|
| Domain Expertise | 10 | {X} | {evidence or "No data"} |
| Execution History | 10 | {X} | {evidence or "No data"} |
| Coachability / Dreamy Fit | 5 | {X} | {evidence or "No data"} |

**Analysis:** {2–3 sentences on founder quality — specific, direct, no filler}

### Market ({score}/20)

| Sub-criterion | Max | Score | Notes |
|---------------|-----|-------|-------|
| Market Size | 8 | {X} | {evidence or "No data"} |
| Market Timing | 6 | {X} | {evidence or "No data"} |
| Regional Dynamics | 6 | {X} | {evidence or "No data"} |

**Analysis:** {2–3 sentences on market opportunity}

### Product ({score}/20)

| Sub-criterion | Max | Score | Notes |
|---------------|-----|-------|-------|
| Problem Clarity | 7 | {X} | {evidence or "No data"} |
| Solution Differentiation | 7 | {X} | {evidence or "No data"} |
| Technical Feasibility | 6 | {X} | {evidence or "No data"} |

**Analysis:** {2–3 sentences on product strength and build risk}

### Business Model ({score}/15)

| Sub-criterion | Max | Score | Notes |
|---------------|-----|-------|-------|
| Revenue Model | 6 | {X} | {evidence or "No data"} |
| Unit Economics | 5 | {X} | {evidence or "No data"} |
| Dreamy Equity Model Fit | 4 | {X} | {evidence or "No data"} |

**Analysis:** {2–3 sentences on monetization and Dreamy model fit}

### Traction ({score}/10)

| Sub-criterion | Max | Score | Notes |
|---------------|-----|-------|-------|
| Evidence of Demand | 5 | {X} | {evidence or "No data"} |
| Momentum | 5 | {X} | {evidence or "No data"} |

**Analysis:** {1–2 sentences on traction quality}

### Dreamy Fit ({score}/10)

| Sub-criterion | Max | Score | Notes |
|---------------|-----|-------|-------|
| Co-founding Model Compatibility | 5 | {X} | {evidence or "No data"} |
| Bandwidth / Timing | 5 | {X} | {evidence or "No data"} |

**Analysis:** {1–2 sentences on fit}

---

## Key Strengths

{3 bullet points — specific, evidence-based, no generic praise}

## Key Risks

{3 bullet points — specific risks that affect the Dreamy co-founding decision}

## Conditions (if Conditional or Watch)

{Numbered list of specific conditions. Omit section if Proceed or Pass.}

---

## Recommendation

**{Proceed / Conditional / Watch / Pass}**

{2–3 sentences explaining the decision. Name the deciding factor — the single most
important thing that swung the recommendation. Be direct.}
```

---

### Step 6: Confirm

After writing the file, print:

```
/evaluate complete.

Score: {X}/100
Recommendation: {Proceed / Conditional / Watch / Pass}
File: {session path}/02-evaluate.md

Next: run /bop
```
